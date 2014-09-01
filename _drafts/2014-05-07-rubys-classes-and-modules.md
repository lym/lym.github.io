---
layout: post
title: "Ruby's Classes and Modules"
description: ""
category: Programming
tags: ["Ruby", "Programming", "Object-oriented Programming"]
---
{% include JB/setup %}

# Classes and Modules.

Ruby is an *object-oriented language* in a very pure sense: every value
in Ruby is (or at least behaves like) an object. Every object is an
instance of a class. A class defines a set of methods that an object
responds to. Classes may extend or subclass other classes, and inherit
or override the methods of their superclass. Classes can also include -
or inherit methods from - modules.

Ruby's objects are strictly encaspulated: their state can be accessed
only through the methods they define. The instance variables manipulated
by those methods cannot be directly accessed from outside the object. It
is possible to define *getter* and *setter* accessor methods that
appear to access state directly. These pairs of accessor methods are
known as attributes and are distinct from instance variables. The
methods defined by a class may have "public", "protected" or "private"
visibility, which affects <i>how</i> and <i>where</i> they may be
invoked.

In contrast to the strict encapsulation of object state, Ruby's classes
are very open. Any Ruby program can add methods to existing classes, and
it is even possible to add *Singleton methods* to individual objects.

Much of Ruby's OO architecture is part of the core language. Other
parts, such as the creation of attributes and the declaration of method
visibility, are done with methods rather than true language keywords.
THis chapter begins with an extended tutotial that demonstrates how to
define a class and add methods to it. This tutorial is followed by
sections on more advanced topics, including:

- Method visibility
- Subclassing and inheritance
- Object creation and initialization
- Modules, both as namespaces and as includable "mixins"
- Singleton methods and the eigen class
- The method name resolution algorithm
- The constant name resolution algorithm.

# Defining a Simple Class
We begin our coveerage of classes with an extended tutorial that
develops a class named <i>Point</i> to represent a geometric point with
X and Y coordinates. The subsections that follow demonstrate how to:

- Define a new class
- Create instances of that class
- Write an initializer method for that class
- Add attribute accessor methods to the class
- Define operators for the class
- Define an iterator method and make the class Enumerable
- Override important *Object* methods such as `to_s`, ==, hash and <=>
- Define class methods, class variables, class instance variables and
  constants.

## Creating the class
Classes are created in Ruby with that *class* keyword:

    class Point
    end

Like most Ruby constructs, a class definition is delimited with an
<i>end</i>. In addition to defining a new class, the class keyword
creates a new constant to refer to the class. THe class name and the
constant name are the same, so all class names must begin with a capital
letter.

Within the body of a class, but outside of any instance methods defined
by the class, the *self* keyword refers to the class being defined.

Like most statements in Ruby, class is an <i>expression</i>. The value
of a class expression is the value of the last expression within the
class body. Typically, the last expression within a class if a *def*
statement that defines a method. The value of the def <i>statement</i>
is always nil.

## Instantiating a Point
Even though we haven't put anything in our Point class yet, we can still
instantiate it:

    p = Point.new

The constant Point holds a class object that represents our new class.
All class objects have a method named new that creates a new instance.

We can't do anything very interesting with the newly created Point
object we've stored in the local variable p, because we haven't yet
defined any methods for the class. We can, however, ask the new object
what kind of object it is:

    p.class       #=> Point
    p.is_a? Point #=> true

begin note
In Ruby 2.0 Every instance of an empty class, one that has no methods or
attributes attached, like any instance of the Point class above gets 61
methods attached to it by default.
end note

## Initializing a Point
When we create new Point objects, we want to initialize them with two
numbers that represent their X and Y coordinates. In many
object-oriented languages, this is done with a "constructor." In RUby,
it is done with an <i>initialize</i> method:

    class Point
        def initialize(x,y)
          @x, @y = x, y
        end
    end

This is only three lines of code, but there are a couple of important
points to point out here. We explained the *def* keyword in detail in
Chapter 6 but that chapter focussed on defining global functions that
could be used from anywhere. When def is used like this with an
unqualified method name inside of a class definition, it defines an
instance method for that class. An instance method is a method that is
invoked on an instance of the class. When an instance method id called,
the value of *self* is an instance of the class in which the method is
defined.

The next point to note is that the `initialize` method has a special
purpose in Ruby. The `new` method of the class object creates a new
instance object, and then it automatically invokes the `initialize`
method on that instance. Whatever arguments you passed to new are passed
on to initialize. Because our `initialize` method expects two arguments,
we must now supply two values when we invoke `Point.new`:

    p = Point.new(0,0)

In addition to being automatically invoked by `Point.new` the
`initialize` method is automatically made private. An object can call
initialize in itself, but you cannot explucitly call initialize on `p`
to reinitialize it state.

Now let's look at the body of the `initialize` method. It takes the two
values we've passed to it , stored in local variables x and y, and
assigns them to instance variables @x and @y. Instance variables always
begin with `@` and they always "belong to" whatever object `self` refers
to. Each instance of our Point class has its own copy of these two
varaibles, which hold its own X and Y coordinates.

