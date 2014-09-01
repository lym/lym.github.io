---
layout: post
title: "Ruby prose notes"
description: "Ruby notes"
category: "Programming"
tags: ["Programming", "Object-oriented programming", "Ruby"]
---
{% include JB/setup %}

`.each` without a block returns an enumerator

for example 

    ['a', 'b', 'c'].each 
    returns a--b--c--


Even if you don’t explicitly create any threads, there is always at least one thread
executing – the main thread in which your Ruby program is running. You can
verify this by entering the following:

    p( Thread.main )

THREAD STATUS
Each thread has a status which may be one of the following:
run  => when thread is executing
sleep  => when thread is sleeping or waiting on I/O
aborting  => when thread is aborting
false  => when thread terminated normally
nil  => when thread terminated with an exception

You can obtain the status of a thread using the status method.

    puts( Thread.main.inspect )
    #=> #<Thread:0x28955c8 run>

    puts( Thread.new{ sleep }.kill.inspect ) 
    #=> #<Thread:0x28cddc0 dead>

    puts( Thread.new{ sleep }.inspect )
    #=> #<Thread:0x28cdd48 sleep>

    thread1 = Thread.new{ }
    puts( thread1.status )
    #=> false

    thread2 = Thread.new{ raise( "Exception raised!" ) }
    puts( thread2 )
    #=> nil


## The `join` method
forces the calling thread (for example the main thread) to suspend its own
execution (so it doesn’t just terminate the program) until the thread which calls
join has completed:

The lower priority is the winner. The main method has a priority of 0
though you can manually set it to a higher one.

To deal with this problem, we need to ensure that, when one thread has access to
a global resource, it blocks the access of other threads. This is another way of
saying that the access to global resources granted to multiple threads should be
‘mutually exclusive’. You can implement this using Ruby’s Mutex class, which
uses a semaphore to indicate whether or not a resource is currently being ac-
cessed, and provides the synchronize method to prevent access to resources
inside a block. Note that you must require ‘thread’ to use the Mutex class.

 This might not seem like a big deal
but it turns out that printing a string takes a bit more time than concatenating
one.


Incidentally, my tests were conducted on a PC running Windows. It is quite
possible that different results will be seen on other operating systems. This is
because the implementation of the Ruby scheduler – which controls the amount
of time allocated to threads – is different on Windows and other operating
systems. On Unix the scheduler runs once every 10 milliseconds but on Windows
the time sharing is controlled by decrementing a counter when certain operations
occur, so that the precise interval is indeterminate.


a line break acts as a terminator while the + operator, when it begins the new line, acts
as a unary operator (it merely asserts that the expression following is positive).

be aware that when entering expressions a line at a time,
the position of the line break is important! 

When using irb you can tell whether
or not the interpreter considers that you have ended a statement. If you have
done so, a plain prompt is displayed ending with ‘>’:
irb(main):013:1>

If the statement is incomplete, the prompt ends with an asterisk:

    irb(main):013:1*

You can see a list of these options by entering thus at the command line:

    irb --help

You can end an irb session by entering the word quit at the prompt or by 
pressing `CTRL+BREAK`.

To run a program in the debugger use the –r debug option (where –r means 
‘require’ and debug is the name of the debugging library) when you start the 
Ruby interpreter. For example, this is how you would debug a program called 
`debug_test.rb`:

    $ ruby –r debug debug_test.rb

while in debugger mode press "l" to list code

    l 1-100 [Enter]
    list from line 1 to 100

    b 78 [Enter]
    put breakpoint on line 78

    wat @t4.name == "wombat" [Enter]

    c [Enter] continue execution till you reach breakpoint.

    del 2 [Enter]
    delete watchpoint number 2

    h [Enter]
    to get help info. while in debug mode

    q [Enter]
    to quit the debugging session

When writing tests:
First you have to require the test/unit file. Then you need to
derive a test class from the TestCase class which is found in the Unit module
which is itself in the Test module:

    class MyTest < Test::Unit::TestCase

Inside this class you can write one or more methods, each of which constitutes a
test containing one or more assertions. The method names must begin with test
(so methods called test1 or testMyProgram are OK, but a method called
myTestMethod isn’t)


`.pop(n)` is an array method that returns an array containing n of the 
array elements

