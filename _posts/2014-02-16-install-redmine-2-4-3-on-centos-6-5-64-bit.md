---
id: 895
title: Install Redmine 2.4.3 on CentOS 6.5 64-bit
date: 2014-02-16T18:08:46+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=895
permalink: /2014/02/16/install-redmine-2-4-3-on-centos-6-5-64-bit/
original_post_id:
  - "895"
  - "1083"
categories:
  - Tech
---
Ruby is not good with dependency management, especially different versions come into play.

With that said, I still need to install Redmine 2.4.3 on my CentOS 6.5 virtual machine. I tried Bitnami first, but it failed, which was a little bit unexpected. Since Bitnami used to help me so much in avoiding the installation nightmare of Ruby software. Anyway, I guess the CentOS 6.5 is just too new.

Install steps are listed below:

> 1. yum install mysql-server mysql-devel ImageMagick ImageMagick-devel ruby ruby-devel rubygems, rubygems-devel  
> 2. Install rvm in shell:  curl -sSL https://get.rvm.io | bash -s stable  
> 3. rvm install 1.9.3 ; rvm use 1.9.3  
> 4. gem install rails rmagick mysql2 bundler  
> 5. Start MySQL service. According to redmine&#8217;s online installation manual, create database, grant user privileges and change the database.yml in redmine config directory.  
> 6. Install redmine bundle： bundle install &#8211;without development test  
> 7. rake generate\_secret\_token  
> 8. RAILS_ENV=production rake db:migrate  
> 9. RAILS\_ENV=production rake redmine:load\_default_data  
> 10. Add linux user and group: useradd redmine  
> mkdir -p tmp tmp/pdf public/plugin_assets  
> sudo chown -R redmine:redmine files log tmp public/plugin_assets  
> sudo chmod -R 755 files log tmp public/plugin_assets  
> 11. Start it： ruby script/rails server webrick -e production