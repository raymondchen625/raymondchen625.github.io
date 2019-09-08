---
id: 1497
title: Jenkins Running in Docker
date: 2017-12-29T13:26:42+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1497
permalink: /2017/12/29/jenkins-running-in-docker/
geo_public:
  - "0"
categories:
  - Tech
---
I have a study Java project with CI running on a Jenkins on a AWS EC2 instance. I have been switching to Google Compute Engine lately so I had the chance to refactor it a little bit while I was doing it.

One main purpose for the refactoring is to put everything in Docker container. So if I ever need to move again, it would be easier.

My old plan was to write a docker file to build MySQL and Jenkins inside. But later I realized that was too coupling. The DB can be independent of the Jenkins. So my docker file extends the Jenkins image and only install mysql client in it, since I need to run some SQL script with it in my project.

On my GCE instance, the only thing I need is docker engine. I run a docker command to start a standard MySQL container, and start a Jenkins container from my own image and link it to the MySQL container. That&#8217; it!

[<img class="size-medium wp-image-1498 alignright" src="http://localhost/wp-content/uploads/2017/12/ci.png?w=300" alt="" width="300" height="142" srcset="http://localhost/wp-content/uploads/2017/12/ci.png 636w, http://localhost/wp-content/uploads/2017/12/ci-300x142.png 300w" sizes="(max-width: 300px) 100vw, 300px" />](http://localhost/wp-content/uploads/2017/12/ci.png)For continuous integration I always think it&#8217;s better to put everything inside the project on SCM. So I even put the scripts above in the project folder.

Once Jenkins up, all I need to do is:

  1. Change the password and create account, of course;
  2. Configure the JDK and Maven tool globally
  3. Define the project and point it to my Bitbucket project URL to poll changes

That&#8217;s it. The pipeline is defined in the Jenkinsfile of my project. It checks out the code branch, creates a temporary db and populates the db schema, modifies some resource file to point to the temp db, runs the junit tests and generates reports, sends the notification to my pushover app and cleans up the db. All is defined in the Jenkins and the scripts in the project.

If I need to move again, all I need to repeat is the steps 1-3 above.