If you want to ‘tie lines together’ by telling Ruby to evaluate expressions split
over multiple lines, ignoring the line breaks, you can use the line continuation
character `\`. e.g

    x = (10 \
    + (2 * 4) )

The ability to treat data as executable code is called meta-programming.

Every time you embed an expression inside a double-quoted string you
are doing meta-programming. After all, the embedded expression is not
really program code – it is a string – and yet Ruby clearly has to
"turn it into" program code in order to be able to evaluate it.

You don’t even need to display the end result using print or puts.
Just placing a double-quoted string into your program will cause Ruby
to evaluate it

    "#{def x(s)
    puts(s.reverse)
    end;
    (1..3).each{x(aStr)}}"

For example, the Rails framework makes extensive use of meta-programming.

You may use meta-programming to explore artificial intelligence and
"machine learning". In fact, any application which would benefit from
having a program’s behaviour modified due to interaction during the
program’s execution is a prime candidate for meta-programming.

This is because anything read in by gets() is a string and
"#{exp}" evaluates it as a string and not as an expression, whereas
eval( exp ) evaluates a string as an expression.

The eval method can evaluate strings spanning many lines, making it
possible to execute an entire program embedded in a string.

There are some variations on the eval theme in the form of the
methods named `instance_eval`, `module_eval` and `class_eval`. 

The join method
forces the calling thread (for example the main thread) to suspend its own
execution (so it doesn’t just terminate the program) until the thread which calls
join has completed:

    * => stands for any number of characters
    . => stands for any single character
    ^ => start matching from the beginning of the line
    $ => start matching from the end
    \s => space character

    * after a spec => zero or more
    + after a spec => one or more

    // => Regex delimeter
    [] => Range delimiter

When you have two captures in your regexp delimiter, say, /(.)(.)/,
they both have to evaluate to true or else the response will be nil
and the $ variables will not be initialized.

    .sub(pattern, hash) -> string => replaces the first occurrence
    .gsub => replaces all occurrences.

`/(.)(.)(.)/` => The results got by comparing the three different
captures are stored in variables $1, $2 and $3. They are like 3
different matches being made "in turn", on the same string. Each
comparison is made on its own but they all have to pass otherwise non
of the $ variables will be initialized and it'll return nil.


The fact that parentheses are omitted in the method invocations makes
them look like references to named fields or named variables of the
object. This is intentional, but the fact is that Ruby is very strict
about encapsulation of its objects; there's no access to the internal
state of an object from the outside. Any such access must be mediated
by an accessor method, such as the class method.

    a = [1,2,3,4]                # Start with an array
    b = a.map {|x| x*x }         # Square elements: b is [1,4,9,16]
    c = a.select {|x| x%2==0 }   # Select even elements: c is [2,4]
    a.inject do |sum,x|          # Compute the sum of the elements => 10
      sum + x 
    end

Like the Array class, the Hash class also defines an each iterator
method. This method invokes the associated block of code once for
each key/value pair in the hash, and (this is where it differs from
Array) passes both the key and the value as parameters to the block: 

    h = {                         # A hash that maps number names to digits
      :one => 1,                  # The "arrows" show mappings: key=>value
      :two => 2                   # The colons indicate Symbol literals
    }  
    h[:one]                       # => 1.  Access a value by key
    h[:three] = 3                 # Add a new key/value pair to the hash
    h.each do |key,value|         # Iterate through the key/value pairs
      print "#{value}:#{key}; "   # Note variables substituted into string 
    end                           # Prints "1:one; 2:two; 3:three; "

Ruby's hashes can use any object as a key, but Symbol objects are the
most commonly used. Symbols are immutable, interned strings. They can
be compared by identity rather than by textual content (because two
distinct Symbol objects will never have the same content). 

The ability to associate a block of code with a method invocation is
a fundamental and very powerful feature of Ruby. Although its most
obvious use is for loop-like constructs, it is also useful for
methods that only invoke the block once. For example: 

    File.open("data.txt") do |f| # Open named file and pass stream to block
      line = f.readline          # Use the stream to read from the file
    end                          # Stream automatically closed at block end
    t = Thread.new do       # Run this block in a new thread
      File.read("data.txt") # Read a file in the background
    end                     # File contents available as thread value

Double-quoted strings can include arbitrary Ruby expressions
delimited by #{ and }. The value of the expression within these
delimiters is converted to a string (by calling its `to_s` method,
which is supported by all objects). The resulting string is then used
to replace the expression text and its delimiters in the string
literal. This substitution of expression values into strings is
usually called string interpolation. 

The return value of a method is the value of the last expression
evaluated in its body:

The Math module is part of the core Ruby library, and this code adds
a new method to it. This is a key feature of Ruby—classes and modules
are "open" and can be modified and extended at runtime. 

The (nonoverridable) = operator in Ruby assigns a value to a variable: 

    x = 1

Assignment can be combined with other operators such as + and -: 

    x += 1          # Increment x: note Ruby does not have ++.
    y -= 1          # Decrement y: no -- operator, either.

Ruby supports parallel assignment, allowing more than one value and
more than one variable in assignment expressions:

    x, y = 1, 2     # Same as x = 1; y = 2
    a, b = b, a     # Swap the value of two variables
    x,y,z = [1,2,3] # Array elements automatically assigned to variables

Methods in Ruby are allowed to return more than one value, and
parallel assignment is helpful in conjunction with such methods. For example: 

# Define a method to convert Cartesian (x,y) coordinates to Polar
    def polar(x,y)
      theta = Math.atan2(y,x)   # Compute the angle
      r = Math.hypot(x,y)       # Compute the distance
      [r, theta]                # The last expression is the return value
    end
    # Here's how we use this method with parallel assignment

    distance, angle = polar(2,2)

Methods that end with an equals sign (=) are special because Ruby
allows them to be invoked using assignment syntax. If an object o has
a method named x=, then the following two lines of code do the very
same thing: 

    o.x=(1)         # Normal method invocation syntax
    o.x = 1         # Method invocation through assignment

you'll notice punctuation characters at the start of Ruby variable
names: global variables are prefixed with `$`, instance variables are
prefixed with `@`, and class variables are prefixed with `@@`. These
prefixes can take a little getting used to, but after a while you may
come to appreciate the fact that the prefix tells you the scope of
the variable. The prefixes are required in order to disambiguate
Ruby's very flexible grammar.

    /[Rr]uby/        # Matches "Ruby" or "ruby"
    /\d{5}/          # Matches 5 consecutive digits
    1..3             # All x where 1 <= x <= 3
    1...3            # All x where 1 <= x < 3

Regexp and Range objects define the normal == operator for testing
equality. In addition, they also define the === operator for testing
matching and membership. Ruby's case statement (like the switch
statement of C or Java) matches its expression against each of the
possible cases using ===, so this operator is often called the case
equality operator. It leads to conditional tests like these: 

Code View:

    # Determine US generation name based on birth year
    # Case expression tests ranges with ===
    generation = case birthyear
                 when 1946..1963: "Baby Boomer"
                 when 1964..1976: "Generation X"
                 when 1978..2000: "Generation Y"
                 else nil
                 end
    # A method to ask the user to confirm something
    def are_you_sure?                  # Define a method. Note question mark!
      while true                       # Loop until we explicitly return
        print "Are you sure? [y/n]: "  # Ask the user a question
        response = gets                # Get her answer
        case response                  # Begin case conditional
        when /^[yY]/                   # If response begins with y or Y
          return true                  # Return true from the method
        when /^[nN]/, /^$/             # If response begins with n,N or is empty
          return false                 # Return false
        end
      end
    end


Once you have Ruby installed, you can invoke the Ruby interpreter
with the ruby command: 

    % ruby -e 'puts "hello world!"'
    hello world!

The -e command-line option causes the interpreter to execute a single
specified line of Ruby code. More commonly, you'd place your Ruby
program in a file and tell the interpreter to invoke it: 

    % ruby hello.rb
    hello world!

you'll also want to know how to use three tools—irb, ri, and gem—that
are bundled with the interpreter.

Loosely speaking, puts prints a string of text to the console and
appends a newline (unless the string already ends with one). If
passed an object that is not a string, puts calls the `to_s` method
of that object and prints the string returned by that method. print
does more or less the same thing, but it does not append a newline. 

Invoke ri on the command line followed by the name of a Ruby class,
module, or method, and ri will display documentation for you.

Normally, you can separate a class or module name from a method name
with a period. If a class defines a class method and an instance
method by the same name, you must instead use `::` to refer to the
class method or `#` to refer to the instance method.

