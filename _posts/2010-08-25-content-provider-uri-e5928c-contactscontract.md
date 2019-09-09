---
id: 227
title: Content Provider, Uri 和 ContactsContract
date: 2010-08-25T03:24:35+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.info/?p=227
permalink: '/2010/08/25/content-provider-uri-%e5%92%8c-contactscontract/'
original_post_id:
  - "227"
  - "1083"
categories:
  - 技术
tags:
  - android
---
我的需求是从电话地址簿里面选择联系人，导入他的姓名和地址。正好<a href="http://www.huangshifu.net/" target="_blank" rel="noopener noreferrer">同事黄师傅</a>show了一个他写的sample程序，里面有选择联系人的功能，于是借过来用：

<p style="padding-left:30px;">
  Intent intent = new Intent(Intent.ACTION_PICK,ContactsContract.Contacts.CONTENT_URI);<br /> startActivityForResult(intent, PICK_CONTACT);
</p>

在onActivityResult方法里，我用下面代码拿到被选中的那个联系人的数据：

<p style="padding-left:30px;">
  Uri uri = data.getData();<br /> ContentResolver resolver = getContentResolver();<br /> Cursor cursor = resolver.query(uri, null, null, null, null);<br /> if (cursor.moveToFirst()) {<br /> int idIdx = cursor.getColumnIndexOrThrow(ContactsContract.Contacts._ID);<br /> long id = cursor.getLong(idIdx);<br /> String name = cursor.getString(cursor.getColumnIndex(ContactsContract.Data.DISPLAY_NAME));<br /> System.out.println(&#8220;user name=&#8221;+name);<br /> }
</p>

到此为止，名字我能正确拿到的。接下来我想拿email，在ContactsContract.Data类里面找EMAIL常量，结果没找到。有点困惑。然后我把cursor的所有column名字和值都遍历一遍打了出来，心想email无论躲在哪个字段里，我都能找到。结果仍然没有，所有字段多半不是null就是数值，没有我填写的email那个值。只有一个叫lookup的字段，里面的值是hashcode，但这个我觉得也不像。

在网上搜了一下，没有答案。后来再把书中有关Content Provider和Uri的概念原理读了一下，再结合官方文档里面的一些描述，我开始有了点眉目。再做了一下实验，终于把email拿到了。

首先要用我不太严谨的语言讲一下Content Provider的机制。其实书里面已经讲得清楚，只是未经实践始终是视而不见。所有Content Provider都会在系统注册一个Uri，而这个Uri就是以“content://”开头的一个字符串，你只要注册了这个Content Provider类，里面指定处理特定模式的字符串，那么发到这个Uri模式下的请求就会交给这个Provider来响应。系统本身内置了一些Content Provider，例如地址簿就是一个。地址簿的Uri是约定的，定义在API的常量里（ContactsContract.Contacts.CONTENT_URI），它实际的值是：content://com.android.contacts/contacts

这是Content Provider和Uri的机制，它只定义了什么拦截处理的机制，对于Content的内部结构，其实是没任何规定的，可以自由组织。简单来讲就是不一定非要是关系数据库不可！这也是我开始搞错的重要原因。

读一下<a href="http://developer.android.com/reference/android/provider/ContactsContract.html" target="_blank" rel="noopener noreferrer">ContactsContract的API文档</a>就知道（btw这些个名字起得让我有点崩溃）：联系人信息是一个**三层的数据模型**。包括Data、RawContacts和Contacts这三个表（此外还有一些其它的扩展表）。简单讲就是每个联系人，都会在这三个表里面有记录。这个设计有点复杂，但初衷也是可以理解的，如果它的数据结构是定义在一个表里那样写得死死的，那怎么能支持复杂的自定义字段？

这三个表里面，联系人ID是共享的（当然了，这是它们联系起来的依据），因此关键是拿到联系人ID。在选择了联系人返回的时候，我们已经可以用下面代码拿到联系人ID：

<p style="padding-left:30px;">
  int idIdx = cursor.getColumnIndexOrThrow(ContactsContract.Contacts._ID);<br /> long id = cursor.getLong(idIdx);
</p>

这个id可以拿来访问DATA表，获取用户的Email信息，代码如下：

<p style="padding-left:30px;">
  Cursor mailCursor=resolver.query(ContactsContract.CommonDataKinds.Email.CONTENT_URI, null, ContactsContract.CommonDataKinds.Email.CONTACT_ID+&#8221;=?&#8221;, new String[]{Long.toString(id)}, null);<br /> if (mailCursor.moveToFirst()) {<br /> do {<br /> String mail=mailCursor.getString(mailCursor.getColumnIndexOrThrow(Email.DATA));<br /> System.out.println(&#8220;email=&#8221;+mail);<br /> } while (mailCursor.moveToNext());<br /> }
</p>

上面关键之处是用ContactsContract.CommonDataKinds.Email.CONTENT_URI这个Uri（实际值为content://com.android.contacts/data/emails）以及既定的联系人ID约束条件来打开了一个新Cursor。从这个Uri可以看到，它访问的是data表，而不是上面的cursor访问的contacts表。在data表里面，它访问的还是emails这个维度里面的数据，里面可能有多条（例如家庭email地址、工作email地址）。

除了邮件，ContactsContact下面还定义了很多CommonDataKinds，如下图：

[<img class="alignleft size-medium wp-image-228" title="CommonDataKinds" src="http://www.raymondchen.info/wp-content/uploads/2010/08/commondatakind-300x193.jpg" alt="" width="300" height="193" />](http://www.raymondchen.info/wp-content/uploads/2010/08/commondatakind.jpg)

这样设计是基于扩展的考虑，但无疑也更加复杂了。具体设计的理由我暂时还未很好理解，不过只要把握住拿邮件地址这个方法，拿其它信息也可以采取同样方式。