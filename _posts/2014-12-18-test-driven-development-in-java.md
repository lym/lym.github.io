---
layout: post
title: "Test Driven Development in Java"
description: ""
category: 
tags: [Code Quality, Productivity, Efficiency]
---
{% include JB/setup %}

# Test Driven Development with in Java
In this post am going to be talking about using the [junit](http://junit.org/)
unit testing framework for java.

# What you need
- junit jar
- hamcrest jar

# Test set up
Write you're test classes with test methods annotated with `@Test`, a
junit annotation that identifies test methods.

Write you test runner class, the class the runs you're entire test
suite.

Compile the test classes into class files. Run your entire test suite by
running just the test class.
