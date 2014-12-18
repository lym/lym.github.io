---
layout: post
title: "Modularity in C"
description: ""
category: Architecture
tags: [Modularity, Software Engineering, Project Management, Programming Languages, C]
---
{% include JB/setup %}

# Modularity in C
First of all the unofficial unit of modularity in any programming
language is a file, a text file that is. By modularity I mean the
division of program functionality/behaviour/services into
manageable/sharable/reusable chunks.

C's unit of modularity is a function. So if your program is
architecturely divided into modules, your modules would have to be
spread across respective functions probably named after the
architectural entities. Now each of these functions goes into a `*.c`
file. Every one such file should explicitly document the purpose of the
function contained therein. It's important to just stick to stating the
purpose and perhaps usage and staying away from "how" the function
works. These modules should ideally not be named "main".

Your *program file* should then just have one function called `main` that
explicitly documents the purpose of your program. You could think of the
main function as the one in charge of the inter-module communication.

Now, after splitting the program into this multitude of modules, all the
function prototypes of these modules and put into a `*.h` file which is
then imported into the program file through the `#include` directive.

After this separation you can then continue with the mudular
decomposition following the above guidelines.