Opinions differ as to what "ri" stands for. It has been called "Ruby
Index," "Ruby Information," and "Ruby Interactive."

the gem install command installs the most recent version of the gem
you request and also installs any gems that the requested gem
requires. gem has other useful subcommands as well. Some examples: 

    gem list               # List installed gems
    gem enviroment         # Display RubyGems configuration information
    gem update rails       # Update a named gem
    gem update             # Update all installed gems
    gem update --system    # Update RubyGems itself
    gem uninstall rails    # Remove an installed gem

The rubygems module is part of the standard library in Ruby 1.9, but
it is no longer required to load gems. Ruby 1.9 knows how to find
installed gems on its own, and you do not have to put require
'rubygems' in your programs that use gems.

When you load a gem with require (in either 1.8 or 1.9), it loads the
most recent installed version of the gem you specify. If you have
more specific version requirements, you can use the gem method before
calling require. This finds a version of the gem matching the version
constraints you specify and "activates" it, so that a subsequent
require will load that version: 

    require 'rubygems'               # Not necessary in Ruby 1.9
    gem 'RedCloth', '> 2.0', '< 4.0' # Activate RedCloth version 2.x or 3.x
    require 'RedCloth'               # And now load it


Note 2 self

    irb(main):002:0> puts "Hello World"
    Hello World
    => nil

`puts` is the basic command to print something out in Ruby. But then
what’s the `=> nil` bit? That’s the result of the expression. puts
always returns nil, which is Ruby’s absolutely-positively-nothing value.

Example of Ruby framework = Rails
Example of Ruby tool = rake, gem e.t.c