The *instance variables* of an object can only be accessed by the
*instance methods* of that object. Code that is not inside an instance
 method cannot reas or set that value of an instance variable (unless it
uses one of the reflexive techniques that are described in chapter 8.

## Defining a `to_si` Method
Just about any class you define should have a `to_s` instance method to
return a string representation of the object. This ability proves
invaluable when debugging. Here's how we might do this for Point:

    class Point
        def initialize(x, y)
          @x, @y = x, y
        end

        def to_s        # Return String that represents this Point
          "(#@x, #@y)"  # Just interpolate the instance variables into a string
        end
    end

With this new method defined, we can create points and print them out:

    p = new Point(1, 2)   # Create a new Point object
    puts p                # Displays "(1, 2)"


## Accessors and Attributes
Our Point class uses two instance variables. As we've noted, however,
the value of these variables are only accessible to other instance
methods. If we want the users/clients of the Point class to be able to
use the X and Y coordinates of a point, we've got to provide accessor
methods that return the value of the variables:

    class Point
        def initialize(x,y)
          @x, @y = x, y
        end

        def x         # The accessor (or getter) method for @x
          @x
        end

        def y         # The accessor method for @y
          @y
        end
    end

With these methods defined, we can write code like this:

    p = Point.new(1, 2)
    q = Point.new(p.x*2, p.y*3)

The expressions `p.x` and `p.y` may look like variable references, but
they are, in fact, method invocations without parentheses.

If we wanted our Point class to be mutable (which is probably not a good
idea), we would also add setter methods to set the value of the instance
variables:

    class MutablePoint
        def initialize(x,y); @x, @y = x, y; end

        def x; @y; end        # The getter method for @x
        def y; @y; end        # The getter method for @y

        def x=(value)         # The setter method for @x
          @x = value
        end

        def y=(value)         # The setter method for @y
          @y = value
        end
    end

Recall that assigment expressions can be used to invoke setter methods
like these methods defined, we can write:

    p = Point.new(1, 1)
    p.x = 0
    p.y = 0

## Using Setters Inside a Class
Once you've defined a *setter method* like `x=` for your class, you
might be tempted to use it within other instance methods of your class.
That is, instead of writting `@x=2`, you might write `x=2`, intending to
invoke `x=(2)` implicitly on self. It doesn't work, of course; `x=2`
creates a new local variable.

The rule is that assignment expressions will only invoke a setter method
when invoked through an object. If you want to use a setter from within
the class that defines it, invoke it explicitly through `self`. For
example: `self.x=2.`


This combination of instance variables with trivial getter and setter
methods is so common that Ruby provides a way to automate it. The
`attr_reader` and `attr_accessor` methods are defined by the Module
class. All classes are modules, (the Class class is a subclass of
Module) so you can invoke these methods inside any class definition.
Both methods take any number of symbols naming attributes. `attr_reader`
creates trivial getter methods for instance variables with the same
name. `attr_accessor` creates getter and setter methods. Thus, if we
were defining a mutable Point class, we could write

    class Point
        attr_accessor :x, :y  #Define accessor mtds for our instance variables
    end

And if we were defining an immutable version of the class, we'd write:

    class Point
        attr_reader :x, :y  # Define reader mtds fo our instance variables
    end

Each of these methods can accept an attribute name or names as a string
rather than as a symbol. The accepted style is to use symbols, but we
can also write code like this:

    attr_reader "x", "y"


## Defining Operators
We'd like the `+` operator to perform vector addition of two Point
objects, the `*` operator to multiply a scalar, and the unary `-`
operator to do the equivalent of multiplying be -1. Method-based
operators such as + are simply methods with punctuations for names.
Because there are unary and binary forms of the `-` operator, Ruby uses
the method name `@` for unary minus. Here's a version of the Point class
with mathematical operators defined:

 
    class Point
        attr_reader :x, :y   # Define accessor mtds for our instance variables

        def initialize(x,y)
          @x,@y = x, y
        end

        def +(other)         # Define + to do vector addition
          Point.new(@x + other.x, @y + other.y)
        end

        def -@               # Define unary minus to negate both coordinates
          Point.new(-@x, -@y)
        end

        def *(scalar)        # Define * to perform scalar multiplication
          Point.new(@x*scalar, @y*scalar)
        end
    end

Take a look at the body of the `+` method. It is able to use the `@x`
instance variable of `self` - the object that the method is invoked on.
But it cannot access `@x` in the other Point object. Ruby simple does
not have a syntax for this; all instance variable references implicitly
use self. Our + method, therefore, is dependent on the `x` and `y`
getter methods. (We'll see later that it is possible to restrict the
visibility of methods so that objects of the same class case use each
other's methods, but code outside the class cannot use them.)


### Type Checking and Duck Typing
Our `+` method does not do any *type checking*; it simply assumes that
it has been passed a suitable object. It is fairly common in Ruby
programming to be loose about the definition of "suitable". In the case
of our `+` method, any object that has methods named `x` and `y` will
do, as long as those methods expect no arguments and retunr a number of
some sort. We don't care if the argument actually is a point, as long as
it looks and behaves like a point. This approach is sometimes called
"duck typing" after the adage "if it walks like a duck and quacks like a
duck, it must be a duck."

If we pass an object to `+` that is not suitable, Ruby will raise an
exception. Attempting to add 3 to a point, for example, results in this
error message:

    NoMethodError: undefined method `x' for 3:Fixnum
            from ./point.rb:37:in `+'

Translated, this tells us that the Fixnum 3 does not have a method named
x, and that this error arose in the `+` method of the Point class. This
is all the information we need to figure out the source of the problem,
but it is somewhat obscure. Checking the class of the method arguments
may make it easier to debug code that uses that method. Here is a
version of the method with class verification:

    def +(other)
        raise TypeError, "Point argument expected" unless other.is_a Point
        Point.new(@x + other.x, @y + other.y)
    end

Here's a looser version of typing that provides improved error messages
but still allows duck typing:

    def +(other)
        raise TypeError, "Point-like argument expected" unless
            other.respond_to? :x and other.respond_to? :y
        Point.new(@x + other.x, @y + other.y)
    end

Note that this version of the method still assumes that `x` and `y`
methods return numbers. We'd get an obscure error message if one of
these methods returned a string, for example.

Another approach to type checking occurs after the fact. We can simply
handle any expetions that occur during execution of the method and raise
a more appropriate exception of our own:

    def +(other)                    # Assume that other looks like a Point
        Point.new(@x + other.x, @y + other.y)
    rescue                          # If anything goes wrong above
        raise TypeError # Then raise our own exception
            "Point addition with an argument that does not quack like a Point"
    end

Note that our `*` method expects a numeric operand, not a Point. If p is
a point, then we can write `p*2`. As our class is written, however, we
cannot write `2*p`. That second expression invokes the `*` method of the
Interger class, which doesn't know how to work with Point objects.
Because the Integer class doesn't know how to multiply by a Point, it
asks the point for help by calling its `coerce` method. If we want the
expression `2*p` to return the same result as `p*2`, we can define
`coerce` method:

    # If we try passing a Point to the * method of Integer, it will call
    # this method on the Point and then will try to multiply the elements
    # of the array. Instead of doing type conversion, we switch the order
    # of the operands, so tha we invoke the * method defined above.
    def coerce(other)
        [self, other]
    end


## Array and Hash Access with []
Ruby uses square brackets for array and hash access, and allows any
class to define a `[]` method and use these brackets itself. Let's
define a `[]` method for our class to allow Point objects to be treated as
read-only arrays of length 2, or as read-only hashes with keys `:x` and
`:y`

    # Define [] method to allow a Point to look like an array or
    # a hash with keys :x and :y
    def [](index)
        case index
        when 0, -2: @x      # Index 0 (or -2) is X coordinate
        when 1, -1: @y      # Index 1 (or -1) is the Y coordinate
        when :x, "x": @x    # Hash keys as symbol or string for X
        when :y, "y": @y    # Hash keys as symbol or string for Y
        else nil            # Arrays & hashes just return nil on bad indexes
        end
    end

## Enumerating Coordinates
If a Point can behave like an array with two elements, the perhaps we
out to be able to iterate through those elements as we can with a true
array. Here's a definition of the `each` iterator for our Point class.
Because a Point always has exactly two elements, our iterator doesn't
have to loop; it can simply call yield twice:

    # This iterator passes the X coordinate to the associted block, and
    # then passes the Y coordinate, and then returns. It allows us to
    # enumerate a point as if it were an array with two elements. This
    # each method is required by the Enumerable module.
    def each
        yield @x
        yield @y
    end

With this iterator defined, we can write code like this:

    p = Point.new(1,2)
    p.each {|x| print x }   #prints "12"

More importantly, defining the each iterator allows us to mix in the
methods of the Enumerable module, all of which are defined in terms of
each. Our class gains over 20 iterators by adding a single line:

    include Enumerable

If we do this, then we can write interesting code like this:

    # Is the point P at the origin?
    p.all? {|x| x == 0 }  # True if the block is true for all elements

## Point Equality
As our class is currently defined, two distinct Point instances are
never equal to each other, even if their X and Y coordinates are the
same. To remedy this, we must provide an implementation of the `==`
operator. 

Here's an `==` method for Point:
  
    def ==(other)                       # Is self == o?
        if other.is_a? Point              # If other is a Point object
            @x == other.x && @y == other.y  # then compare the fields.
        elsif                             # If other is not a Point
            false                           # then, by definition, self != other
        end
    end

### Duck Typing and Equality
The `+` operator we defined did no type checking at all: it works with
any argument object with `x` and `y` methods that return numbers. This
`==` method is implemented differently; instead of allowing duck typing,
it requires that the argument is a Point. This is an implementation
choice. The implementation of `==` above chooses to define equality so
that an object cannot equal to a Point unless it is itself a Point.

Implementations may be more stricter or more liberal than this. The
implementation above uses the `is_a?` predicate to test the class of the
argument. This allows an instance of a subclass of Point to be equal to
a Point. A stricter implementation would use `instance_of?` to disallow
subclass instances. Similarly, the implementation above uses `==` to
compare the X and Y coordinates. For numbers, the `==` operator allows
type conversion, which means that the point (1,1) is equal to 
(1.0, 1.0). This is probably as it should be, but a stricter definition
of equality could use `eql` to compare the coordinates.

A more liberal definition of equality would support duck typing. Some
caution is required, however. Our `==` method should not raise a
`NoMethodError` if the argument object does not have `x` and `y`
methods. Instead, it should simply return false:

    def ==(other)
        @x == other.x && @y == other.y  # Assume other has proper x & y mtds
    rescue                            # If that assumption fails
        false                           # Then self != other
    end


Recall that Ruby objects also define an `eql?` method for testing
equality. By default the `eql?` method, like the `==` operator, tests
object identity rather than equality of object content. Often, we want
`eql?` to work just like the `==` operator, and we can accomplish this
with an alias:

    class Point
        alias eql? ==
    end

On the other hand, there are two reasons we might want `eql?` to be
different from `==`.  In Numeric and its subclasses, for example, `==`
allows type conversion and `eql?` does not. If we believe that the users
of our Point class might want to be able to compare instances in two
different ways, then we might follow this example. Because points are
just two numbers, it would make sense to follow the example set by
`Numeric` here. Our `eql?` method would look much like the `==` method,
but it would use `eql?` to compare point coordinates instead of ==:

    def eql?(other)
        if other.instance_of? Point
            @x.eql?(other.x) && @y.eql?(other.y)
        elsif
            false
        end
    end

As an aside, note that this is the right approach for any classes that
impement collections *(sets, lists, trees)* of arbitrary objects. The
`==` operator should compare the members of the collection using their
`==` operators, and the `eql?` method should comapre the members using
their `eql?` method.

The second reason to implement an `eql?` method that is different from
the `==` operator is if you want instances of your class to behave
specially when used as a hash key. The `Hash` class uses `eql?` to
compare hash keys (but not values). If you leave `eql?` undefined, then
hashes will compare instances of your class by object identity. This
means that if you associate a value with a key `p`, you will only be
able to retrieve that value with the exact same object `p`. An object
`q` won't work even if `p == q`. Mutable objects do not work well as
hash keys, but leaving `eql?` undefined neatly sidesteps the problem.

Because `eql?` is used for hashes, you must never implement this method
by itself. If you define an `eql?` method, you must also define a hash
method to compute a hashcode for your object. If two objects are equal
according to `eql?`, then their hash methods must return the same value.
(Two unequal objects may return the same hashcode, but you should avoid
this to the extent possible.)

Implementing optimal hash methods can be very tricky. Fortunately,
there's a simple way to compute perfectly adequate hashcodes for just
about any class: simply combine the hashcodes in the proper way. The
following hash method is *not* a good one:
  
    def hash
        @x.hash + @y.hash
    end

The problem with this method is that it returns the same hash code for
the point (1,0) as it does for the point (0,1). This is legal, but it
leads ti poor performance when points are used as hash keys. Instead, we
should mix things up  a bit:

    def hash
        code = 17
        code = 37*code + @x.hash
        code = 37*code + @y.hash
        # Add lines like this for each significant instance variable
        code  # Return the resulting code
    end

## Ordering Points
Suppose we wish to define an ordering for Point objects so that we can
compare and sort them. There are a number of ways to order points, but
we'll chose to arrange them based on their distance from the origin.
This distance (or magnitude) is computed by the *Pythogorean theorem*:
the square root of the sum of the squares of X and Y coordinates.

To define this ordering for Point objects, we need only define the `<=>`
operator and include the `Comparable` module. Doing this mixes in
implementations of the equality and relational operators that are based
on our implementation of the general `<=>` operator we defined. The
`<=>` operator should compare `self` to the object it is passed. If
`self` is less than that object (closer to the origin, in this case), it
should return -1. If the two objects are equal, it should return 0.
Finally is `self` is greater than the argument object, the method should
return 1. (The method should return `nil` of the argument object and
`self` are of incomparable types). The following code is our
implementation of `<=>`. There are two thing to note about it. Firstly,
it doesn't bother with the `Math.sqrt` method and simply comapres the
sum of the squares of the coordinates. Second, after computing the sums
of the squares, it simply delegates to the `<=>` of the Float class:

    include Comparable  

    # Define an ordering for points based on their distance from the
    # origin. 
    # This method is required by the comparable module.
    def <=>(other)
        return nil unless other.instance_of? Point
        @x**2 + @y**2 <=> other.x**2 + other.y**2
    end

Note that the `Comparable` module defines an == method that uses our
definition of `<=>`. Our distance-based comparison operator results in
an `==` method that considers points (1,0) and (0,1) to be equal.
Because our Point class explicitly defines its own `==` method, however,
the `==` method of Comparable is never invoked. Ideally, the `==` and
`<=>` operators should have consistent definitions of equality. This was
not possible with our Point class, and we end up with operators that
allow the following:

    p,q = Point.new(1,0), Point.new(0,1)
    p == q          # => false: p is not eqaul to q
    p < q           # => false: p is not less than q
    p > q           # => false: p is not greater than q

Finally, it is worth noting here that the `Enumerable` module defines
several methods such as `sort`, `min` and `max` that only work if the
objects being enumerated define the `<=>` operator.


## A mutable Point
The Point class we've been developing is immutable: once a point object
is created, there's no public API to change the X and Y coordinates of
that point. This is probably as it should be. But let's detour and
investigate some methods we might add if we wanted points to be mutable.

First of all, we'd need `x=` and `y==` setter methods to allow the X and
Y coordinates to be set directly. We could define these methods
explicitly, or simply change our `attr_reader` to `attr_accessor`:

Next, we'd like an alternative to the `+` operator for when we want to
add the coordinates of point q to the coordinates of point p, and modify
point p rather than creating and returning a new Point object. We'll
call this method `add!`, with the exclamation mark indicating that it
alters the internal state of the object on which it is invoked:

    def add!(p)         # Add p to self, return modified self
        @x += p.x
        @y += p.y
        self
    end

When defining a mutator method, we normally only add an exclamation mark
to the name if there is a nonmutating version of the same method. In
this case, the name `add!` makes sense if we also define an add method
that returns a new object, rather than altering its receiver. A
nonmutating version of a mutator method is often written simply by
creating a copy of self and invoking the mutator on the copied object:

    def add(p)        # A nonmutating version of add!
        q = self.dup    # Make a copy of self
        q.add!(p)       # Invoke the mutating method on the copy
    end

In this trivial example, our add method works just like the `+` operatot
we've already defined, and it's not really necessary. So if we don't
define a nonmutating `add`, we should consider dropping the exclamation
mark from `add!` and allowing the name of the method itself ("add"
instead of "plus") to indicate that it is a mutator.


## Quick and easy Mutable Classes
If you want a *mutable* Point class, one way to create it is with
`Struct`. `Struct` is a core Ruby class that generates other classes.
These generated classes have accessor methods for the named fields you
specify. There are two ways to create a new class with Struct.new:

    Struct.new("Point", :x, :y)   # Creates a new class Struct::Point
    Point = Struct.new(:x, :y)    # Creates  new class, assigns to point

### Naming Anonymous Classes
The second line in the code relies on a curious fact about Ruby classes:
If you assgin an unnamed class object to a constant, the name of that
constant becomes the name of a class. You can observe this same
behaviour if you use the `Class.new` constructor:

    C = Class.new     # A new class with no body assigned to a constant
    c = C.new         # Create an instance of the class
    c.class.to_s      # => "C": constant name becomes class name


Once a class has been created with `Struct.new`, you can use it like any
other class. Its new method will expect values for each of the named
fields you specify, and it instance methods provide read and write
accessors for those field:

    p = Point.new(1,2)    # => #<struct Point x =1, y =2>
    p.x                   # => 1
    p.y                   # => 2
    p.x = 3               # => 3
    p.x                   # => 3

Structs also define the `[]` and `[]=` operators for arrays and
hash-style indexing, and even provide `each` and `each_pair` iterators
for looping through the values held in an instance of the struct:

    p[:x] = 4             # => 4: same as p.x =
    p[:x]                 # => 4: same as p.x
    p[1]                  # => 2: same as p.y
    p.each {|c| print c}  # prints "42"
    p.each_pair {|n,c| print n,c} # Prints "x4y2"

Struct-based classes have a working `==` operator, can be used as hash
keys (though caution is necessary because they are mutable), and even
define a helpful `to_s` method:

    q = Point.new(4,2)
    q == p              # => true
    h = {q => 1}        # Create a hash using q as a key
    h[p]                # 1: extract value using p as key
    q.to_s              # "#<struct Point x=4, y=2>"

A Point class defined as a Struct does not have point-specific methods
like `add!` or the `<=>` defined earlier in this chapter. There is no
reason why we can't add them though. Ruby class definitions are not
static. Any class (including classes defined with Struct.new) can be
"opened" and have methods added to it. Here's a Point class initially
defined as a Struct, with point-specific methods added:

  Point = Struct.new(:x, :y)    # Create new class, assign to Point
  class Point                   # Open Point class for new methods
    def add!(other)             # Define add! method
      self.x += other.x
      self.y += other.y
      self
    end

    include Comparable          # Include a module for the class
    def <=>(other)              # Define the <=> operator
      return nil unless other.instance_of? Point
      self.x**2 + self.y**2 <=> other.x**2 + other.y**2
    end
  end

As noted at the begining of this section, the Struct class is designed
to create mutable classes. With just a bit of work, however, we can make
a Struct-based class immutable:

  Point = Struct.new(:x, :y)      # Define mutable class
  class Point                     # Open the class
    undef x=, y=, []=             # Undefine mutator methods
  end

## A Class Method
Let's take another approach to adding Point objects together. Instead of
invoking an instance method of one point ans passing another point to
that method, let's write a method named `sum` that takes any number of
Point objects, adds them together, and returns a new Point. This method
is not an instance method invoked on a Point object. Rather, it is a
class method, invoked through the Point class itself. We might invoke
the sum method like this:

  total = Point.sum(p1, p2, p3)   # p1, p2 and p3 are Point objects

Keep in mind that the expression `Point` refers to a class object that
represents our `Point` class. To define a class method for the Point
class, what we are really doing is defining a `singleton method` of the
Point object. To define a singleton method, use the `def` statement as
usual, but specify the object on which the method is to be defined as
well as the name of the method. Our class method `sum` is defined like
this:

  class Point
    attr_reader :x, :y

    def Point.sum(*points)
      x = y = 0
      points.each {|p| x += p.x; y += p.y }
      Point.new(x, y)
    end

    # .... the rest of the class here....
  end

This definition of the class method names the class explicitly, and
mirrors the syntax used to invoke the method. Class methods can also be
defined using `self` instead of the class name. Thus, this method could
also be written like this:

    def self.sum(*points) 
        x = y = 0
        points.each {|p| x += p.x; y += p.y }
        Point.new(x,y)
    end

Using `self` instead of `Point` makes the code slightly less clear, but
its an application of the *DRY (Don't Repeat Yourself)* principle. If
you use `self` instead of the class name, you can change the name of a
class without having to edit the definition of its class methods.

There's yet another technique for defining class methods. Though it is
less clear than the previously shown techinque, it can be handy when
defining multiple class methods and you are likely to see it in existing
code:

    # Open up the Point object so we can add methods to it
    class << Point        # Syntax for adding methods to a single object.
        def sum(*points)    # This is the class method Point.sum
            x = y = 0
            points.each {|p| x += p.x; y += p.y }
            Point.new(x,y)
        end
        # Other class methods can be defined here
    end

This technique can also be used inside the class definition, where we
can use `self` instead of repeating the class name:

    class Point
      # Instance methods go here

      class << self
        # Class methods go here
      end
    end


## Constants
Many class can benefit from the definition of some associated constants.
Here are some constants that might be useful for our `Point` class:

  class Point
    def initialize(x,y)   # initialize method
      @x, @y = x, y
    end
    
    ORIGIN = Point.new(0.0)
    UNIT_X = Point.new(1,0)
    UNIT_Y = Point.new(0,1)

    # Rest of class definition goes here
  end

Inside the class definition, these constants can be refered ro by their
unqualified names. Outside the definition, the must be prefixed by the
name of the class, of course:

    Point::UNIT_X + Point::UNIT_Y     # => (1,1)

Note that because our constants in this example refer to instances of
the class, we cannot define the constants until after we've defined the
`initialize` method of the class. Also, keep in mind that it is
perfectly legal to define constants in the Point class from outside the
class:

    Point::NEGATIVE_UNIT_X = Point.new(-1,0)


## Class Variables
Class variables are visible to, and shared by the class methods and the
instance ,ethods of a class, and also by the class definition itself.
Like instance variables, class variables are encapsulated; they can be
used by the implementation of the class, but they are not visible to the
users of a class. Class variables have names that begin with `@@`.

There's no real need to use class variables in our `Point` class, but
for the purposes of this tutorial, let's suppose that we want to collect
data about the number of Point objects that are created and their
average coordinates. Here's how we might write the code:

  class Point
    # Initialize our class variables in the class definition itself
    @@n      = 0          # How many points have been created
    @@totalX = 0          # The sum of all X coordinates
    @@totalY = 0          # The sum of all Y coordinates

    def initialize(x,y)   # Initialize method
      @x, @y = x, y       # Sets initial values for instance variables
      
      # Use the class variables in this instance mtd to collect data
      @@n += 1            # Keep track of how many pnts have been created
      @@totalX += x       # Add these coordinates to the totals
      @@totalY + = y
    end

    # A class method to report the data we collected
    def self.report
      # Here we use the class variables in a class method
      puts "Number of points created: #@@n"
      puts "Average X coordinate: #{@@totalX.to_f/@@n}"
      puts "Average Y coordinate: #{@@totalY.to_f/@@n}"     
    end
  end

The thing to notice about this code is that class variables are used in
instance methods, class methods and in the class definition itself,
outside of any method. 

Class variables are fundamentally different form instance variables.
We've seen that instance variables are always evaluated in reference to
`self`. That is why an instance variable reference in a class definition
is completely different from an instance variable reference in an
instance method. Class variables, on the other hand, are always
evaluated in reference to the class object created by the enclosing
Class definition statement.


## Class Instance Variables
Classes are objects and can have instance variables just as other
objects can. The instance variables of a class - often called class
instance variables - are not the same as class variables. But they are
similar enough that they can often be used instead of class variables.

An instance variable used inside a class definition but outside an
instance method definition is a class instance variable. Like class
variables, class instance variables are associated with the class rather
than with any particular instance. A disadvantage of class instance
variables is that they cannot be used within instance methods as cass
variables can. Another disadvantage is the pottential for confusing them
with ordinary instance variables. Without the distinctive punctuation
prefixes, it may be more difficult to remember whether a variable is
associated with instances or with the class object.

One of the most important advantages of class instance variables over
class variables has to do with the confusing behaviuor of class
variables when subclassing an existing class. WE'll return to this point
later in the chapter.

Let's port our statistics gathering verrsion of the Point class to use
class instance variables instead of class variables. The only difficulty
is that because class instance variables cannot be used form instance
methods, we must move the statistics gathering code out of the
`initialize` method (which is an instance method) and into the new class
method used to create points:

  class Point
    # Initialize our class instance variables in the class definition
    # itself
    @n = 0        # How many points have been created
    @totalX = 0   # The sum of all X coordinates
    @totalY = 0   # The sum of all Y coordinates

    def initialize(x, y)  # Initialize method
      @x, @y = x, y       # Sets initial values for instance variables
    end

    def self.new(x,y)     # Class method to create new point objects
      # Use the class instance vars in this class mtd to collect data
      @n += 1      # Keep track of how manu points have been created
      @totalX += x        # Add these coordinates to the totals
      @totalY += y
      
      super       # Invoke the real definition of new to create a Point
    end

    # A class method to report the data we collected
    def self.report
      # Here we use the class instance variables in a class method
      puts "Number of points created: #@n"
      puts "Average X coordinate: #{@totalX.to_f/@n}"
      puts "Average Y coordinate: #{@totalY.to_f/@n}"
    end
  end

Because class instance variables are just instance variables of class
objects, we can use `attr_reader` and `attr_accessor` to create accessor
methods for them. The trick, however, is to invoke these metaprogramming
methods in the right context. Recall that one way to define class
methods uses the syntax `class << self`. This same syntax allows us to
define attribute accessor methods for class instance variables:

    class << self
        attr_accessor :n, :totalX, :totalY
    end

With these accessors defined, we can refer to our raw data as `Point.n`,
`Point.totalX` and `Point.totalY`.


# Method Visibility: Public, Protected, Private
Instance methods may be <i>public</i>, <i>private</i> or
<i>protected</i>. If you've programmed with other object-oriented
languanges, you may already be familiar with these terms. Pay attention
anyway, because these words have a somewhat different meaning in Ruby
from that in other languages.

Methods are normally public unless they are explicitly declared to
private or protected. Once exception is the `initialize` method, which
is always implicitly private. Another exception is any "global" method
declared outside of a class definition - those methods are defined as
private instance methods of `Object`. A public method can be accessed
from anywhere - there are no restrictions on it's use.

A private method is internal to the implementation of a class, and it
can only be called by other instance methods of the class (or, as we'll
see later, its subclasses). Private methods are implicitly invoked on
`self`, and may not be explicitly invoked on an object. If `m` is a
private method, then you must invoke it in functional style as `m`. You
cannot write `o.m` or even `self.m`.

A protected method is just like a private method in that it can only be
invoked from within the implementation of a class or its subclasses. It
differs from a private method in that it may be explicitly invoked on
any instance of the class, and it is not restricted to implicit
invocation on `self`. A protected method can be used for example, to
define an accessor that allows instances of a class to share internal
state with each other, but does not allow users of the class to share
that state.

Protected methods are the least commonly defined and also the most
difficult to understance. The rule about when a protected method can be
invoked can be formally described as follows:

<blockquote>
  A protected method defined by a class C may be invoked on an object o
by a method in an object b if and only if the classees of o and p are
both subclasses of, or equal to, the class C.
</blockquote>

Method visibility is declared with three nethods named public, private
and protected. These are instance methods of the Module class. All
classes are modules, and inside a class definition (but outside method
definitions), `self` refers to the class being defined. Thus, public,
private and protected may be used bare as if they were keywords of the
language. In fact, they are method invocations on `self`. There are two
ways to invoke these methods. With no arguments, they specify that all
subsequent method definitions will have the specified visibility. A
class use them like this:

  class Point
    # public methods go here

    # The following methods are protected
    protected

    # protected methods go here

    # The following methods are private
    private

    # private methods go here
  end

The methods may also be invoked with the names of one or more methods
(as symbols or strings) as arguments. When invoked like this, they alter
the visibility of the named methods. In this usage, the visibility
declaration must come after the definition of the method. One approach
is to declare all private and protected methods at once, at the end of a
class. Another approach is to declare the visibility of each private
or protected method immediately after it's defined. Here, for example,
is a class with a private utility method and a protected accessor
method:

  class Widget
    def x
      @x
    end
    protected :x

    def utility_method
      nil
    end 
    private :utility_method
  end

Remember that public, private and protected apply only to methods in
Ruby. Instance variables are encapsulated and effectively private and
constants are effectively public. There is no way to make an instance
variable accessible from outside a class (except by defining an accessor
method, of course). And there is no way to define a constant that is
inaccessible to outside use.

Occasionally, it is useful to specify that a class method should be
private. If you class defines factory methods, for example, you might
want to make the `new` method private. To do this, use the
`private_class_method` method, specifying one or more method names as
symbols:

    private_class_method :new

You can make a private class method public again with
`public_class_method`. Neither method can be invoked without arguments
in the way that public, private and protected can be.

Ruby is, by design, a very open language. The ability to specify that
some methods are private and protected encourages good programming style
and prevents inadvertent use of method that are not part of the public
API of a class. It is important to understand, however, that Ruby's
metaprogramming capabilities make it trivial to invoke private and
protected methods and even to access encapsulated instance variables. To
invoke the private utility method defined in the previous code, you can
use the `send` method, or you can use `instance_eval` to evaluate a
block in the context of the object:

    w = Widget.new                      # Create a Widget 
    w.send :utility_method              # Invoke private method!
    w.instance_eval { utility_method }  # Another way to invoke it
    w.instance_eval { @x }              # Read instance variable of w

If you want to invoke a method by name, but you don't want to
inadvertently invoke a private method that you don't know about, you can
(in Ruby 1.9) use `public_send` instead of `send`, but does not invoke
private methods when called with an explicit receiver. 


# Subclassing and Inheritance
Most object-oriented languages, including Ruby, provide a subclassign
mechanism that allows us to create new classes whose behaviour is based
on, but modified from, the behaviour of an existing class. We'll begin
this discussion of subclassing with definitions of ht ebasic
terminology. If you've programmed in Java, C++ or a similar language,
you are probably already familiar with these terms.

When we define a class, we may specify that it extends - or inherits
from - another class, know as a <i>parent</i> class. If we define a
class `Ruby` that extends a class `Gem`, we say that `Ruby` is an
<i>heir</i> of `Gem`, and that `Gem` is a <i>parent</i> of `Ruby`
If you do not specify a parent when you define a class, then your class
implicitly extends `Object`. A class may have any number of heirs and
every class has a single parent except `Object`, which has none.

The fact that classes may have multiple heirs but only a single parent
means that they can be arranged in a tree structure, which we call
<i>the Ruby class hierarchy</i>. The Object class is the root of this
hierarchy, and every class inherits directly or indirectly from it. 


## Inheritance terminology
A *descendant* of a class C is any class that inherits directly or
indirectly from C, including C itself. (Formally: either C or,
recursively, a descendant of an heir of C.

A *proper descendant* of C is a descendat other than C itself.

An *Ancestor* of C is a class A such that C is a descendant of A.

A *proper ancestor* of C is a class A such that C is a proper descendant
of A.

In Ruby 1.9, `Object` is no longer the root of the class hierarchy. A
new class named `BasicObject` serves that purpose, and `Object` is an
heir of `BasicObject`. BasicObject is a very simple class, with almost
no methods of its own, and it is useful as an ancestor of delegating
wrapper classes

When you create a class in Ruby 1.9, you still extend `Object` unless
you explicitly specify the superclass, and most programmers will never
need to use or extend `BasicObject`.

The syntax for extendind a class is simple. Just add a `<` character and
the name of the parent to your class statement. For example:

    class Point3D < Point   # Define class Point3D as an heir of Point
    end

We'll flesh out this three-dimensional Point class in the subsections
that follow, showing how methods are inherited from the parent(s), and
how to override or augment the inherited methods to define new behaviour
for the heir.

### Subclassing a Struct
Earlier in this chapter, we saw how to use `Struct.new` to automatically
generate simple classes. It is also possible to inherit from a
struct-based class, so that methods other than the automatically
generated ones can be added:

    Class Point3D < Struct.new("Point3D", :x, :y, :z)
        # Parent struct gives us accessor methods, ==, to_s, etc.
        # Add point-specific methods here
    end

## Inheriting Methods
The `Point3D` class we have defined is a trivial heir of `Point`. It
declares itself an extension of `Point` but there is no class body, so
it adds nothing to that class. A `Point3D` object is effectively the
same thing as a `Point` object. One of the only observable differences
is in the value returned by the `class` method:

    p2 = Point.new(1, 2)
    p3 = Point3D.new(1, 2)
    print p2.to_s, p2.class     # prints "(1, 2)Point"
    print p3.to_s, p3.class     # prints "(1, 2)Point3D"

The value returned by the `class` method is different, but what's more
striking about this example is what is the same. Our `Point3D` object
has inherited the `to_s` method defined by `Point`. It has also
inherited the `initialize` method - this is what allows us to create a
`Point3D` object with the same new call that we use to create a `Point`
object. There's another example of method inheritance in this code: both
`Point` and `Point3D` inherit the `class` method from `Object`.


## Overriding Methods
When we define a new class, we add new behaviour to it by defining new
methods. Just as importantly, however, we can customize the inherited
behaviour of the class by redefining inherited methods.

For example, the `Object` class defines a `to_s` method to convert an
object to a String in a very generic way:

    o = Object.new
    puts o.to_s   # Prints something like "#<Object:0xb7f7fce4>"

When we defined a `to_s` method in the `Point` class, we were
overriding/redefining the `to_s` method inherited from `Object`.

One the important things to understand about object-oriented programming
and inheritance is that when methods are invoked, they are looked up
dynamically so that the appropriate definition or redefinition of the
method is found. That is, method invocations are not bound statically at
the time they are parsed, but rather, are looked up at the time they are
executed. Here's an example to demonstrate this important point:

  # Greet the World
  class WorldGreeter
    def greet         # Display a greeting
      puts "#{greeting} #{who}"
    end

    def greeting      # What greeting to use
      "Hello"
    end

    def who           # Who to greet
      "World"
    end
  end

  # Greet the world in Spanish
  class SpanishWorldGreeter < WorldGreeter
    def greeting                            # Override the greeting
      "Holla"
    end
  end

  # We call a method defined in the WorldGreeter, which calls the
  # overridern/redefined version of greeting in SpanidhWorldGreeting, and
  # prints "Hola World"
  SpanishWorlGreeter.new.greet

If you've done object-oriented programming before, the behaviour of this
program is probably obvious and trivial to you. But if you're new to it,
it may be profound. We call the `greet` method inherited from
`WorldGreeter`. This `greet` method calls the `greeting` method. At the
time that `greet` was defined, the `greeting` method returned "Helloe".
But we've inherited from `WorldGreeter`, and the object we're calling
`greet` on has a new definition of `greeting`. When we invoke
`greeting`, Ruby lloks up the "appropriate" definition of that method
for the object it is being invoked on and we end up with a proper
Spanish greeting than an English one. This runtime lookup of the
appropriate definition of a method is called
<i>method name resolution</i>

Notice that it is also perfectly reasonable to define an abstract class
(defered class) that invokes certain undefined "abstract" methods, which
are left for descendants to define. The opposite of abstract is
concrete. A class that extends an abstract class is concrete if it
defines all of the abstract methods of its ancestors. For example:

  # This class is abstract; it doesn't define `greeting` or `who`
  # No special syntax is required: any class that invokes methods that are 
  # intended for a descendant to implement is abstract.
  class AbstractGreeter
    def greet
      puts "#{greeting} #{who}"
    end
  end

  # A concrete(effective) heir
  class WorldGreeter < AbstractGreeter
    def greeting; "Hello"; end
    def who; "World"; end
  end

  WorldGreeter.new.greet    # Displays "Hello World"

### Overriding private methods
Private methods cannot be invoked from outside the class that defines
them. But they are inherited by descendants. This means that descendants
can invoke them and can override them.

Be careful when you inherit from a class that you did not write
yourself. Classes often use private methods as internal helper methods.
They are not part of the public API of the class and are not intended to
be visible. If you haven't read the source code of the class, you won't
even know the names of the methods it defines for its own use. If you
happen to define a method (whatever its visibility) in your descendant
class that has the same name as a private method in the superclass, you
will have indvertently overriden the superclass' internal utility
method, and this will almost certainly cause unintended behaviour.

The upshot is that, in Ruby, you should only inherit from a class when
you are familiar with the implementation of the parent class. If you
only want to depend on the public API of a class not on its
implementation, then you should extend the functionality of the class by
encapsulating and delegating to it (as a client), not by inheriting from
it.

## Augmenting Behaviour by Chaining
Sometimes when we override a method, we don't want to replace it
altogether, we just want to augment its behaviour by adding some new
code. In order to do this, we need a way to invoke the overriden method
from the overriding method. This is known as *chaining*, and it's
accomplished with the keyword `super`.

`super` works like a special method invocation: it invokes a method with
the same name as the current one, in the parent class of the current
class. (Note that the parent class need not define that method itself -
it can inherit it from one of its ancestors.) You may specify arguments
for `super` just as you would for a normal method invocation. One common
and important place for method chaining is the `initialize` method of a
class. Here's how we might write the `initialize` method of our
`Point3D` class:

  class Point3D < Point
    def initialize(x, y, z)
      # Pass our first two arguments along to the superclass `initialize`
      # method
      super(x, y)
      # And deal with the third argument ourself
      @z = z;
    end
  end

If you use `super` as a bare keyword - with no arguments and no
parentheses - then all the arguments that were passed to the current
method are passed to the parent class' method. Note, however, that it's the
current values of the method parameters that are passed to the
parentclass' method. If the current method has modified the values in
its parameter variables, then the modified values are passed to the
invocation of the parent class method.

As with normal method invocations, the parentheses around `super`
arguments are optional. Because a bare `super` has special meaning,
however, one must explicitly use a pair of empty parentheses if one
wants to pass zero arguments from a method that itself has one or more
arguments.

## Inheritance of Class methods.
Class methods can be inherited and overriden just as instance methods
can be. If our `Point` class defines a class method `sum`, then our
`Point3D` heir inherits that method. That is, if `Point3D` does not
define its own class method named `sum`, then the expressions
`Point3D.sum` invokes the same method as the expression `Point.sum`.

As a stylistic matter, it is preferable to invoke class methods through
the class object on which they are defined. A code maintainer seeing an
expression `Point3D.sum` would go looking for a definition of the `sum`
method in the `Point3D` class, and he might have a hard time finding it
in the `Point` class. When invoking a class method with an explicit
receiver, you should avoid relying on inheritance - always invoke the
class method through the class that defines it.

The `Class.new` method is anexception - it is inherited by and invoked
on just about every new class we define.

Within the body of a class method, you may invoke the other class
methods of the class without an explicit receiver - they are invoked on
`self`, and the value of `self` in a class method is the class on which
it was invoked. It is here, inside the body of a class method, that the
inheritance of class methods is useful: it allows you to implicitly
invoke a class method even when that class method is defined by a parent
class.

Finally, note that class methods can use `super` just as instance
methods can to invoke the same-named method in the parent class.


## Inheritance and Instance Variables
Instance variables often appear to be inherited in Ruby. Consider this
code, for example:

  class Point3D < Point
    def initialize(x,y,z)
      super(x,y)
      @z = z;
    end
    def to_s
      "(#@x, #@y, #@z)"  # Variables @x and @y inherited?
    end
  end

The `to_s` method in Point3D references the @x and @y variables from the
parent class Point. This code works as you probably expect it to:

    Point3D.new(1, 2, 3).to_s   # => "(1, 2, 3)"

Because this code behaves as expected, you may be tempted to say that
these variables are inherited. That is not how Ruby works though. All
Ruby objects have a set of instance variables. These are not defined by
the object's representative class - they are simply created when a value
is assigned to them. Because instance variables are not defined by a
class, they are unrelated to the inheritance mechanism.

In this code, `Point3D` defines an `initialize` method that chains to
the `initialize` method of its parent class. The chained method assigns
values to the variables `@x` and `@y`, which makes those variables come
into existance for a particular instance of `Point3D`.

Programmers comming from Java - or from other strongly tyoed languages
in which a class defines a set of fields for its instaces - may find
this takes some getting used to. Really, though, it is quite simple:
Ruby's instance variables are not inherited and have nothing to do with
the inheritance mechanism. The reason that they sometimes appear to be
inherited is that instance variables are created by the methods that
assign values to them, an those methods are often inherited or chained.

There is an important corollary. Because instance variables have nothing
to do with inheritance, it follows that an instance variable used by an
heir cannot "shadow" and instance variable in the parent. If an heir
uses an instance variable with the same name as a variable used by one
of its ancestors, it will overwrite the value of its ancestor's
variable. This can be done intentionally, to alter the behaviour of the
ancestor, or it can be done indvertently. In the latter case, it is
almost certain to cause bugs. As with the inheritance of private methods
decribed earlier, this is another reason why it is only safe to extend
Ruby classes when you are familiar with (and in contel of) the
implementation of the parent.

Finally, recall that class instance variables are simply instance
variables of the `Class` object that represents a class. As such, they
are not inherited. Furthermore, the `Point` and `Point3D` objects (we're
talking about the `Class` objects themselves, not the classes they
represent) are both just instances of `Class`. There is no relationship
between them, and no way that one could inherit variables from the
other.


## Inheritance and Class Variables
Class variables are shared by a class and all of its descendants. If a
class `A` defines a variable `@@a`, then heir `B` can use that variable.
Although this may appear, superficially, to be inheritance, it is
actually something different.

The difference becomes clear when we think about setting the value of a
class variable. If a descendant assigns a value to a class variable
already in use by an ancestor, it does not create its own private copy
of the class variable, but instead alters the value seen by the
ancestor. It also alters the shared value seen by all other descendants
of the parent. Ruby 1.8 prints a warning about this when you run it with
`-w`. Ruby 1.9 does not issue this warning.

If a class uses class variables, then any descendant can alter the
behaviour of the class and all its descendants by changing the value of
the shared class variable. This is a strong argument for the use of
class instance variables instead of class variables.

The following code demonstrates the sharing of class variables. It
outputs 123:

  class A
    @@value = 1                   # A class variable
    def A.value; @@value; end     # An accessor method for it
  end
  print A.value                   # Display value of A's class variable
  class B < A; @@value = 2; end   # Subclass alters shared class variable
  print A.value                   # Superclass sees altered value
  class C < A; @@value = 3; end   # Another alters shared variable again
  print B.value                   # 1st heir sees value from 2nd heir


## Inheritance of Constants
Constants are inherited and can be overriden, much like instance methods
can. There is, however, a very important difference between the
inheritance of methods and the inheritance of constants.

Our `Point3D` class can use trhe `ORIGIN` constant defined by its
`Point` parent, for example. Although the clearest style is to qualify
constants with their defining class, `Point3D` could alos refer to this
constant with an unqualifed `ORIGIN` or even as `Point3D::ORIGIN`.

Where inheritance of constants becomes interesting is when a class like
`Point3D` redefines a constant. A three-dimensional point class probably
wants a constant named `ORIGIN` to refer to a three-dimensional point,
so `Point3D` is likely to include a line like this:

  ORIGIN = Point3D.new(0, 0, 0)

As you know, Ruby issues a warning when a constant is redefined. In this
case, however, this is a newly created constant. We now have two
constants `Point::ORIGIN` and `Point3D::ORIGIN`.

The important difference between constants and methods is that the
constants are looked up in the lexical scope of the place they are used
before they are looked up in the inheritance hierarchy. This means that
if `Point3D` inherits methods that use the constant `ORIGIN`, the
behaviour of those inherited methods will not change when `Point3D`
defines its own version of `ORIGIN`.


# Object Creation and Initialization
