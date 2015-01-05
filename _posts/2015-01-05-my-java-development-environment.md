---
layout: post
title: "My Java Development Environment"
description: ""
category: Project Implementation Programming Languages
tags: [Java, Java EE, Development Environment]
---
{% include JB/setup %}


# My Java Development Environment
Full disclosure, I've only been writing [Java](http://en.wikipedia.org/wiki/Java_%28programming_language%29) for about 2 and a half months.
Prior to that I had been doing [C](http://c.learncodethehardway.org/) for like a year and had done [Ruby](https://www.ruby-lang.org/en/) before that.
I had to learn Java because I had the task of building a web service and it had
to be built in this language because the infrastructure that the folks I was
building it for had dictated so. One of the developers that I was liasing with
suggested I get comfortable with the [netbeans IDE](https://netbeans.org/) but this left a bit of a
bitter test in my mouth. All my programming life I had learned to avoid [IDEs](http://en.wikipedia.org/wiki/Integrated_development_environment)
like the plague. I wanted a development environment that gave me the freedom to
use whatever development tool I was comfortable with and not have some bundle
forced on me. Also IDEs tend to hide a lot of stuff from me and they are bloody
resource hogs. You see I don't develop on a 16G RAM box so I tend to be nervous
about anything that wastes my precious memory. Nevertheless, I gave netbeans a
chance and tried to stay as open minded as I could but that relationship wasn't
going to get anywhere. So I decided to do it the hardway like I always do
whenever am learning a new language. I look at the various aspects of
development in the selected language. These aspects are always different
depending on the way the programming language was designed.
For Java I decided my development phases would be editing, compiling, testing,
packaging and of course keeping the projects under version control management.
What follows is a specification of the choices I've decided to work with so far
and possibly the reasons why. These choices are a result of my current stage in
developing in Java and will most definitely change in the future as I learn
more. I also must add that every time am learning a new language I look at the
available open source projects and see the conventions
that the community likes to use, [Github](https://github.com/) and [Bitbucket](https://bitbucket.org/)
make this litle exercise super trivial. I do a lot of community investigation
before I even write the first [hello world](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program).
My choices have further been guided by this investigation.

## Editor
I started with [vim](http://www.vim.org/). I had javac.vim turned off since
it was constantly whining that it couldn't resolve my imports because of my
classpath setting. Vim was good but I needed to create a visual difference
between my C and Ruby code editors so I've since switched to sublime for my Java
development and I left vim for the other languages that I prefer to use it for.
So for now my editor of choice, for Java is Sublime. In the no-so-far future I
plan to switch to [Intellij](https://www.jetbrains.com/idea/) for the simple
fact that it's a beautiful, smart and fast IDE. Yes, I notice am flip/flopping
on my statement of not being an IDE fan but am a Software Engineer and we use
the best tool for the job even if the tool might not be your favorite ;-).

## Building and Dependency Management
I started with [ant](http://ant.apache.org/) because of its resemblance to C's
make in imperativeness. However as with all things imperatively programmed it
became really unsustainable for bigger projects as the LOCs tend to go so high
that they start interfering with with development flow. Looking through the OSS
java projects on the Internet I noticed that [maven](http://maven.apache.org/)
had been sort of set up as the defacto choice for all massive projects. I also
like the fact that its sort of [Convention over Configuration](http://en.wikipedia.org/wiki/Convention_over_configuration)
and so I don't have to spend a lot of time re-writing the same logic over and
over again like I was doing with ant. So maven is now my build tool of choice.
Maven is also amazing at dependency management. With ant I had to manually
download all the jars and add them to my classpath. Now I just specifiy all the
external dependencies as such in the pom.xml file and viola!

## Testing Framework
[Junit](http://junit.org/) is the little test framework that I've so far
learned to use, one could argue that that choice was a no brainer as Junit bears
a strong resemblance Ruby's own MiniTest. I also adopted it because I was
familiar with the people who built it and trust the design decisions they make.

## Enterprise EE Framework
And lastly enterprise application development. Initially I used [java ee](http://www.oracle.com/technetwork/java/javaee/overview/index.html)
libraries straight up to build web services and applications. That again was
like trying to write build rules with ant yet there's something like maven. I
like “niche frameworks”. In the Ruby world I use [Rails](http://rubyonrails.org/)
for web application development. I use the [Qt](http://qt-project.org/)
framework with GUI development. And so on. In the Java world I've found that
the [Spring framework](http://spring.io/) makes development with Java EE so not
dreary. Spring makes building Java EE applications something one can look
forward to like a 6 year-old can't wait for December 24th to get done so he can
unwrap his gift.

## Books that have helped me along the way:

- Effective Java, second Edition. Written by Joshua Bloch
- The Java Language Specification, Java se 8 edition. Written by James Gosling,
  Bill Joy, Guy Steele, Gilad Bracha and Alex Buckley
- Java 8 in action
- Spring in Action
- The Java EE 7 Tutorial, Release 7, for Java EE Platform
- Junit tutorial
- maven tutorial
- SOA using Java Web services, by Mark D Hansen
- Proffesional Java development with the Spring framework, by Rod Johnson et al.
- Algorithms, Fourth Edition. Written by Robert Sedgewick and Kevin Wayne.
  A book that teaches implementation of general algorithms in the Java language.
