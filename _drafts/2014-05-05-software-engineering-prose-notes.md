---
layout: post
title: "Software Engineering prose notes"
description: "Unfiltered notes"
category: "Software Engineering"
tags: ["programming", "Software Engineering"]
---
{% include JB/setup %}

## Attributes and routines
Any abstract data type such as POINT is characterized by a set  of
functions, describing the operations applicable to instances of the
ADT. 

In classes (ADT implementations), functions will yield features - the
operations applicable to instances of the class.

We have seen that ADT functions are of 3 kinds:
 - Queries
 - Commands and
 - Creators.

For features, we need a complimentary classification, based on how each
feature is implemented: by space or by time.

• Some features will be represented by space, that is to say by
associating a certain piece of information with every instance of the
class. They will be called attributes. For points, x and y are
attributes in cartesian representation; rho and theta are attributes
in polar representation.

• Some features will be represented by time, that is to say by
defining a certain computation (an algorithm) applicable to all
instances of the class. They will be called routines. For points, rho
and theta are routines in cartesian representation; x and y are
routines in polar representation.

A further distinction affects routines (the second of these
categories). Some routines will return a result; they are called
functions. Here x and y in polar representation, as well as rho and
theta in cartesian representation, are functions since they return a
result, of type REAL. Routines which do not return a result
correspond to the commands of an ADT specification and are called
procedures. For example the class POINT will include procedures
translate, rotate and scale.


To novices, this is often the most disconcerting aspect of the
method. In object-oriented software construction, we never really
ask: “Apply this operation to these objects”. Instead we say: “Apply
this operation to this object here.” And perhaps (in the second
form): “Oh, by the way, I almost forgot, you will need those values
there as arguments”.


Current means: “the target of the current call”. 

## Uniform Type System
Basic types such as BOOLEAN and INTEGER are handled in the same way
as user-defined types.


Memory addresses are just one way of implementing the reference type.

## Note
In block-structured languages, object allocation occurs at the same
time for all entities declared in a given block, allowing the use of
a single stack for a whole program.


In relations between people or companies, a contract is a written document that serves to clarify the terms of a relationship. It is really surprising that in software

## Assertions are not an input checking mechanism

## Class features vs. ADT functions
To understand the relation between assertions and ADTs we need first to 
establish the relation between class features and their ADT counterparts - 
the ADT's functions. An earlier discussion introduced three categories of 
function: 
 - creators
 - queries
 - commands.

As you'll recall, the category of a function

    f:A x B x ... -> X

depended on where the ADT, say T, appeared among the tyoes A, B,... X involved
in this signature:
 - If T appears on the right only, f is a creator; in the class it yields a
    creation procedure.
 - If T appears only on the left og the arrow, f is a query, providing access
    to properties of instances of the class. The corresponding features are 
    either attributes or functions (collectively called queries, for classes 
    as well as ADTs).
 - If T appears on both the left and the right, f is a command function, which
    yields a new object from one or more existing objects. Often f will be 
    expressed at the implementation stage, by a procedure (also called a 
    command) which modifies an object, rather than creating a new object as 
    a function would do.


## Modular Protection
A method satisfies *Modular Protection* if it yields architectures in
which the effect of an abnormal condition occuring at runtime in a
module will remain confined to that module, or at worst will only
propagate to a few neighboring modules.

## Class and system validity
Some terminology will be useful to discuss the issues raised by
*covariance* and *descendant hiding*. A system is *class-valid* if it
 satisfies the type rules summarized at the beginning of chapter 17:
every entity declared with a type; every assignment and actual-formal
argument assiciation satisfies conformance; and every call uses a
feature of the target's type, exported to the caller.

  The system is *system valid* if no type violation can occur at run
  time.

Ideally these two notions should be equivalent. What we have seen
through the preceeding examples is that with covariance and descendant
hiding a system can be class-valid without being system-valid. Such an
error - making a system invalid although it is class-valid - will be
called a *system validity error*


## Law of Inversion
If your routines exchange too many data, put your routines in your data.

## Behaviour Classes
Deferred classes capturing the common behaviour of a large number of
possible objects, implementing what is fully known at the most general
level in terms of what depends on each variant.

## Inheritance terminology
A *descendant* of a class C is any class that inherits directly or
indirectly from C, including C itself. (Formally: either C or,
recursively, a descendant of an heir of C.

A *proper descendant* of C is a descendat other than C itself.

An *Ancestor* of C is a class A such that C is a descendant of A.

A *proper ancestor* of C is a class A such that C is a proper descendant
of A.


page 868 Using inheritance well

# Reference
page 167: function categories
explanation of creators, querries and commands.
