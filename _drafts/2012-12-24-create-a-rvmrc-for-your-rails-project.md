---
layout: post
title: "Create a .rvmrc for your rails project"
description: ""
category: 
tags: ["rvm", "ruby"]
---
{% include JB/setup %}
<p>stack:</p>
`rails, ruby, bash, rvm`
==
I've been working on a project that has strict ruby version
requirements, 1.9.3 and higher or it won't run. So basically every time
I get to my app's document root I have to keep typing:
<pre><code> $ rvm use 1.9.3 </code></pre>
After doing this for a while, of course this behaviour smells so bad
that there had to be a better way. Turns out the good folks who make
[rvm](https://rvm.io) had my back. To spare yourself the laborious
activity of having to type the same command everytime you want to work
with your application, you can create a simple script that gets run on
dropping to your application's root, a .rvmrc. This file is basically a
shell script and you can put any commands that you want executed on
accessing your application's root directory, but for my case all I want is to have a
gemset created. 
So what I did was create a file named .rvmrc at the root of the document
and entering the following line:
<pre><code> rvm use 1.9.3 </code></pre>
that basically says that use ruby 1.9.3 for this project.
And that's it. Now everytime to go the your project's root, I assume
you're interacting with you app through a shell emulator of course, your
application will run with ruby 1.9.3. This is extremely helpful if you
multiple projects that use different ruby versions.
