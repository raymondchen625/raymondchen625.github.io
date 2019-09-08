---
id: 265
title: Sqlite user_version
date: 2010-08-13T10:19:01+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=265
permalink: /2010/08/13/sqlite-user_version/
original_post_id:
  - "265"
  - "1083"
categories:
  - Tech
---
After the long and frustrating bug tracing of my android program, I think I&#8217;ve some idea about how sqlite and android control the version of its database schema.

At first, I guessed the version number is table-basis, i.e. you specify a version number for each table, and the android program will check the version number to decide whether this table needs to be altered. I was wrong. The android program use the Pragma, user_version, to do the version tracking. Since the db is exclusive to each android app, so this parameter is used to reflect the db schema version of each app.

Let&#8217;s see the whole process. When the SqliteOpenHelper class creates the table, it receives a version value and set it to the pragma &#8220;user_version&#8221;. Next time it receives another version number, it checks it with the existing version, which is retreived from that pragma. If the existing version is lower, it will call the appropriate method to alter the table, then update the existing version. So when any table wants to change its schema, it needs to specify a version number higher than the current one.

That&#8217;s pretty straightford, huh? It is until we need to modify multiple tables. Of course it depends on the approach we use to upgrade our db schemas. In my case, I create a helper class for each table. So if I wants to upgrade two tables and provide the same version number for both of them, the first one got upgraded will set the version number after the finish of upgrade. That&#8217;ll prohibit the 2nd table from upgrading, since the two version number are equivalent now.