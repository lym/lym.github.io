---
layout: post
title: "Manually Create Mongo Database. Well, sort of"
description: ""
category: 
tags: ['databases', 'NoSQL']
---
{% include JB/setup %}
I have been working on rails applications backed by the traditional SQL
databases, postgresql, to be more particular.
The workflow went something like this:
- Generate new rails app
- Add pg gem to Gemfile
- Create all three databases; development, test and production.
- Edit database.yaml and then be on my way.

A couple of days back I was building a new application that implemented
a NoSQL database from scratch, MongoDB. So I went about installing the
database, the ODM and generally got rid of anything that had to do with
ActiveRecord since I was using mongoid. 

So I went through step 1, at step 2 i switched pg for mongoid in the
Gemfile, step 3 explains the purpose of this blog entry. I had never
used this type of database so I decided to google, "How to create a
database in MongoDB", boy was I in for a shock.
It turns out, for this kind of database, you don't need to explicitly
set up an empty database. A database is created dymically when you
insert data into it. 

It's actually fun, you just pop up mongo, the console utility that comes
with mongoDB think psql if you've been using postgresql. After it
successfully starts up, you can run
<pre><code>show dbs</code></pre>
this command lists all the available databases. To insert into and
thereby create a new one you just type
<pre><code>use your_desired_database_name</code></pre>
you can then insert document, what we refer to in sql as a row as
follows:
<pre><code>db.collection_name.save( key:'value', key2: 'value2',.......)</code></pre>
If the above operation is successful, the next time you run
    <pre><code>show dbs</code></pre>
you should see your new database listed.
Pretty neat huh?

Below is an example I ran for my commandline:
<pre><code>
    lym@salym-dev-box:~$ mongo
    MongoDB shell version: 2.2.2
    connecting to: test
    > use rails3_mongoid_devise_development
    switched to db rails3_mongoid_devise_development
    > db.users.save( { name:'First User', email: 'user@example.com', password: 'please', password_confirmation: 'please' } )
    > db.users.find()
    { "_id" : ObjectId("50d08c148b3ed9a3c52a93af"), "name" : "First User", "email" : "user@example.com", "password" : "please", "password_confirmation" : "please" }
    > show dbs
    local	(empty)
    rails3_mongoid_devise_development	0.0625GB
    test	0.0625GB
    > exit
    bye
</code></pre>
