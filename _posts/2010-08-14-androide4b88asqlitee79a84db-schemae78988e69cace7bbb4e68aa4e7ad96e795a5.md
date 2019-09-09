---
id: 207
title: Android上SQLite的DB Schema版本维护策略
date: 2010-08-14T08:53:55+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.info/?p=207
permalink: '/2010/08/14/android%e4%b8%8asqlite%e7%9a%84db-schema%e7%89%88%e6%9c%ac%e7%bb%b4%e6%8a%a4%e7%ad%96%e7%95%a5/'
original_post_id:
  - "207"
  - "1083"
categories:
  - 技术
---
过去10多年的开发生涯中，基本上都是与数据库打交道。从来没遇过这次搞Android上面的SQLite数据库遇到的问题那么奇怪。后来我明白了：因为以前起搞的都是服务器端编程，DB其实都在一个地方，改数据库结构，都是前后两个版本之间的事，更多关注点都是在如何保证改表对正常服务的冲击减到最小。

而在移动开发上，我们需要考虑的是外面可能有无数个旧版本并存，它们都同时需要升级到最新版本。分别要执行的操作都可能不一样，尤其在要保留用户旧数据的情况下。

在我写的这个Android程序里，用到了SQLite数据库来保存数据。第一次写，所以数据访问的代码是抄<a href="http://www.amazon.com/Professional-Android-Application-Development-Programmer/dp/0470565527" target="_blank" rel="noopener noreferrer">书上的</a>，用了<a href="http://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html" target="_blank" rel="noopener noreferrer">SQLiteOpenHelper</a>这个辅助类。基本方法是：

  1. 为每个表都创建一个Adapter类；
  2. 每个这样的Adapter类里面再定义一个继承自SQLiteOpenHelper的内部类。这个类主要帮助包含它的Adapter类执行打开数据库，运行sql的工作。它覆盖了两个关键的方法：onCreate和onUpgrade。分别在DB未创建和DB升级时被回调。

详细的解释可以参考网上资料，可以搜到一大堆，例如<a href="http://code.google.com/p/androidlearn/wiki/SQLiteOpenHelper" target="_self" rel="noopener noreferrer">这个</a>。

在应用只有一个表的时候，用起来是没太大问题的。后来多建了几个表，用着发现数据经常神秘地丢失，有时表又不会自动创建。后来经过多番查找资料，才明白了其中的机制。

首先，DB Schema的版本是针对整个数据库的（即针对整个应用，对于不共享数据库的情况来讲），而并非针对一个表的。它其实保存在SQLite数据库的pragma中，变量名为user_version，用命令行进去就可以看到当前的值。

其次，SQLiteOpenHelper对象被构建时，总是要接受一个version参数，这个参数代表最新版本号。SQLiteOpenHelper会将它和数据库中当前的值对比，如果有差异，就会调用onUpgrade方法，并且把这个数据库的user_version设置为最新的值。

问题就出现在这里，当我有多个数据表、多个Adapter的时候，第一个表被访问时，SQLiteOpenHelper已经将数据库的版本号更新为最新值。当第二、第三个表去访问的时候，就会发现数据库版本已经是最新，就不会调用onUpgrade方法。因此如果这个版本同时改了两个表，就会出错。

究其原因，觉得DB Schema的版本控制代码是全局的，根本不应该散落在各个地方，应该有一个地方中央控制。于是我作了一些改动：

  * 把所有Adapter的内部类的两个方法onCreate和onUpgrade都设置为什么都不做：

@Override  
public void onCreate(SQLiteDatabase _db) {  
System.out.println(&#8220;do nothing&#8221;);  
}

@Override  
public void onUpgrade(SQLiteDatabase \_db, int \_oldVersion,  
int _newVersion) {  
System.out.println(&#8220;Per-table upgrade is disabled&#8221;);  
}

  * 创建一个新的类DbUtil来专门负责维护DB Schema的版本。

DbUtil负责记录当前最新的版本号，它有一个静态方法checkDbSchemaVersion(Context context)，每次主程序的Activity启动的时候，都会在onCreate里面首先调用一下它来检查数据库版本。这个方法里面包含了一个继承自SQLiteOpenHelper的内部类，和以前的那些Adapter的内部类相似，它也是覆盖了onCreate和onUpgrade方法。onCreate方法的实现比较简单，它就是简单地执行了所有表的create语句。而onUpgrade方法中，我让它调用updateDbSchemaVersion(oldVersion, newVersion)，这里的oldVersion是程序在当前运行的数据库里面拿到的版本值，可能是几个版本之前的值；newVersion是本程序设置的最新版本值。

upgradeDbSchemaVersion方法的实现也比较简单，它的作用是根据新旧版本的差异，将两版本之间的SQL语句全部按顺序执行一次，源码如下：

private static void updateDbSchemaVersion(int oldVersion, int latestVersion) {  
if (oldVersion>=latestVersion) return;  
System.out.println(&#8220;Upgrading DB schema from &#8220;+oldVersion +&#8221; to &#8220;+(oldVersion+1));  
switch (oldVersion) {  
case 2:  
break;  
case 3:  
break;  
}

oldVersion++;  
updateDbSchemaVersion(oldVersion,latestVersion);  
}

可以看到上面的程序是一个递归，它是将程序从2开始，将每个版本与前一版本之间的升级操作写在对应的case语句块中。无论当前当前的数据库版本是多少，总是可以在里面找到合适的位置，然后开始逐个版本升级，直到更新到最新版本为止。

需要注意的是，当程序是新安装（不是升级），也就是数据库不存在的情况下，DbUtil中的SQLiteOpenHelper的onCreate会被回调，但onUpgrade不会被回调，数据版本会即时更新为最新的。因此需要保证onCreate里面执行出来的DB Schema是最新的。所以每个版本除了要写updateDbSchemaVersion中的case，还要确保CREATE语句都是对的，建表没有建漏。

当然把程序再优化一下，把onCreate和onUpgrade整合，也是可以的，能做到严格按照版本升级的时间线来走一遍这些DDL。现在表不多，暂且偷一下懒吧。