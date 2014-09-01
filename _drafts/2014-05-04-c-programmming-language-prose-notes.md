---
layout: post
title: "C programmming Language prose notes"
description: "prose notes about the C programming language"
category: "Programming"
tags: ["C", "programming", "imperative programming"]
---
{% include JB/setup %}

#Introduction
In general, computer languages deal with two concepts: *data* and
*algorithms*.

The *data* constitutes the information a program uses and processes.

The *algorithms* are the methods the progam uses.

C is a procedural language, that means it emphasizes the algorithm side
of programming. Conceptually, procedural programming consists of
figuring out the actions a computer should take and then using the
programming language to implement those actions. A program prescribes a
set of procedures for the computer to follow to produce a particular
outcome, much as a recipe prescribes a set of procedures for a cook to
follow to produce a cake

BCPL and B are <i>typeless</i> languages. By contrast, C provides a variety
of data types. The fundamental types are:
 - characters, 
 - and integers 
 - and floating point numbers of several sizes.
In addition, there is a hierarchy of derived data types created with
pointers, arrays, structures and unions.

Expressions are formed from operators and operands; any
expression, including an assignment or a function call, can be a statement.

Pointers provide for machine-independent address arithmetic.

C provides the fundamental control-flow constructions required for
well-structured programs: 
  - statement grouping, decision making (if-else),
  - selecting one of a set of possible values (switch),
  - looping with the termination test at the top (while, for)
  - or at the bottom (do), and 
  - early loop exit (break)

Functions may return values of basic types, structures, unions, or
pointers. Any function may be called recursively. Local variables are
typically Local variables are typically "automatic", or created anew
with each invocation. Function definitions may not be nested but
variables may be declared in a block structured fashion. The functions
of a c program may exist in separate source files that are compiled
separately. Variables may be internal to a function, external, but known
only within a single source file, or visible to the entire program.

<blockquote>
Note to self
  c variable scope:
    - function local
    - source file local
    - global
end note
</blockquote>

A preprocessing step performs macro substitution on program text,
inclusion of other source files and conditonal compilation.

C is a relatively "low level" language. This characterization is not
pejorative; it simply means that C deals with the same sort of objects
that most computers do, namely characters, numbers and addresses. These
may be combined and moved about by the arithmetic and logical operators
implemented by real machines.

C provides no operations to deal directly with composite objects such as
character strings, sets, lists or arrays. There are no operations that
manipulate an entire array or string, although structures may be copied
as a unit. The language does not define any storage allocation facility
other than static definition and stack discipline provided by the local
variables of functions; there's no heap or garbage collection.
Finally, C itself provides no input/output facilities; there are no READ
or WRITE statements, and no builtin file access methods. ALl of these
higher level mechanisms must be provided by explicitly called functions.
Most C implementations have included a reasonably standard collection of
such functions.

Similarly, C offers only straighforward, single-thread control flows:
tests, loops, grouping, and subprograms, but not multiprogramming,
parallel operations, synchronization, or coroutines.

Although the absense of some of these features may seem like a grave
deficiency, ("You mean I have to call a function to compare two
character strings?"), keeping the language down to modest size has real
benefits. Since C is relatively small, it can be described in a small
space and learned quickly. A programmer can reasonably expect to know
and understand and indeed regurlarly use the entire language.

ANSI = American National Standards Institute.

C is not a strongly-typed language, but as it has evolved, its
type-checking has been strengthened.
C retains the basic philosophy that programmers know what they are doing;
it only requires that they state their intentions explicitly.

A C program, whatever its size, consists of *functions* and *variables*.
 - A *function* contains statements that specify the computing operations to
be done.
- *Variables* store values used during the computation.

The statements of a function are enclosed in braces { }

A function is called by naming it, followed by a parenthesized list of arguments.

printf never supplies a newline character automatically, so several calls may be used to build up an output line
in stages. Our first program could just as well have been written

    #include <stdio.h>
    int main(void)
    {
        printf("hello, ");
        printf("world");
        printf("\n");
    }

In C, all variables must be declared before they are used, usually at the 
beginning of the function before any executable statements. A 
declaration announces the properties of variables; it consists of a name and a 
list of variables, such as
int fahr, celsius;
int lower, upper, step;

The type `int` means that the variables listed are integers; by
contrast with `float`, which means floating point, i.e., numbers that
may have a fractional part. The range of both `int` and `float`
depends on the machine you are using; 16-bits ints, which lie between
-32768 and +32767, are common, as are 32-bit ints. A float number is
typically a 32-bit quantity, with at least six significant digits and
magnitude generally between about 10-38 and 1038.

C provides several other data types besides int and float, including:
- char : character - a single byte
- short : short integer.
- long : long integer
- double : double-precision floating point.

The size of these objects is also machine-dependent. There are also arrays, structures and unions of these basic
types, pointers to them, and functions that return them, all of which we will meet in due course.

Computation in the temperature conversion program begins with the assignment statements
  lower = 0;
  upper = 300;
  step = 20;

The while loop operates as follows: The condition in parentheses is tested. If it is true (fahr is less than or equal
to upper), the body of the loop (the three statements enclosed in braces) is executed. Then the condition is re-
tested, and if true, the body is executed again. When the test becomes false (fahr exceeds upper) the loop ends,
and execution continues at the statement that follows the loop. There are no further statements in this program, so
it terminates.

The body of a while can be one or more statements enclosed in braces, as in the temperature converter, or a
single statement without braces, as in

    while (i < j)
      i = 2 * i;

In either case, we will always indent the statements controlled by the while by one tab stop (which we have
shown as four spaces) so you can see at a glance which statements are inside the loop. The indentation emphasizes
the logical structure of the program. Although C compilers do not care about how a program looks, proper
indentation and spacing are critical in making programs easy for people to read. We recommend writing only one
statement per line, and using blanks around operators to clarify grouping. The position of braces is less important,
although people hold passionate beliefs. We have chosen one of several popular styles. Pick a style that suits you,
then use it consistently.

The reason for multiplying by 5 and dividing by 9 instead of just multiplying by 5/9 is that in C, as in many other
languages, integer division truncates: any fractional part is discarded. Since 5 and 9 are integers. 5/9 would be
truncated to zero and so all the Celsius temperatures would be reported as zero.

printf is a general-purpose output formatting
function, which we will describe in detail in Chapter 7. Its first argument is a string of characters to be printed,
with each % indicating where one of the other (second, third, ...) arguments is to be substituted, and in what form it
is to be printed. For instance, %d specifies an integer argument, so the statement
printf("%d\t%d\n", fahr, celsius);
causes the values of the two integers fahr and celsius to be printed, with a tab (\t) between them.
Each % construction in the first argument of printf is paired with the corresponding second argument, third
argument, etc.; they must match up properly by number and type, or you will get wrong answers.

By the way, printf is not part of the C language; there is no input or output defined in C itself. printf is just a
useful function from the standard library of functions that are normally accessible to C programs. The behaviour of
printf is defined in the ANSI standard, however, so its properties should be the same with any compiler and
library that conforms to the standard.

scanf is like printf, except that it reads input instead of writing output

There are a couple of problems with the temperature conversion program. The simpler one is that the output isn't
very pretty because the numbers are not right-justified. That's easy to fix; if we augment each %d in the printf
statement with a width, the numbers printed will be right-justified in their fields. For instance, we might say
printf("%3d %6d\n", fahr, celsius);
to print the first number of each line in a field three digits wide, and the second in a field six digits wide.

The more serious problem is that because we have used integer arithmetic, the Celsius temperatures are not very
accurate; for instance, 0oF is actually about -17.8oC, not -17. To get more accurate answers, we should use floating-
point arithmetic instead of integer.


This is much the same as before, except that fahr and celsius are declared to be float and the formula for
conversion is written in a more natural way. We were unable to use 5/9 in the previous version because integer
division would truncate it to zero. A decimal point in a constant indicates that it is floating point, however, so
5.0/9.0 is not truncated because it is the ratio of two floating-point values.

If an arithmetic operator has integer operands, an integer operation is performed. If an arithmetic operator has one
floating-point operand and one integer operand, however, the integer will be converted to floating point before the
operation is done. If we had written (fahr-32), the 32 would be automatically converted to floating point.
Nevertheless, writing floating-point constants with explicit decimal points even when they have integral values
emphasizes their floating-point nature for human readers.

The printf conversion specification %3.0f says that a floating-point number (here fahr) is to be printed at
least three characters wide, with no decimal point and no fraction digits. %6.1f describes another number
(celsius) that is to be printed at least six characters wide, with 1 digit after the decimal point. 

Width and precision may be omitted from a specification: %6f says that the number is to be at least six characters
wide; %.2f specifies two characters after the decimal point, but the width is not constrained; and %f merely says
to print the number as floating point.

`%d` : print as decimal integer
`%6d` : print as decimal integer, at least 6 characters wide
`%f` : print as floating point
`%6f` : print as floating point, at least 6 characters wide
`%.2f` : print as floating point, 2 characters after decimal point
`%6.2f` : print as floating point, at least 6 wide and 2 after decimal point


Among others, printf also recognizes %o for octal, %x for hexadecimal, %c for character, %s for character
string and %% for itself.

1.3 The for statement

    for (initial; test; step)
        stuff to do;


Symbolic Constants
A #define line defines a symbolic name or symbolic constant to be a particular string of characters:
  #define name replacement list

Thereafter, any occurrence of name (not in quotes and not part of another name) will be replaced by
the corresponding replacement text. The name has the same form as a variable name: a sequence of
letters and digits that begins with a letter. The replacement text can be any sequence of characters;
it is not limited to numbers.

    #include <stdio.h>
    #define LOWER 0 /* lower limit of table */
    #define UPPER 300 /* upper limit of table */
    #define STEP 20 /* step size */

    /* print Fahrenheit-Celsius table */

    main()
    {
      int fahr;
      for (fahr = LOWER; fahr <= UPPER; fahr = fahr + STEP)
        printf("%3d %6.1f\n", fahr, (5.0/9.0)*(fahr-32));
    }

The quantities LOWER, UPPER and STEP are symbolic constants, not variables, so they do not appear in
declarations. Symbolic constant names are conventionally written in upper case so they can ber readily
distinguished from lower case variable names. Notice that there is no semicolon at the end of a #define line.

Note to self:
    #define [symbolic_constant] value
  no semicolons at the end and no equal signs
  define is use as constants are not variables and hence aren't declared
  like say int [var];
end note


Character Input and Output
We're going to consider a family of related programs for processing
character data. You will find that many programs are just expanded
versions of the prototypes that we discuss here.

The model of input and output supported by the standard library is very
simple. 
Text input or output, regardless of where it originates or where it goes
to, is dealt with as streams of characters. A text stream is a sequence
of characters divided into lines; each line consists of zero or more
characters followed by a newline character. It is the responsibility of
the library to make each input or output stream confirm to this model;
the C programmer using the library need not worry about how lines are
represented outside the program.

The standard library provides several functions for reading or writing
one character at a time, of which
  getchar and
  putchar 
are the simplest. 
Each time it is called, getchar reads the next input character from a
text stream and returns that as its value. That is, after
  c = getchar();
the variable c contains the next character of input.

The function putchar() prints a character each time it is called:
  putchar(c);

the statement above prints the contents of the integer variable c as a
character, usually on the screen. Calls to putchar() and printf may be
interleaved; the output will appear in the order in which the calls are
made.

The problem is distinguishing the end of input from valid data. The solution 
is that getchar returns a distinctive value when there is no more input, a 
value that cannot be confused with any real character. This value is called 
EOF, for "end of file". We must declare c to be a type big enough to hold 
any value that getchar returns. We can't use char since c must be big enough 
to hold EOF in addition to any possible char. Therefore we use int.


EOF is an integer defined in `stdio.h`, but the specific numeric value doesn't 
matter as long as it is not the same as any char value. By using the symbolic 
constant, we are assured that nothing in the program depends on the
specific numeric value.

In C, any assignment, such as
  c = getchar();
is an expression and has a value, which is the value of the left hand side after the assignment.
This means that a assignment can appear as part of a larger expression.


    #include <stdio.h>
    /* copy input to output; 2nd version */
    main()
    {
      int c;

      while ((c = getchar()) != EOF)
        putchar(c);
    }

The precedence of != is higher than
that of =

1.5.2 Character Counting

printf uses %f for both float and double; %.0f suppresses the printing of the decimal point and the
fraction part, which is zero.
The body of this for loop is empty, because all the work is done in the test and increment parts. But the
grammatical rules of C require that a for statement have a body. The isolated semicolon, called a null statement,
is there to satisfy that requirement. We put it on a separate line to make it visible.


Before we leave the character counting program, observe that if the input contains no characters, the while or
for test fails on the very first call to getchar, and the program produces zero, the right answer. This is
important. One of the nice things about while and for is that they test at the top of the loop, before proceeding
with the body. If there is nothing to do, nothing is done, even if that means never going through the loop body.
Programs should act intelligently when given zero-length input. The while and for statements help ensure that
programs do reasonable things with boundary conditions.


The process of compiling and linking a program is often called building.

in C, it does not matter where on the line you begin typing—you can begin typing your
statement at any position on the line.


If you’re using the standard Unix C compiler, the command is cc instead of gcc. 


Linux comes with the GNU C compiler by default.

You can also specify a different name for the executable file at the time the program is
compiled.This is done with the –o (that’s the letter O) option, which is followed by the
name of the executable. For example, the command line

    $ gcc prog1.c –o prog1

All programme statements in C must be terminated by a semicolon that
appears immediately following the closing parenthesis of the printf
call.

the return 0; statement says to finish the execution of main, and return
to the system a status value of 0. You can use any integer here. Zero is
used by convention to indicate that a program completed successfully -
that is, without running into any errors. Different numbers can be used
to indicate different types of error conditions that occured (such as a
file not being found). This exit status can be tested by other programs
(such as the Unix shell) to see whether the program ran successfully.


Note to self:
Features of C functions:
  - Return type
  - Argument types
  - Return values, optional as function may be void
end note


The printf routine call in Program 3.4 now has two items or arguments enclosed
within the parentheses.These arguments are separated by a comma.The first argument to
the printf routine is always the character string to be displayed. However, along with
the display of the character string, you might also frequently want to have the value of
certain program variables displayed. In this case, you want to have the value of the vari-
able sum displayed at the terminal after the characters
The sum of 50 and 25 is
are displayed.The percent character inside the first argument is a special character recog-
nized by the printf function.The character that immediately follows the percent sign
specifies what type of value is to be displayed at that point. In the preceding program, the
letter i is recognized by the printf routine as signifying that an integer value is to be
displayed.2
Whenever the printf routine finds the %i characters inside a character string, it
automatically displays the value of the next argument to the printf routine. Because
sum is the next argument to printf, its value is automatically displayed after the charac-
ters “The sum of 50 and 25 is” are displayed.


Note to self
  printf (String) -> String
  printf ("string %(type)", var) -> String + var

  e.g;
  printf ("The sum of %i and %i is %i\n", value1, value2, sum);
end note


Note that printf also allows you to specify %d format characters to display an integer.This
book consistently uses %i throughout the remaining chapters.

Commenting:

    /*
     * Multi-line
     * comment
     */ 

    // single line


Variables can be used to store:
  - Integers
  - Floating point numbers
  - characters
  - Pointers to locations inside the computer's memory.


The rules for forming variable names are quite simple: They must begin
with a letter or underscore and can be followed by any combination
of letters (upper- or lowercase), underscores, or digits 0-9. THe

The C programming language provides four other basic data types: 
 - `float` (values containing decimal points), 
 - `double`,
 - `char`, and 
 - `_Bool`

The double type is the same as
type float, only with roughly twice the precision.

The char data type can be used to
store a single character, such as the letter a, the digit character 6,
or a semicolon.

Finally, the `_Bool` data type can be used to store just the values 0 or 1.
Variables of this type are used for indicating an on/off, yes/no, or true/false situation.

In C, any number, single character, or character string is known as a constant. For
example, the number 58 represents a constant integer value.The character string
"Programming in C is fun.\n" is an example of a constant character string.
Expressions consisting entirely of constant values are called constant expressions. So, the
expression
128 + 7 - 17
is a constant expression because each of the terms of the expression is a constant value.
But if i were declared to be an integer variable, the expression
128 + 7 – i
would not represent a constant expression.


Note to self
  Constant is to C, what Literal is to Ruby
end note


## The Basic Integer Type int

In C, an integer consists of a sequence of one or more digits. A minus
sign preceeding the sequence indicates that the value is negative. The
values 158, -10, and 0 are all valid examples of integer constants. No
embedded spaces are permitted between the digits, and the values larger
than 999 cannot be expressed using commas. (So, the value 12,000 is not
a valid integer constant and must be written as 12000.)

Two special formats in C enable integer constants to be expressed in a
base other than decimal (base 10). 
If the first digit of an integer is 0, the integer is taken as ezpressed
in Octal notation - that is in base 8. In that case the remaining digits
of the value must be valid base-8 digits and therefore, must be 0-7. So,
to express the value 50 in base 8 in C, which is equivalent to the value
40 in decimal, the notation 050 is used. Similarly, the octal constant
0177 represents the decimal value 127 (1 X 64 + 7 X 8 + 7).
An integer value can be displayed at the terminal in octal notation by
using the format character %o in the format string of a printf
statement. In such a case, the value is displayed in octal without a
leading zero. The format characters %#o do cause a leading zero to be
displayed before an octal value.

If an integer constant is preceded by a zero and a letter x (either
lowercase or uppercase), the value is taken as being expressed in
hexadecimal (base 16) notation.

Immediately following the letter x are digits of the hexadecimal value,
which can be composed of the digits 0-9 and the letters a-f(or A-F). The
letters represent the values 10-15, respectively. So, to assign the
hexadecimal value FFEF0D to an integer variable called rgbColr, the
statement
  rgbColor = 0xFFEF0D;
 
can be used. The format characters %x display a value in hexadecimal
format without the leading 0x, and using lowercase letters a-f for
hexadecimal digits. To display the value with the leading 0x, you use
the format characters %#x, as in the following:
  printf ("Colr is %#x\n", rgbColor);

An uppercase x, as in %X, or %#X can be used to display the leading x
and the hexadecimal digits that follow using uppercase letters.

## Storage Sizes and Ranges
Every value, whether its a character, integer or floating-point number,
has a range of values associated with it. This range has to do with the
amount of storage that is allocated to store a particular type of data.
In general, that amount is not defined in the language.
It typically depends on the computer you're running, and is, therefore,
called "Implementation- or machine-dependent". 
For example an integer might take up 32 bits on your computer, or
perhaps it might be stored in 64. You should never write programs that
make assumptions about the size of your data types. You are however
guaranteed that a minimum amount of storage will be set aside for each
basic data type. For example, its guaranted that an integer value will
be stored in a minimum of 32 bits of storage, which is the size of a
"word" on many computers.

## The Floating Number Type, float

A variable declared to be of type float can be used for storing values containing deci-
mal places. A floating-point constant is distinguished by the presence of a decimal point.
You can omit digits before the decimal point or digits after the decimal point, but obvi-
ously you can’t omit both.The values 3., 125.8, and –.0001 are all valid examples of
floating-point constants.To display a floating-point value at the terminal, the printf
conversion characters %f are used.
Floating-point constants can also be expressed in scientific notation.The value 1.7e4 is
a floating-point value expressed in this notation and represents the value 1.7 × 10–4.
The value before the letter e is known as the mantissa, whereas the value that follows is
called the exponent.This exponent, which can be preceded by an optional plus or minus
sign, represents the power of 10 by which the mantissa is to be multiplied. So, in the
constant 2.25e–3, the 2.25 is the value of the mantissa and –3 is the value of the expo-
nent.This constant represents the value 2.25 × 10–3, or 0.00225. Incidentally, the letter
e, which separates the mantissa from the exponent, can be written in either lowercase or
uppercase.

To display a value in scientific notation, the format characters %e should be specified
in the printf format string.The printf format characters %g can be used to let printf
decide whether to display the floating-point value in normal floating-point notation or
in scientific notation.This decision is based on the value of the exponent: If it’s less than
–4 or greater than 5, %e (scientific notation) format is used; otherwise, %f format is used.
Use the %g format characters for displaying floating-point numbers—it produces the
most aesthetically pleasing output.
A hexadecimal floating constant consists of a leading 0x or 0X, followed by one or
more decimal or hexadecimal digits, followed by a p or P, followed by an optionally
signed binary exponent. For example, 0x0.3p10 represents the value 3/16 × 210 = 192.


## The extended Precision Type, double
Type double is very similar to type float, but it is used whenever the range provided by
a float variable is not sufficient.Variables declared to be of type double can store
roughly twice as many significant digits as can a variable of type float. Most computers
represent double values using 64 bits.
Unless told otherwise, all floating-point constants are taken as double values by the C
compiler.To explicitly express a float constant, append either an f or F to the end of
the number, as follows:
12.5f
To display a double value, the format characters %f, %e, or %g, which are the same format
characters used to display a float value, can be used.


## The Single Character Type, char
A char variable can be used to store a single character.1 A character constant is formed
by enclosing the character within a pair of single quotation marks. So 'a', ';', and '0'
are all valid examples of character constants.The first constant represents the letter a, the
second is a semicolon, and the third is the character zero—which is not the same as the
number zero. Do not confuse a character constant, which is a single character enclosed
in single quotes, with a character string, which is any number of characters enclosed in
double quotes.
The character constant '\n'—the newline character—is a valid character constant
even though it seems to contradict the rule cited previously.This is because the backslash
character is a special character in the C system and does not actually count as a charac-
ter. In other words, the C compiler treats the character '\n' as a single character, even
though it is actually formed by two characters.There are other special characters that are
initiated with the backslash character. 

The format characters %c can be used in a printf call to display the value of a char
variable at the terminal.


## The Boolean Data Type `_Bool`

A `_Bool` variable is defined in the language to be large enough to store just the values 0
and 1.The precise amount of memory that is used is unspecified. `_Bool` variables are
used in programs that need to indicate a Boolean condition. For example, a variable of
this type might be used to indicate whether all data has been read from a file.
By convention, 0 is used to indicate a false value, and 1 indicates a true value.When
assigning a value to a `_Bool` variable, a value of 0 is stored as 0 inside the variable,
whereas any nonzero value is stored as 1.
To make it easier to work with `_Bool` variables in your program, the standard header
file `<stdbool.h>` defines the values bool, true, and false.


Certain floating-point
values cannot be exactly represented inside the computer’s memory.

When displaying the values of float or double variables, you have the choice of
three different formats.The %f characters are used to display values in a standard manner.
Unless told otherwise, printf always displays a float or double value to six decimal
places rounded.You see later in this chapter how to select the number of decimal places
that are displayed.
The %e characters are used to display the value of a float or double variable in sci-
entific notation. Once again, six decimal places are automatically displayed by the system.
With the %g characters, printf chooses between %f and %e and also automatically
removes from the display any trailing zeroes. If no digits follow the decimal point, it
doesn’t display that either.


Remember that whereas a character string (such as the first argument to printf) 
is enclosed within a pair of double quotes, a character constant must always be 
enclosed within a pair of single quotes.


## Type Specifiers: long, long long, short, unsigned, and signed
If the particular long is placed before the int declaration, the
declared integer variable is of extended range on some computer systems.
An example of a long int declaration might be
  long int factorial;

This declares the variable factorial to be a long integer variable. As
with floats and doubles, the particular accuracy of a long variable
depends on your particular computer system. On many computer systems, an
int and a long int both have the same range and either can be used to
store integer values upto 32 bits wide (2^31 - 1, or 2,147,483,647).

A constant value of type long int is formed by optionally appending the
letter L (upper- or lowercase) onto the end of an integer constant. No
spaces are permitted between the number and the L. So the declaration:
    long int number_of_points = 131071100L;

declares the variable `number_of_points` to be of type `long int` with an
initial value of `131,071,100`
  To display the value of a long int using printf, the letter "l" is usd
as a modifier before the integer format characters: i,o and x. 
This means that the format characters %li can be used to display the
value of a long int in the decimal format, the characters %lo can
display the value in octal format, and the characters %lx can display
the value in hexadecim al format.

There's also a long long integer data type, so
  long long int maxAllowedStorage;

declares the indicated variable to be of the specified extendeed range,
which is guanteed to be at least 64 bits wide. Instead of a single
letter l, two l's are used in the printf string to display long long
integers, as in %lli.

The long specifier is also allowed infront of a double declaration, as
follows:
    long double US_deficit_2004;

A long double constant is written as a floating point constant with the
letter l or L immediately following, such as
    1.234e+7L

To display a long double, the L modifier is used. So, %Lf displays a
long double value in floating-point notation, %Le displays the same
value in scientic notation, and %Lg tells printf to chose between % Lf
and %Le.

The specifier "short" when placed infront of an int declaration, tells
the C compiler that the particular variable being declared is used to
store fairly small integer values. The motivation for using short
variables is primarily one of conserving memory space, which can be an
iissue in situations where the program needs a lot of memory and the
amount of available memory is limited.

On some machines, a short int takes up half the amount of storage as
a regular int variable does. In any case, you are guaranteed that the
amount of space allocated for a short int will not be less than 16
bits. There is no way to explicitly write a constant of type short
int in C.To display a short int variable, place the letter h in front
of any of the normal integer conversion characters: `%hi`, `%ho`, or
`%hx`. Alternatively, you can also use any of the integer conversion
characters to display short ints, due to the way they can be
converted into integers when they are passed as arguments to the
printf routine. The final specifier that can be placed in front of an
int variable is used when an integer variable will be used to store
only positive numbers.The declaration `unsigned int counter;`
declares to the compiler that the variable `counteri` is used to
contain only positive values. By restricting the use of an integer
variable to the exclusive storage of positive integers, the range of
the integer variable is extended. An unsigned int constant is formed
by placing the letter u (or U) after the constant,
as follows:
    0x00ffU
You can combine the letters u (or U) and l (or L) when writing an
integer constant, so
    20000UL

tells the compiler to treat the constant 20000 as an unsigned long.

An integer constant that’s not followed by any of the letters u, U,
l, or L and that is too large to fit into a normal-sized int is
treated as an unsigned int by the compiler. If it’s too small to fit
into an unsigned int, the compiler treats it as a long int. If it still
can’t fit inside a long int, the compiler makes it an unsigned long
int. If it doesn’t fit there, the compiler treats it as a long long
int if it fits, and as an unsigned long long int otherwise.
When declaring variables to be of type long long int, long int, short
int, or unsigned int, you can omit the keyword int.Therefore, the
unsigned variable counter could have been equivalently declared as
follows: 
  unsigned counter;

You can also declare char variables to be unsigned.
The signed qualifier can be used to explicitly tell the compiler that
a particular variable is a signed quantity.

### Table 4.1

<table>
<caption> the basic data types and qualifiers. </caption>
    <thead>
    <tr> <th>Type</th> <th>Constant Examples</th>  <th>printf chars</th>
    </tr>
    </thead>
    <tbody>
    <tr> <td>char</td> <td>'a', '\n'</td>       <td>%c </td> </tr>
    <tr> <td>`_Bool`</td> <td> 0,1</td> <td>%i, %u</td> </tr>
    <tr> <td>short int</td> <td> ______</td> <td> %hi, %hx, %ho </td> </tr>
    <tr> <td>unsigned short int </td><td>______</td> <td> %hu, %hx, %ho
    </td></tr>
    <tr> <td> int </td> <td> 12, -97, 0xFFE0,0177 </td>  <td> %i, %x, %o
    </td> </tr>
    <tr> <td> unsigned int</td> <td>12u, 100U, 0XFFu</td> <td> %u, %x,
    %o</td> </tr>
    <tr> <td> long int </td> <td>12L, -2001, 0xffffL</td> <td> %li, %lx, %lo </td> </tr>
    <tr><td>unsigned long int</td>  <td>12UL, 100ul, 0xffeeUL</td> <td> %lu, %lx, %lo
    </td></tr>
    <tr><td>long long int</td> <td>0xe5e5e5e5LL, 500ll</td> <td>%lli, %llx, %llo</td></tr>
    <tr> <td>unsigned long long int</td> <td>12ull, 0xffeULL</td> <td> %llu, %llx, %llx</td> </tr>
    <tr><td>float</td>   <td>12.34f, 3.1e-5f, 0x1.5p10, 0x1P-1 </td><td>%f, %e, %g, %a</td> </tr>
    <tr><td>double</td>  <td>12.34, 3.1e5, 0x.1p3</td> <td>%f, %e, %g, %a</td> </tr>
    <tr><td>long double</td> <td>12.341, 3.1e-51</td>  <td> %Lf, %Le,
    %Lg</td></tr>
    </tbody>
</table>

Note
    1.7 × 10–4.
The value before the letter e is known as the "mantissa", whereas the
value that follows is called the "exponent".

## Working with Arithmetic Expressions
In C, just as in virtually all programming languages, the plus sign
"+" is used to add two values, the minus sign  "–" is used to
subtract two values, the asterisk `*` is used to multiply two values,
and the slash (/) is used to divide two values.These operators are known
as binary arithmetic operators because they operate on two values or terms.

Expressions containing operators of the same precedence
are evaluated either from left to right or from right to left,
depending on the operator. This is known as the associative
property of an operator.

## The Modulus Operator
As you know, printf uses the character that immediately follows the percent sign to
determine how to print the next argument. However, if it is another percent sign that
follows, the printf routine takes this as an indication that you really intend to display a
percent sign and inserts one at the appropriate place in the program’s output.

e.g;
   printf ("a %% d = %i\n", a % d);   // => a % d = 4

A program statement can be continued to the next line at any point at
which a blank space could be used.


Whenever a floating-point value is assigned to an integer variable in C, the decimal
portion of the number gets truncated. So, when the value of f1 is assigned to i1 in the
previous program, the number 123.125 is truncated, which means that only its integer
portion, or 123, is stored in i1.The first line of the program’s output verifies that this is
the case.
Assigning an integer variable to a floating variable does not cause any change in the
value of the number; the value is simply converted by the system and stored in the float-
ing variable.The second line of the program’s output verifies that the value of i2 (–150)
was correctly converted and stored in the float variable f1.
The next two lines of the program’s output illustrate two points that must be remem-
bered when forming arithmetic expressions.The first has to do with integer arithmetic,
which was previously discussed in this chapter.Whenever two operands in an expression
are integers (and this applies to short, unsigned, long, and long long integers as well),
the operation is carried out under the rules of integer arithmetic.Therefore, any decimal
portion resulting from a division operation is discarded, even if the result is assigned to a
floating variable (as you did in the program).Therefore, when the integer variable i2 is
divided by the integer constant 100, the system performs the division as an integer divi-
sion.The result of dividing –150 by 100, which is –1, is, therefore, the value that is stored
in the float variable f1.
The next division performed in the previous listing involves an integer variable and a
floating-point constant. Any operation between two values in C is performed as a float-
ing-point operation if either value is a floating-point variable or constant.Therefore,
when the value of i2 is divided by 100.0, the system treats the division as a floating-
point division and produces the result of –1.5, which is assigned to the float variable f1.

##The Type Cast Operator
The last division operation from Program 4.5 that reads
f2 = (float) i2 / 100;
// type cast operator
introduces the type cast operator.The type cast operator has the effect of converting the
value of the variable i2 to type float for purposes of evaluation of the expression. In no
way does this operator permanently affect the value of the variable i2; it is a unary oper-
ator that behaves like other unary operators. Because the expression –a has no perma-
nent effect on the value of a, neither does the expression (float) a.
The type cast operator has a higher precedence than all the arithmetic operators
except the unary minus and unary plus. Of course, if necessary, you can always use
parentheses in an expression to force the terms to be evaluated in any desired order.
As another example of the use of the type cast operator, the expression
(int) 29.55 + (int) 21.99
is evaluated in C as
29 + 21
because the effect of casting a floating value to an integer is one of truncating the
floating-point value.The expression
(float) 6 / (float) 4
produces a result of 1.5, as does the following expression:
(float) 6 / 4


## Combining Operations with Assignment: The
Assignment Operators

The C language permits you to join the arithmetic operators with the assignment opera-
tor using the following general format: op=

In this format, op is any of the arithmetic operators, including +, –, ×, /, and %. In
addition, op can be any of the bit operators for shifting and masking, which is discussed
later.
Consider this statement:
count += 10;
The effect of the so-called “plus equals” operator += is to add the expression on the right
side of the operator to the expression on the left side of the operator and to store the
result back into the variable on the left-hand side of the operator. So, the previous state-
ment is equivalent to this statement:
count = count + 10;
The expression
counter -= 5
uses the “minus equals” assignment operator to subtract 5 from the value of counter and
is equivalent to this expression:
counter = counter - 5
A slightly more involved expression is:
a /= b + c
which divides a by whatever appears to the right of the equal sign—or by the sum of b
and c—and stores the result in a.The addition is performed first because the addition
operator has higher precedence than the assignment operator. In fact, all operators but
the comma operator have higher precedence than the assignment operators, which all
have the same precedence.
In this case, this expression is identical to the following:
a = a / (b + c)
The motivation for using assignment operators is threefold. First, the program statement
becomes easier to write because what appears on the left side of the operator does not
have to be repeated on the right side. Second, the resulting expression is usually easier to
read.Third, the use of these operators can result in programs that execute more quickly
because the compiler can sometimes generate less code to evaluate an expression.


## Types `_Complex` and `_Imaginary`
Before leaving this chapter it is worthy to note two other types in the language called
`_Complex` and `_Imaginary` for working with complex and imaginary numbers.
Support for `_Complex` and `_Imaginary` types is optional for a compiler.



## Program Looping

If you arrange 15 dots in the shape of a triangle, you end up with an
arrangement that might look like this:

            •
          •  •
        •  •  •
      •  •  •  •
    •  •  •  •  •

The first row of the triangle contains one dot, the secod row contains
two dots, and so on. 

In general, the number of dots it takes to form a
triangle containing n rows is the sum of integers from 1 through n.
This sum is known as a "triangular number". 

If you start at 1, the fourth triangular number is the sum of the
consecutive integers 1 through 4 ( 1 + 2 + 3 + 4), or 10.

Suppose you want to write a program that calculates and displays the
value of the eighth triangular number at the terminal. Obviously you
could easily calculate this number in your head, but for the sake of
argument, assume that you want to write a program in C to perform this
task. Such a program is shown below:

    // Program to calculate the eighth triangular Number

    #include <stdio.h>

    int main(void)
    {
        int triangularNumber;

        triangularNumber = 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8;

        printf ("The eighth triangular number is %i\n", triangularNumber);

        return 0;
    } 

The technique above works fine for calculating relatively small,
triangular numbers. But what happens if you need to find the value of
the 200th triangular number, for ecample? It certainly would be tedious
to modify the program above to explicitly add up all of the integers
from 1 to 200. Luckily there's an easier way.

One of the fundamental properties of a computer is its ability to
repetitively execute a set of statements. These looping capabilities
enable you to develop concise programs containing repetitive processes
that could otherwise require thousands or even millions of program
statements to perform.

The C programming language contains three different program statements
for program looping. These are:
  - the for statement,
  - the while statement and 
  - the do statement.

## for statement

The general format of the for statement is as follows:

  for ( init_expression; loop_condition; loop_expression )
    program statement


## Relational Operators
Table 5.1 lists all the relational operators that are available in C.

- == 
- !=
- <
- <=
- >
- >=

The relational operators have lower precedence than all arithmetic operators.This means,
for example, that the following expression
  a < b + c
is evaluated as
  a < (b + c)

In summary, execution of the for statement proceeds as follows:
1. The initial expression is evaluated first.This expression usually sets a variable that
  will be used inside the loop, generally referred to as an index variable, to some ini-
  tial value such as 0 or 1.
2. The looping condition is evaluated. If the condition is not satisfied (the expression
  is FALSE), the loop is immediately terminated. Execution continues with the pro-
  gram statement that immediately follows the loop.
3. The program statement that constitutes the body of the loop is executed.
4. The looping expression is evaluated.This expression is generally used to change
  the value of the index variable, frequently by adding 1 to it or subtracting 1
  from it.
5. Return to step 2.

Remember that the looping condition is evaluated immediately on entry into the loop,
before the body of the loop has even executed one time. Also, remember not to put a
semicolon after the close parenthesis at the end of the loop (this immediately ends the
loop).


Note
  On a 32-bit system, `size_t` is a 32-bit value, but on a 64-bit system, it 
  must be a 64-bit value, to take advantage of the larger addressing model. 
  This means a 64-bit system will probably contain a typedef of `size_t` to 
  be an unsigned long. 
end note


In general, any place
in a C program that a single statement is permitted, a block of statements can be used,
provided that you remember to enclose the block within a pair of braces.


The characters %2i tell the printf routine that not only do you want to dis-
play the value of an integer at that particular point, but you also want 
the size of the integer to be displayed to take up two columns in the 
display. Any integer that would normally take up less than two columns 
(that is, the integers 0 through 9) are displayed with a leading space.This 
is known as right justification.


`%2i` is a "field width specification"

If the value that is to be displayed requires more columns than are specified by the
field width, printf simply ignores the field width specification and uses as many
columns as are necessary to display the value



scanf():
The scanf routine is very similar in concept to the
printf routine.Whereas the printf routine is used to display values at the terminal, the
scanf routine enables you to type values into the program.


The second argument to the scanf routine specifies where the value that is 
typed in by the user is to be stored.The & character before the variable 
number is necessary in this case.

Chapter 11, “Pointers,” discusses the "&"
character, which is actually an operator, in great detail. Always remember to put the lead-
ing & in front of the variable name in the scanf function call. If you forget, it causes
unpredictable results and might cause your program to terminate abnormally.

Note to self:
& is a memory writer???
end note


## for Loop Variants
Multiple Expressions
You can include multiple expressions in any of the fields of the for loop, provided that
you separate such expressions by commas. For example, in the for statement that begins

    for ( i = 0, j = 0; i < 10; ++i )
        ...

the value of i is set to 0 and the value of j is set to 0 before the loop begins.The two
expressions i = 0 and j = 0 are separated from each other by a comma, and both
expressions are considered part of the `init_expression` field of the loop. As another
example, the for loop that starts

    for ( i = 0, j = 100; i < 10; ++i, j = j - 10 )
        ...

sets up two index variables, i and j; the former initialized to 0 and the latter to 100
before the loop begins. Each time after the body of the loop is executed, the value of i
is incremented by 1, whereas the value of j is decremented by 10.


Omitting Fields
Just as the need might arise to include more than one expression in a particular field of
the for statement, the need might arise to omit one or more fields from the statement.
This can be done simply by omitting the desired field and marking its place with a semi-
colon.The most common application for the omission of a field in the for statement
occurs when there is no initial expression that needs to be evaluated.The
init expression field can simply be “left blank” in such a case, as long as the semi-
colon is still included:

    for ( ; j != 100; ++j )
        ...

This statement might be used if j were already set to some initial value before the loop
was entered.

A for loop that has its looping condition field omitted effectively sets up an infi-
nite loop; that is, a loop that is theoretically executed forever. Such a loop can be used
provided there is some other means used to exit from the loop (such as executing a
return, break, or goto statement as discussed elsewhere in this book).


Declaring Variables
You can also declare variables as part of your initial expression inside a for loop.This is
done using the normal ways you’ve defined variables in the past. For example, the fol-
lowing can be used to set up a for loop with an integer variable counter both defined
and initialized to the value 1:

    for ( int counter = 1; counter <= 5; ++counter )

The variable counter is only known throughout the execution of the for loop (it’s
called a local variable) and cannot be accessed outside the loop. As another example, the
following for loop

    for ( int n = 1, triangularNumber = 0; n <= 200; ++n )
        triangularNumber += n;

defines two integer variables and sets their values accordingly.


## The while Statement
The *while* statement further extends the C languages's repertoire of
looping capabilities. The syntax of this frequently used construct is as
follows:

    while ( expression )
        program statement

The expression specified inside the parentheses is evaluated. If the
result of the expression evaluation is TRUE, the program statement that
immediately follows is executed. After execution of this statement (or
statements if enclosed in braces), the expression is once again
evaluated. If the result of the evaluation is TRUE, the program
statement is once again executed. This process continues until the
expression finally evaluates as FALSE, at which point the loop is
terminated. Execution of the program then continues with the statement
that follows the program statement.


You might have realized from this program that you could have readily accomplished
the same task by using a for statement. In fact, a for statement can always be translated
into an equivalent while statement, and vice versa. For example, the general for
statement

    for ( init_expression; loop_condition; loop_expression )
        program statement

can be equivalently expressed in the form of a while statement as

        init_expression;
        while ( loop_condition ) {
            program statement
            loop_expression;
        }

After you become familiar with the use of the while statement, you will gain a better
feel as to when it seems more logical to use a while statement and when to use a for
statement.
In general, a loop executed a predetermined number of times is a prime candidate for
implementation as a for statement. Also, if the initial expression, looping expression, and
looping condition all involve the same variable, the for statement is probably the right
choice.


## The do Statement
The two looping statements discussed so far in this chapter both make a test of the con-
ditions before the loop is executed.Therefore, the body of the loop might never be exe-
cuted at all if the conditions are not satisfied.When developing programs, it sometimes
becomes desirable to have the test made at the end of the loop rather than at the begin-
ning. Naturally, the C language provides a special language construct to handle such a
situation.This looping statement is known as the do statement.The syntax of this state-
ment is as follows:

    do 
        program statement
    while ( loop_expression)

Execution of the do statement proceeds as follows: the program statement
is executed first. Next, the `loop_expression` inside the parentheses is
evaluated. If the result of evaluating the loop expression is TRUE, the
loop continues and the program statement is once again executed.
As long as evaluation of the `loop_expression` continues to be
TRUE, the program statement is repeatedly executed.When evaluation of the expres-
sion proves FALSE, the loop is terminated, and the next statement in the program is exe-
cuted in the normal sequential manner.
The do statement is simply a transposition of the while statement, with the looping
conditions placed at the end of the loop rather than at the beginning.
Remember that, unlike the for and while loops, the do statement guarantees that the
body of the loop is executed at least once.

## The break Statement
Sometimes when executing a loop, it becomes desirable to leave the loop
as soon as a certain condition occurs (for instance, you detect an error
condition, or you reach the end of your data prematurely). 
The break statement can be used for this purpose. Execution of the break
statement causes the program to immediately exit from the loop it is
executing, whether it is a for, while or do loop. Subsequently
statements in the loop are skipped, and execution of the loop is
terminated. Execution continues with whatever statement follows the
loop.

If a break is executed from within a set of nested loops, only the
innermost loop in which the break is executed is terminated.

The format of the break statement is simply the keyword "break" followed
by a semi colon"

    break;

## The Continue Statement
The continue statement is similar to the break statement except it doesn’t cause the
loop to terminate. Rather, as its name implies, this statement causes the loop in which it
is executed to be continued. At the point that the continue statement is executed, any
statements in the loop that appear after the continue statement are automatically
skipped. Execution of the loop otherwise continues as normal.
The continue statement is most often used to bypass a group of statements inside a
loop based upon some condition, but to otherwise continue execution of the loop.The
format of the continue statement is simply
continue;



## Chapter 6: Making Decisions
The C programming language also provides several other decision-making constructs,
which are covered in this chapter:
  - The if statement
  - The switch statement
  - The conditional operator


## The if Statement
The C programming language provides a general decision-making capability in the form
of a language construct known as the if statement. The general format of this statement
is as follows:

    if ( expression )
        program statement

Imagine that you could translate a statement such as “If it is not raining, then I will go
swimming” into the C language. Using the preceding format for the if statement, this
might be “written” in C as follows:

    if ( it is not raining )
        I will go swimming


Note to self:
  Only use braces when you have multiple statements in the body of a
  loop or conditional
end note


## The if-else Construct
The general construct is as follows:

    if (expression )
        program statement 1
    else
        program statement 2

## Compound Relational Tests
A compound relational test is simply one or more simple relational tests
joined by either the logical AND or the logical OR operator. These
operators are represented by the character pairs && and || (two vertical
bar characters), respectively. As an example, the C statement

    if ( grade >= 70 && grade <= 79 )
        ++grades_70_to_79;

increments the value of grades_70_to_79 only if the value of grade is
greater than or equal to 70 and less than or equal to 79. In a like
manner, the statement

    if ( index < 0 || index > 99 )
        printf ("Error - index out of range\n");

causes the execution of the printf statement if index is less than 0 or
grater than 99.


## Nested if Statements

    if ( gameIsOver == 0 )
        if ( playerToMove == YOU )
            printf ("Your Move\n");

If the value of gameIsOver is 0, the following statement is executed, 
which is another if statement.This if statement compares the value of 
playerToMove against YOU. If the two
values are equal, the message “Your Move” is displayed at the terminal.Therefore, the
printf statement is executed only if gameIsOver equals 0 and playerToMove equals YOU.
In fact, this statement could have been equivalently formulated using compound rela-
tionals as follows:

    if ( gameIsOver == 0 && playerToMove == YOU )
        printf ("Your Move\n");

## The else if Construct
You’ve seen how the else statement comes into play when you have a test against two
possible conditions—either the number is even, else it is odd; either the year is a leap
year, else it is not. However, programming decisions that you have to make are not
always so black-and-white. Consider the task of writing a program that displays –1 if a
number typed in by a user is less than zero, 0 if the number typed in is equal to zero, and
1 if the number is greater than zero. (This is actually an implementation of what is com-
monly called the sign function.)

    if ( expression 1 )
        program statement 1
    else if (expression 2)
        program statement 2
    else
        program statement 3

Note:
  on a computer system that uses the ASCII format
  mentioned previously, the character '0' is actually represented internally as the number
  48, the character '1' as the number 49, and so on.
end note

Note:
  In general, whenever you’re reading data from the terminal, the
  program doesn’t see any of the data typed on the line until the Enter key is pressed.

  It’s better to use routines in the standard library called islower and 
  isupper and avoid the internal representation issue entirely. To do that, 
  you need to include the line #include `<ctype.h>` in your program.
end note

It must become a matter of self-discipline while cod-
ing a program to always say “What would happen if ...” and to insert the necessary pro-
gram statements to handle the situation properly.


## The switch Statement

    switch ( expression )
    {
        case value1:
            program statement
            program statement
            ...
            break;
        case value2:
            program statement
            program statement
            ...
            break;
            ...
        case valuen:
            program statement
            program statement
            ...
            break;
        default:
            program statement
            program statement
            ...
            break;
    }

The expression enclosed within parentheses is successively compared against the values
value1, value2, ..., valuen, which must be simple constants or constant expressions. If a
case is found whose value is equal to the value of expression, the program statements
that follow the case are executed. Note that when more than one such program state-
ment is included, they do not have to be enclosed within braces.
The break statement signals the end of a particular case and causes execution of the
switch statement to be terminated. Remember to include the break statement at the
end of every case. Forgetting to do so for a particular case causes program execution to
continue into the next case whenever that case gets executed.
The special optional case called default is executed if the value of expression does
not match any of the case values.This is conceptually equivalent to the “fall through”
else that you used in the previous example. In fact, the general form of the switch
statement can be equivalently expressed as an if statement as follows:

    if ( expression == value1 )
    {
      program statement
      program statement
      ...
    }
    else if ( expression == value2 )
    {
      program statement
      program statement
      ...
    }
      ...
    else if ( expression == valuen )
    {
      program statement
      program statement
      ...
    }
    else
    {
      program statement
      program statement
      ...
    }


The break statement in the default case is actually unnecessary in the preceding
program because no statements follow this case inside the switch. Nevertheless, it is a
good programming habit to remember to include the break at the end of every case.
When writing a switch statement, bear in mind that no two case values can be the
same. However, you can associate more than one case value with a particular set of
program statements.This is done simply by listing the multiple case values (with the
keyword case before the value and the colon after the value in each case) before the
common statements that are to be executed.

As an example, in the following switch
statement, the printf statement, which multiples value1 by value2, is executed if
operator is equal to an asterisk or to the lowercase letter x.

    switch (operator)
    {
      ...
      case '*':
      case 'x':
        printf ("%.2f\n", value1 * value2);
        break;
        ...
    }



## Boolean Variables
There are several approaches that you can take to generate a table of prime numbers.
If you have the task of generating all prime numbers up to 50, for example, the most
straightforward (and simplest) algorithm to generate such a table is simply to test each
integer p for divisibility by all integers from 2 through p–1. If any such integer evenly
divided p, then p is not prime; otherwise, it is a prime number.

You might have noticed that the variable isPrime takes on either the value 0 or 1,
and no other values.That’s why you declared it to be a `_Bool` variable. Its value is 1 as
long as p still qualifies as a prime number. But as soon as a single even divisor is found,
its value is set to 0 to indicate that p no longer satisfies the criteria for being prime.
Often, variables that are used in such a manner are referred to as flags. A flag typically
assumes only one of two different values. Furthermore, the value of a flag is usually test-
ed at least once in the program to see if it is “on” (TRUE) or “off ” (FALSE), and some
particular action is taken based upon the results of the test.
In C, the notion of a flag being TRUE or FALSE is most naturally translated into the
values 1 and 0, respectively. So in the Program 6.10, when you set the value of isPrime
to 1 inside the loop, you are effectively setting it as TRUE to indicate that p “is prime.”
If during the course of execution of the inner for loop an even divisor is found, the
value of isPrime is set to FALSE to indicate that p no longer “is prime.”

It is no coincidence that the value 1 is typically used to represent the TRUE or “on”
state and 0 to represent the FALSE or “off ” state.This representation corresponds to the
notion of a single bit inside a computer.When the bit is “on,” its value is 1; when it is

“off,” its value is 0. But in C, there is an even more convincing argument in favor of
these logic values. It has to do with the way the C language treats the concept of TRUE
and FALSE.
Recall from the beginning of this chapter that if the conditions specified inside the
if statement are “satisfied,” the program statement that immediately follows executes.
But what exactly does “satisfied” mean? In the C language, satisfied means nonzero, and
nothing more. So the statement
if ( 100 )
printf ("This will always be printed.\n");
results in execution of the printf statement because the condition in the if statement
(in this case, simply the value 100) is nonzero and, therefore, is satisfied.
In each of the programs in this chapter, the notions of “nonzero means satisfied” and
“zero means not satisfied” are used.This is because whenever a relational expression is
evaluated in C, it is given the value 1 if the expression is satisfied and 0 if the expression
is not satisfied. So evaluation of the statement

    if ( number < 0 ) 
      number = -number;

actually proceeds as follows:
1. The relational expression number < 0 is evaluated. If the condition is satisfied, that
  is, if number is less than zero, the value of the expression is 1; otherwise, its value
  is 0.
2. The if statement tests the result of the expression evaluation. If the result is
  nonzero, the statement that immediately follows is executed; otherwise, the state-
  ment is skipped.

The preceding discussion also applies to evaluation of conditions inside the for,
statements. Evaluation of compound relational expressions such as in the
statement
while, and do

while ( char != 'e' && count != 80 )

also proceeds as outlined previously. If both specified conditions are valid, the result is 1;
but if either condition is not valid, the result of the evaluation is 0.The results of the
evaluation are then checked. If the result is 0, the while loop terminates; otherwise it
continues.

Returning to Program 6.10 and the notion of flags, it is perfectly valid in C to test if
the value of a flag is TRUE by an expression such as

  if ( isPrime )

rather than with the equivalent expression

  if ( isPrime != 0 )

To easily test if the value of a flag is FALSE, you can use the logical negation operator,
!. In the expression

  if ( ! isPrime )

the logical negation operator is used to test if the value of isPrime is FALSE (read this
statement as “if not isPrime”). In general, an expression such as
! expression
negates the logical value of expression. So if expression is zero, the logical negation
operator produces a 1. And if the result of the evaluation of expression is nonzero, the
negation operator yields a 0.
The logical negation operator can be used to easily “flip” the value of a flag, such as
in the expression

  myMove = ! myMove;

As you might expect, this operator has the same precedence as the unary minus opera-
tor, which means that it has higher precedence than all binary arithmetic operators and
all relational operators. So to test if the value of a variable x is not less than the value of a
variable y, such as in

    ! ( x < y )

the parentheses are required to ensure proper evaluation of the expression. Of course,
you could have equivalently expressed the previous expression as

    x >= y

In Chapter 4, “Variables, Data Types, and Arithmetic Expressions,” you learned about
some special values that are defined in the language which you can use when working
with Boolean values.These are the type bool, and the values true and false.To use
these, you need to include the header file `<stdbool.h>` inside your program.

As you can see, by including `<stdbool.h>` in your program, you can declare variables to
be of type bool instead of `_Bool`.This is strictly for cosmetic purposes because the for-
mer is easier to read and type than the latter, and it fits in more with the style of the
other basic C data types, such as int, float, and char.


## The Conditional Operator
Perhaps the most unusual operator in the C language is one called the conditional opera-
tor. Unlike all other operators in C—which are either unary or binary operators—the
conditional operator is a ternary operator; that is, it takes three operands.The two sym-
bols that are used to denote this operator are the question mark (?) and the colon (:).
The first operand is placed before the ?, the second between the ? and the :, and the
third after the :.
The general format of the conditional operator is

  condition ? expression1 : expression2

where condition is an expression, usually a relational expression, that is evaluated first
whenever the conditional operator is encountered. If the result of the evaluation of con-
dition is TRUE (that is, nonzero), then expression1 is evaluated and the result of the
evaluation becomes the result of the operation. If condition evaluates FALSE (that is,
zero), then expression2 is evaluated and its result becomes the result of the operation.
The conditional operator is most often used to assign one of two values to a variable
depending upon some condition. For example, suppose you have an integer variable x
and another integer variable s. If you want to assign –1 to s if x were less than zero, and
the value of x2 to s otherwise, the following statement could be written:

    s = ( x < 0 ) ? -1 : x * x;

The condition x < 0 is first tested when the preceding statement is executed.
Parentheses are generally placed around the condition expression to aid in the statement’s
readability.This is usually not required because the precedence of the conditional opera-
tor is very low—lower, in fact, than all other operators besides the assignment operators
and the comma operator.
If the value of x is less than zero, the expression immediately following the ? is evalu-
ated.This expression is simply the constant integer value –1, which is assigned to the
variable s if x is less than zero.
If the value of x is not less than zero, the expression immediately following the : is
evaluated and assigned to s. So if x is greater than or equal to
zero, the value of `x * x`, or `x2`, is assigned to s.
As another example of the use of the conditional operator, the following statement
assigns to the variable maxValue the maximum of a and b:

    maxValue = ( a > b ) ? a : b;

If the expression that is used after the : (the “else” part) consists of another conditional
operator, you can achieve the effects of an “else if ” clause. For example, the sign function
that was implemented in Program 6.6 can be written in one program line using two
conditional operators as follows:

    sign = ( number < 0 ) ? -1 : (( number == 0 ) ? 0 : 1);

If number is less than zero, sign is assigned the value –1; else if number is equal to zero,
sign is assigned the value 0; else it is assigned the value 1.The parentheses around the
“else” part of the preceding expression are actually unnecessary.This is because the con-
ditional operator associates from right to left, meaning that multiple uses of this operator
in a single expression, such as in

  e1 ? e2 : e3 ? e4 : e5

group from right to left and, therefore, are evaluated as

  e1 ? e2 : ( e3 ? e4 : e5 )

It is not necessary that the conditional operator be used on the right-hand side of an
assignment—it can be used in any situation in which an expression could be used.This
means that you could display the sign of the variable number, without first assigning it to
a variable, using a printf statement as shown:

    printf ("Sign = %i\n", ( number < 0 ) ? –1 : ( number == 0 ) ? 0 : 1);

The conditional operator is very handy when writing preprocessor macros in C.This is
seen in detail in Chapter 13, “The Preprocessor.”



## Working with Arrays

    [type] [array_label][arr_size]

    int grades[10]
    float temperature[100]

Because you
never assigned values to five of the elements in the array—elements 1, 4, and 6 through
8—the values that are displayed for them are meaningless. Even though the program’s
output shows these values as zero, the value of any uninitialized variable or array element
is not defined. For this reason, no assumption should ever be made as to the value of an
uninitialized variable or array element.

## Generating Fibonacci Numbers
The first two Fibonacci numbers, called F0 and F1, are defined to be 0 and 1, respective-
ly.Thereafter, each successive Fibonacci number Fi is defined to be the sum of the two
preceding Fibonacci numbers Fi–2 and Fi–1. So F2 is calculated by adding together the
values of F0 and F1. In the preceding program, this corresponds directly to calculating
`Fibonacci[2]` by adding the values `Fibonacci[0]` and `Fibonacci[1]`


The sequence of Fibonacci numbers historically origi-
nated from the “rabbits problem”: If you start with a pair of rabbits and assume that each
pair of rabbits produces a new pair of rabbits each month, that each newly born pair of
rabbits can produce offspring by the end of their second month, and that rabbits never
die, how many pairs of rabbits will you have after the end of one year? The answer to
this problem rests in the fact that at the end of the nth month, there will be a total of
Fn+2 rabbits.Therefore, according to the table from Program 7.3, at the end of the
twelfth month, you will have a total of 377 pairs of rabbits.



Note:
  any nonprime integer can be expressed as a multiple of prime factors. (For
  example, 20 has the prime factors 2, 2, and 5.) 
end note

As a further optimization of the prime number generator program, it can be readily
demonstrated that any nonprime integer n must have as one of its factors an integer that
is less than or equal to the square root of n.This means that it is only necessary to deter-
mine if a given integer is prime by testing it for even divisibility against all prime factors
up to the square root of the integer.


## Initializing Arrays
The statement

    int counters[5] = { 0, 0, 0, 0, 0 };

declares an array called counters to contain five integer values and initializes each of
these elements to zero. In a similar fashion, the statement

    int integers[5] = { 0, 1, 2, 3, 4 };

sets the value of `integers[0]` to 0, `integers[1]` to 1, `integers[2]` to 2, and so on.

Arrays of characters are initialized in a similar manner; thus the statement

    char letters[5] = { 'a', 'b', 'c', 'd', 'e' };

defines the character array letters and initializes the five elements to the characters
'a', 'b', 'c', 'd', and 'e', respectively.
It is not necessary to completely initialize an entire array. If fewer initial values are
specified, only an equal number of elements are initialized.The remaining values in the
array are set to zero. So the declaration

    float sample_data[500] = { 100.0, 300.0, 500.5 };

initializes the first three values of `sample_data` to 100.0, 300.0, and 500.5, and sets the
remaining 497 elements to zero.
By enclosing an element number in a pair of brackets, specific array elements can be
initialized in any order. For example,

    float sample_data[500] = { [2] = 500.5, [1] = 300.0, [0] = 100.0 };

initializes the `sample_data` array to the same values as shown in the previous example.
And the statements

    int x = 1233;
    int a[10] = { [9] = x + 1, [2] = 3, [1] = 2, [0] = 1 };

define a 10-element array and initialize the last element to the value of x + 1 (or to
1234), and the first three elements to 1, 2, and 3, respectively.
Unfortunately, C does not provide any shortcut mechanisms for initializing array ele-
ments.That is, there is no way to specify a repeat count, so if it were desired to initially
set all 500 values of `sample_data` to 1, all 500 would have to be explicitly spelled out. In
such a case, it is better to initialize the array inside the program using an appropriate for
loop.


The most notable point in the preceding program is the declaration of the character
array word.There is no mention of the number of elements in the array.The C language
allows you to define an array without specifying the number of elements. If this is done,
the size of the array is determined automatically based on the number of initialization
elements. Because Program 7.6 has six initial values listed for the array word, the C lan-
guage implicitly dimensions the array to six elements.
This approach works fine so long as you initialize every element in the array at the
point that the array is defined. If this is not to be the case, you must explicitly dimension
the array.
In the case of using index numbers in the initialization list, as in

    float sample_data[] = { [0] = 1.0, [49] = 100.0, [99] = 200.0 };

the largest index number specified sets the size of the array. In
this case, `sample_data` is set to contain 100 elements, based on
the largest index value of 99 that is specified.


## Base Conversion Using Arrays

The next program further illustrates the use of integer and character arrays.The task is to
develop a program that converts a positive integer from its base 10 representation into its
equivalent representation in another base up to base 16. As inputs to the program, you
specify the number to be converted and also the base to which you want the number
converted.The program then converts the keyed-in number to the appropriate base and
displays the result.
The first step in developing such a program is to devise an algorithm to convert a
number from base 10 to another base. An algorithm to generate the digits of the con-
verted number can be informally stated as follows: A digit of the converted number is
obtained by taking the modulo of the number by the base.The number is then divided
by the base, with any fractional remainder discarded, and the process is repeated until the
number reaches zero.
The outlined procedure generates the digits of the converted number starting from
the rightmost digit. See how this works in the following example. Suppose you want to
convert the number 10 into base 2.Table 7.1 shows the steps that would be followed to
arrive at the result.

###Table 7.1 Converting an Integer from Base 10 to Base 2

<table>
    <thead>
        <tr>
            <th>Number</th> <th>Number Modulo 2</th> <th>Number / 2</th>
        </tr>
    </thead>
    <tbody>
        <tr> <td>10</td> <td>0</td> <td>5</td></tr>
        <tr> <td>5</td> <td>1</td> <td>2</td></tr>
        <tr> <td>2</td> <td>0</td> <td>1</td> </tr>
        <tr> <td>1</td> <td>1</td> <td>0</td></tr>
    </tbody>
</table>

The result of converting 10 to base 2 is, therefore, seen to be 1010, reading the digits of
the “Number Modulo 2” column from the bottom to the top.

This program also
introduces the type qualifier const, which is used for variables whose value does not
change in a program.


## Multidimensional Arrays

Two-dimensional arrays can be initialized in a manner analogous to their one-
dimensional counterparts.When listing elements for initialization, the values are listed
by row. Brace pairs are used to separate the list of initializers for one row from the next.
So to define and initialize the array M to the elements listed in Table 7.3, a statement
such as the following can be used:

    int M[4][5] = {
                    { 10, 5, -3, 17, 82 },
                    { 9, 0, 0, 8, -7 },
                    { 32, 20, 1, 0, 14 },
                    { 0, 0, 8, 7, 6 }
                };



## Working with Functions
sample function

    void printMessage (void){
        printf ("Programming is fun.\n");
    }

The first line of the function tells the compiler (from left to right)
four things about the function:
 - Who can call it
 - The type of value it returns
 - Its name
 - The arguments it takes.
 
Recall from discussions of program 3.1 that main is a specially
recognized name in the C system that always indicates where the program
is to begin execution. You must always have a main. You can add a main
function to the preceding code to end up with a complete program, as
shown below:

    #include <stdio.h>
    void printMessage(void)
    {
        printf ("Programming is fun.\n");
    }

    int main(void)
    {
        printMessage();
  
        return 0;
    }

Note that it is acceptable to insert a return statement at the
end of printMessage like this:

    return;

Because printMessage does not return a value, no value is specified for the return.This
statement is optional because reaching the end of a function without executing a return
has the effect of exiting the function anyway without returning a value. In other words,
either with or without the return statement, the behavior on exit from printMessage
is identical.


## Function Prototype Declaration
The function calculateTriangleNumber requires a bit of explanation. The
first line of the function:

    void calculateTriangularNumber (int n)

is called the function prototype declaration. It tells the compiler that
calculateTriangularNumber is a function that returns no value (the
keyword void) and that takes a single argument, called n, which is an
int. The name that is chosen for an argument, called its formal
parameter name, as well as the name of the function itself, can be any
valid name.

After the formal parameter name has been defined, it can be used to
refer to the argument anywhere inside the body of the function.

The beginning of the function's definition is indicated by the opening
curly brace.

## Automatic Local Variables
Variables defined inside a function are known as automatic local
variables because they are automatically created each time a function is
called, and because their values are local to the function. The value of
a local variable can only be accessed by the function in which the
variable is defined. Its value cannot be accessed by any other function. If an
initial value is given to a variable inside a function, that initial value is assigned to the
variable each time the function is called.
When defining a local variable inside a function, it is more precise in C to use the
keyword auto before the definition of the variable. An example of this is as follows:

    auto int i, triangularNumber = 0;

Because the C compiler assumes by default that any variable defined inside a function is
an automatic local variable, the keyword auto is seldom used, and for this reason it is not
used in this book.


## Returning Function Results
The functions in Program 8.4 and 8.5 perform some straightforward
calculations and then display the results of the calculations at the
terminal. However, you might not always want the results of your
calculations displayed. The C language provides you with a convenient
mechanism whereby the results of a function can be returned to the
calliong routine. The general syntax of this construct is
straightforward enough:

    return expression;

This statement indicates that the function is to return the value of
expression to the calling routine.

An appropriate return statement is not enough. When the function
declaration is made, you must also declare the type of value the
function returns. This declaration is placed immediately before the
function.

A C function can only return a single value in the manner just
described. Unlike some other languages, C makes no distinction between
subroutines (procedures) and functions. In C, there's only the function,
which can optionally return a value. If the declaration of the type
returned by a function is omitted, the C compiler assumes that the
function returns an int - if it returns a value at all.

As noted earlier, a function declaration that is preceded by a keyword
void expplicitly informs the compiler that the function does not return
a value. A subsequent attempt at using the function in an expression, as
if a value were returned, results in a compiler error message.

For example, because the calculateTriangularNumber function of
Program 8.4 did not return a value, you placed the keyword void before its name when
defining the function. Subsequently attempting to use this function as if it returned a
value, as in

    number = calculateTriangularNumber (20);

results in a compiler error.
In a sense, the void data type is actually defining the absence of a data type.Therefore,
a function declared to be of type void has no value and cannot be used as if it does have
a value in an expression.


## Newtom-Raphson Iteration Technique
For finding the square root of a number.

Algorithm:
Newton-Raphson Method to Compute the Square Root of x
Step 1: Set the guess to 1.
Step 2: If |guess^2 - x| < ε, proceed to step 4.
Step 3: Set the value to (x / guess + guess) / 2 and return to step 2.
Step 4: The guess is the approximation of the square root.

It is necessary to test the absolute difference of guess2 and x against ε in step 2 because
the value of guess can approach the square root of x from either side.


The scope of a local variable is the function in which it is defined.

## Declaring Return Types and Argument Types
As mentioned previously, the C compiler assumes that a function returns
a value of type int as the default case. More specifically, when a call
is made to a function, the compiler assumes that the function returns a
value of type int unless either of the following has occured:
  1. The function has been defined in the program before the function
     call is encountered.
  2. The value returned by the function has been declared before the
     function call is encountered.

In Program 8.8, the absoluteValue function is defined before the compiler encounters
a call to this function from within the squareRoot function.The compiler knows, there-
fore, that when this call is encountered, the absoluteValue function will return a value
of type float. Had the absoluteValue function been defined after the squareRoot
function, then upon encountering the call to the absoluteValue function, the compiler
would have assumed that this function returned an integer value. Most C compilers
catch this error and generate an appropriate diagnostic message.
To be able to define the absoluteValue function after the squareRoot function (or
even in another file—see Chapter 15), you must declare the type of result returned by the
absoluteValue function before the function is called.The declaration can be made inside
the squareRoot function itself, or outside of any function. In the latter case, the declara-
tion is usually made at the beginning of the program.
Not only is the function declaration used to declare the function’s return type, but it
is also used to tell the compiler how many arguments the function takes and what their
types are.


To declare absoluteValue as a function that returns a value of type float and that
takes a single argument, also of type float, the following declaration is used:
float
absoluteValue (float);
As you can see, you just have to specify the argument type inside the parentheses, and
not its name.You can optionally specify a “dummy” name after the type if you want:
float
absoluteValue (float
x);
This name doesn’t have to be the same as the one used in the function definition—the
compiler ignores it anyway.
A foolproof way to write a function declaration is to simply use your text editor to
make a copy of the first line from the actual definition of the function. Remember to
place a semicolon at the end.


If the function doesn’t take an argument, use the keyword void between the paren-
theses. If the function doesn’t return a value, this fact can also be declared to thwart any
attempts at using the function as if it does:

    void calculateTriangularNumber (int n);

If the function takes a variable number of arguments (such as is the case with printf
and scanf), the compiler must be informed.The declaration

    int printf (char *format, ...);

tells the compiler that printf takes a character pointer as its first argument (more on that
later), and is followed by any number of additional arguments (the use of the ...). printf
and scanf are declared in the special file stdio.h.This is why you have been placing
the following line at the start of each of your programs:

    #include <stdio.h>

Without this line, the compiler can assume `printf` and `scanf` take a fixed number of
arguments, which could result in incorrect code being generated.


The compiler automatically converts your arguments to the appropriate types when a
function is called, but only if you have placed the function’s definition or have declared
the function and its argument types before the call.
Here are some reminders and suggestions about functions:
  1. Remember that, by default, the compiler assumes that a function  returns an int.

  2. When defining a function that returns an int, define it as such.

  3. When defining a function that doesn’t return a value, define it as void.

  4. The compiler converts your arguments to agree with the ones the function
expects only if you have previously defined or declared the function.

  5. To play it safe, declare all functions in your program, even if they are defined
before they are called. (You might decide later to move them somewhere else in
your file or even to another file.)


Checking Function Arguments

Note: 
1.The square root routine in the standard C library is called sqrt and it returns a domain error if a
end note
negative argument is supplied.The actual value that is returned is implementation-defined. On
some systems, if you try to display such a value, it displays as nan, which means not a number.


As you can see from the modified squareRoot function (and as you also saw in the
last example from Chapter 7, “Working with Arrays”), you can have more than one
return statement in a function.Whenever a return is executed, control is immediately
sent back to the calling function; any program statements in the function that appear
after the return are not executed.This fact also makes the return statement ideal for
use by a function that does not return a value. In such a case, as noted earlier in this
chapter, the return statement takes the simpler form
return;
because no value is to be returned. Obviously, if the function is supposed to return a
value, this form cannot be used to return from the function.


## Top-Down Programming
The notion of functions that call functions that in turn call functions,
and so on, forms the basis for producing good, structured programs. In
the main routine of Program 8.8, the `square_root` function is called
several times. All the details concerned with the actual calculation of
the square root are contained within the square root function itself,
and not within main. Thus you can write a call to this function before
you even write the instructions of the function itself, as long as you
specify the arguments that the function takes and the value that it
returns.

Later, when proceeding to write the code for the `square_root` function,
this same type of top-down programming technique can be applied: You can
write a call to the `absolute_value` function without concerning yourself
at that time with the details of the operation of that function. All you
need to know is that you can develop a function to take that absolute
value of a number.

The same programming technique that makes programs easier to write also
makes them easier to read. Thus the reader of Program 8.8 can easily
determine upon examination of the main routine that the program is
simply calculating and displaying the square root of three numbers. She
need not sift through all of the details of how the square root is
actually calculated to glean this information. If she wants to get more
involved in the details, she can study the specific code associated with
the square root function. Inside that function, the same discussion
applies to the `absolute_value` function. She does not need to know how
the absolute value of a number is calculated to understand the operation
of the `square_root` function. Such details are relegated to the
`absolute_value` function itself, which can be studied if a more detailed
knowledge of its operation is desired.


## Functions and Arrays
It is not necessary to declare the function prototype inside the main if
the function was defined before the main was invoked. Its only necessary
if the function is defined afterwards.

Note:
  Array arguments passed by reference, not copied.
  Getting back to the main point to be made about the preceding program, you might
have realized by now that the function multiplyBy2 actually changes values inside the
floatVals array. Isn’t this a contradiction to what you learned before about a function
not being able to change the value of its arguments? Not really.
This program example points out one major distinction that must always be kept in
mind when dealing with array arguments: If a function changes the value of an array
element, that change is made to the original array that was passed to the function.This
change remains in effect even after the function has completed execution and has
returned to the calling routine.
The reason an array behaves differently from a simple variable or an array element—
whose value cannot be changed by a function—is worthy of explanation. As mentioned
previously, when a function is called, the values that are passed as arguments to the func-
tion are copied into the corresponding formal parameters.This statement is still valid.
However, when dealing with arrays, the entire contents of the array are not copied into
the formal parameter array. Instead, the function gets passed information describing where
in the computer’s memory the array is located. Any changes made to the formal parame-
ter array by the function are actually made to the original array passed to the function,
and not to a copy of the array.Therefore, when the function returns, these changes still
remain in effect.
Remember, the discussion about changing array values in a function applies only to
entire arrays that are passed as arguments, and not to individual elements, whose values
are copied into the corresponding formal parameters and, therefore, cannot be perma-
nently changed by the function. Chapter 11 discusses this concept in greater detail.

end note

Simple Exchange sort Algorithm
Step 1: Set i to 0.
Step 2: Set j to i + 1.
Step 3: If `a[i]` > `a[j]`, exchange their values.
Step 4: Set j to j + 1. If j < n, go to step 3.
Step 5: Set i to i + 1. If i < n - 1, go to step 2.
Step 6: a is now sorted in ascending order.


Book: The Art of Computer Programming,Volume 3, Sorting and Searching (Donald E.
Knuth, Addison-Wesley)

Note:
  There is also a function called qsort in the standard C library that can be used to sort an array
  containing any data type. However, before you use it, you need to understand pointers to func-
  tions, which are discussed in Chapter 11.
end note


Multidimensional Arrays
The discussion of this topic for single-
dimensional arrays also applies here: An assignment made to any element of the formal
parameter array inside the function makes a permanent change to the array that was
passed to the function.

When declaring a single-dimensional array as a formal parameter inside a function,
you learned that the actual dimension of the array is not needed; simply use a pair of
empty brackets to inform the C compiler that the parameter is, in fact, an array.This
does not totally apply in the case of multidimensional arrays. For a two-dimensional
array, the number of rows in the array can be omitted, but the declaration must contain
the number of columns in the array. So the declarations

    int array_values[100][50]

and

    int array_values[][50]

are both valid declarations for a formal parameter array called
`array_values` containing 100 rows by 50 columns; but the declarations

    int array_values[100][]

and

    int array_values[][]

are not because the number of columns in the array must be specified.

Multidimensional Variable-Length Arrays and Functions
The function declaration for scalarMultiply looks like this:

    void scalarMultiply (int nRows, int nCols, int matrix[nRows][nCols], int scalar)

The rows and columns in the matrix, nRows, and nCols, must be listed as arguments
before the matrix itself so that the compiler knows about these parameters before it
encounters the declaration of matrix in the argument list. If you try it this way instead:

    void scalarMultiply (int matrix[nRows][nCols], int nRows, int nCols, int scalar)

you get an error from the compiler because it doesn’t know about nRows and nCols
when it sees them listed in the declaration of matrix.


## Global Variables
Global variables have default initial values: zero. So, in the global
declaration

    int gData[100];

all 100 elements of the gData array are set to zero when the program
begins execution.

So it's important to remember that while global variables have default
initial values of zero, local variables have no default initial value
and so must be explicitly initialized by the program.


## Automatic and Static Variables
When you normally declare a local variable inside a function, as in the
declaration of the variables `guess` and `epsilon` in your `square_root`
function

    float square_root (float x){
        const float epsilon = .00001;
        float guess         = 1.0;
        .........
    }

you're declaring automatic local variables. Recall that the keyword
`auto` can, in fact, precede the declaration of such variables, but is
optional because it is the default case. An automatic variable is, in a
sense, actually created each time the function is called. In the
preceding example, the local variables epsilon and guess are created
whenever the `square_root` function is called. As soon as the `sqaure_root`
function is finished, these local variables disappear. This process
happens automatically, hence the name `Automatic Variables`.

Automatic local variables can be given initial values, as is done with
the values of epsilon and guess. In fact, any valid C expression can be
specified as the initial value for the simple automatic variable. The
value of the expression is calculated and assigned to the automatic
local variable each time the function is called. And because an
automatic variable dissappears after the function completes execution,
the value of that variable dissappears along with it. In other words,
the value an automatic variable has when a function finishes execution
is guaranteed not to exist the next time the function is called.

If you place the word `static` infront of a local variable declaration,
you are in an entirely new ball game. The word `static` in C refers not
to an electric charge, but rather to the notion of something that has no
movement. This is the key to the concept of a static variable - it does
not come and go as the function is called and returns. This implies that
the value a static variable has upon leaving a function is the same
value that the variable will have the next time the function is called.

Static variables also differ with respect to their initialization. A
static local variable is initialized only once at the start of the
overall program execution - and not each time that the function is
called. Furthermore, the initial value specified for a static variable
must be a simple constant or constant expression. Static variables also
have default initial values of zero, unlike automatic variables, which
have no default initial value.


Note to self
  Difference between `const` and `static` variables:
  Const vars are accessible to the whole program. static vars are tied
to a particular function.
end


## Recursive Functions
Recursive functions are most commonly illustrated by an example that calculates the
factorial of a number. Recall that the factorial of a positive integer n, written n!, is simply
the product of the successive integers 1 through n.The factorial of 0 is a special case and
is defined equal to 1. So 5! is calculated as follows:

  `5!  = 5 * 4 * 3 * 2 * 1`
      = 120

and
  `6!  = 6 * 5 * 4 * 3 * 2 * 1`
      = 720

Comparing the calculation of 6! to the calculation of 5!, observe that the former is equal
to 6 times the latter; that is, 6! = 6 x 5!. In the general case, the factorial of any positive
integer n greater than zero is equal to n multiplied by the factorial of n - 1:
n! = n x (n - 1)!
The expression of the value of n! in terms of the value of (n-1)! is called a recursive defi-
nition because the definition of the value of a factorial is based on the value of another
factorial. In fact, you can develop a function that calculates the factorial of an integer n
according to this recursive definition. Such a function is illustrated in Program 8.16.



## Working with Structures
You can define a structure called `date` in the C language that consists
of three components that represent the month, day and year. The syntax
for such a definition is rather straightforward:
  
  struct date{
    int month;
    int day;
    int year;
  };

The `date` structure just defined contains three integer members called
month, day, and year. The definition of date in a sense defines a new
type in the language in that variables can subsequently be declared to
be of type `struct date`, as in the declaration
  
    struct date today;

You can also declare a variable `purshase_date` to be of the same type by
a separate declaration such as

    struct date today, purchase_date;

Unlike variables of type int, float, or char, a special syntax is needed
when dealing with structure variables. A member of a structure is
accessed by specifying the variable name, followed by a period, and then
the member name. For example, to set the value of the day in the
variable `today` to 25, you write
  
  today.day = 25;

Note that there are no spaces permitted betweent the variable name, the
period, and the member name. To set the `year` in `today` to 2004, the
expression

  today.year = 2004;

can be used. Finally, to test the value of month to see if it is equal
to 12, a statement such as

  if ( today.month == 12 )
    next_month = 1;
  
does the trick.


Note:
  Be certain you understand the difference between `defining` a structure
and `declaring` variables of a particular structure type.
end note.


The format characters `%.2i` are used to specify that two integer digits
are to be displayed with zero fill. 

## Initializing Structures
Initializing structures is similar to initializing arrays - the elements
are simply listed inside a pair of braces, with each element separated
by a comma.

To initialize the date structure variable today to July 2, 2005, the
statement

    struct date today = { 7, 2, 2005 };

can be used. The statement

    struct time this_time = { 3, 29, 55 };

defines the `struct time` variable `this_time` and sets its value to
3:29:55 a.m. As with other variables, if `this_time` is a local
structure variable, it is initialized each time the function is entered.
If the structure variable is made `static` (by placing the keyword
`static` infront of it), it is only initialized once at the start of
program execution. In either case the initial values listed inside the
curly braces must be constant expressions.

As with the initialization of an array, fewer values might be listed
than are contained in the structure. So the statement

    struct time time_1 = { 12, 10 };

sets `time_1.hour` to 12 and `time_1.minutes` to 10 but gives no initial
value to `time_1.seconds`. In such a case the default initial value is
undefined.

You can also specify the member names in the initialization list. In
that case, the general format is

    .member = value

This method enables you to initialize the members in any order, or to
only initialize specified members. For example,

    struct time time_1 = { .hour = 12, .minutes = 10 };

sets the `time_1` variable to the same initial values as shown in the
previous example. The statement

  struct date today = { .year = 2004 };

sets just the year member of the date structure variable today to 2004.

## Compound Literals
You can assign one or more values to a structure in a single statement
using what is known as `compound literals`. For example, assuming that
today has been previously declared as s struct date variable, the
assignment of the members of today as shown in Program 9.1 can also be
done in a single statement as follows:

  today = (struct date) { 9, 25, 2004 };

Note that this statement can appear anywhere in the program; it is not a
declaration statement. The type cast operator is used to tell the
compiler the type of expression, which in this case is struct date, and
is followed by the list of values that are to be assigned to the members
of the structure, in order. These values are listed in the same way as
if you were initializing a structure variable.

You can also specify values using the member notation like this:

  today = (struct date) { .month = 9, .day = 25, .year = 2004 };

The advantage of using this approach is that the arguments can appear
in any order. Without specifying the member names, they must be
supplied in the order in which they are defined in the structure.


## Array of Structures
You have seen how useful the structure is in enabling you to logically
group related elements together. With the `time` structure, for
instance, it is only necessary to keep track of one variable, instead of
three, for each time that is used by the program. So to handle 10
different times in a program, you only have to keep track of 10
different variables, instead of 30.

An even better method for handling the 10 different times involves the
combination of two powerful features of the C programming language:
structures and arrays. C does not limit you to storing simple data types
inside an array; it is perfectly valid to define an `array of
structures`. For example,

    struct time experiments[10];

defines an array called experiments, which consists of 10 elements of
type `struct date`. Similarly the definition

    struct date birthdays[15];

defines the array birthdays to contain 15 elements of type struct date.
Referencing a particular structure element inside the array is quite
natural. To set the second birthday inside the birthdays array to
August 8, 1986, the sequence of statements

    birthdays[1].month = 8;
    birthdays[1].day   = 8;
    birthdays[1].year  = 1986;

works just fine. To pass the entire structure contained in
experiments[4] to a function `check_time`, the array element is specified:

    check_time (experiments[4]);

As is to be expected, the `check_time` function declaration must specify
that an argument of type struct time is expected:

    void check_time (struct time t_0) {
        . 
        .
        .
    }

Initialization of arrays containing structures is similar to
initialization of multidimensional arrays. So the statement

    struct time run_time [5] =
        { {12, 0, 0}, {12, 30, 0}, {13, 15, 0}};

sets the first three times in the array `run_time` to 12:00:00, 12:30:00
and 15:15:00. The inner pairs of braces are optional, meaning the
preceding statement can be equivalently expressed as

    struct time run_time [5] = 
        {12, 0, 0, 12, 30, 0, 13, 15, 0 };

The following statement
  
    struct time run_time [5] =
        { [2] = {12, 0, 0}};

initializes just the third element of the array to the specified value,
whereas the statement

    static struct time run_time [5] = { [1].hour = 12, [1].minutes = 30 };

sets just the hours and minutes of the second element of the `run_time`
array to 12 and 30 respectively.

Note
  The concept of an array of structures is a very powerful and important
one in C.

end note


## Structures Containing Structures

    struct date_and_time {
        struct date s_date;
        struct time s_time;
    };

The first member of this structure is of type struct date and is called
`s_date`. The second member of the `date_and_time` structure is of type
struct time and is called `s_time`.

This definition of the `date_and_time` structure requires that a date
structure and a time structure have been previously defined to the
compiler.

Variables can now be defined to be of type `struct date_and_time`, as in

    struct date_and_time event;

To reference the date structure of the variable `event`, the syntax is
the same:

    event.s_date

So you could call your `date_update` function with this date as the
argument and assign the result back to the same place by a statement
such as

    event.s_date = time_update (event.s_time);

To reference a particular member inside one of these structures, a
period followed by the member name is tacked on the end:

    event s_date.month = 10;

This statement sets the month of the date structure contained within
`event` to October, and the statement

    ++event.s_time.seconds;

adds one to the seconds contained within the `time` structure.

The event variable can be initialized in th expected manner:

    struct date_and_time event = {
        { 2, 1, 2004 }, { 3, 30, 0 }
    };

This sets the date in `event` to February 1, 2004, and sets the time to
3:30:00.

Of course you can use members' names in the initialization, as in

    struct date_and_time event = 
        { { .month = 2, .day = 1, .year = 2004 },
        { .hour = 3, .minutes = 30, .seconds = 0 }
    };

Naturally, it is possible to set up an array of `date_and_time` strucures,
as is done with the following declaration:

    struct date_and_time events[100];

The array events is declared to contain 100 elements of type struct
`date_and_time`. The fourth `date_and_time` contained within the array is
referenced in the usual way as events[3], and the ith date in the array
can be sent to your `date_update` function as follows:

    events[i].s_date = date_update (events[i].s_date);

To set the first time in the array to noon, the series of statements

  events[0].stime.hour    = 12;
  events[0].stime.minutes = 0;
  events[0].stime.seconds = 0;

can be used.


## Structures Containing Arrays
It is possible to define structures that contain arrays as members. One
of the most common applications of this type is setting up an array of
characters inside a structure. For example, suppose you want to define a
structure called `month` that contains as its members the number of days
in the month as well as a three-character abbreviation for the month
name. The following definition does the job:

    struct month {
        int number_of_days;
        char name[3];
    };

This sets up the month structure that contains an integer member called
`number_of_days` and a character member called `name`. The member name is
actually an array of three characters. You can now define a variable to
be of type `struct month` in the normal fashion:

    struct month a_month;

You can set the proper fields inside `a_month` for January with the
following sequence of statements:

    a_month.number_of_days = 31;
    a_month.name[0]        = 'J';
    a_month.name[1]        = 'a';
    a_month.name[2]        = 'n';

Or you can initialize this variable to the same values with the
following statement:

    struct month a_month = { 31, { 'J', 'a', 'n' } };

To go one step further, you can set up 12 month structures inside an
array to represent each month of the year:

  struct month months[12]


## Structure Variants
You do have some flexibility in defining a structure. First, it is valid
to declare a variable to be of a particular structure at the same time
that the structure is defined. THis is done simply by including the
variable name (or names) before the terminating semicolon of the
structure definition. For example the statement

    struct date {
        int month;
        int day;
        int year;
    } today_date, purchase_date;

defines the structure `date` and also declares the variables
`today_date` and `purchase_date` to be of this type. You
can also assign initial values to the variables in the normal
fashion. Thus

    struct date {
        int month;
        int day;
        int year;
    } today_date = { 1, 11, 2005 };

defines the structure `date` and the variable `today_date` with initial
values as indicated.

If all of the variables of a particular structure type are defined when
the structure is defined, the structure name can be omitted. So the
statement

  struct {
    int month;
    int day;
    int year;
  } dates[100];

defines an array called `dates` to consist of 100 elements. Each element
is a structure containing three integer members: month, day and year.
Because you did not supply a name for the structure, the only way to
subsequently declare variables of the same type is by explicitly
defining the structure again.

  
Note to self
  Once routines are implemented as *static variables* in C.
end note


# Character Strings
The double quotation marks are used to delimit the character string. 
  "This is a character string.\n"

Character strings can contain any combination of letters, numbers, or
special characters.

Note:
  Be certain you remember that single quotation and double quotation
  marks are used to create two different types of constants in C.
  Single quotes are for character literals.
end note

Some of the more commonly performed operations on character strings
include: 
- combining two character strings together (concatenation),
- copying one character string to another,
- extracting a portion of a character string (sub-string) and
- determining if two strings are equal (that is, if they contain the
  same characters)

Note
  type `wchar_t` can be used for representing so-called wide characters.
end note


## Variable-Length Character Strings
In the C language, the special character that is used to signal the end
of a string is known as the *null* character as is written as *'\0'*

One needs to note *two* nice features that C provides for dealing with
character strings.

The first feature involves the initialization of character arrays. C
permits a character array to be initialized by simply specifying a
constant character string rather than a list of individual characters.
So, for example, the statement

  char word[] = { "Hello!" };

can be used to set up an array of characters called word with the
initial characters 'H', 'e', 'l', 'l', 'o', '!', and '\0', respectively.
You can also omit the braces when initialzing character arrays in this
manner. So, the statement

  char word[] = "Hello!";

is perfectly valid. Either statement is equivalent to the statement

  char word[] = { 'H', 'e', 'l', 'l', 'o', '!', '\0' };

If you're explicitly specifying the size of the array, make certain you
leave enough space for the terminating null character. So, in

  char word[7] = { "Hello!" };

the compiler has enough room in the array to place the terminating null
character. However, in

  char[6] = { "Hello!" };

the compiler can't fit the terminating null character at the end of the
array, and so it doesn't put one there (and it doesn't complain about it
either).

In general, wherever they appear in your program, character-string
constants in the C language are automatically terminated by a null
character. This fact helps functions such as `printf` to determine
when the end of a character string has been reached. So in the call

  printf ("Programming is fun.\n");

the null character is automatically placed after the newline character
in the character string, thereby enabling the `printf` function to
determine when it has reached the end of the format string.

The other feature to be mentioned here involves the display of character
strings. The special format characters `%s` inside a `printf` format
string can be used to display an array of characters that is terminated
by a null character. So, if `word` is a null-terminated array of
characters, the `printf` call

  printf ("%s\n", word);

can be used to display the entire contents of the `word` array at the
terminal. The `printf` function assumes when it encounters the `%s`
format characters that the corresponding argument is a character string
that is terminated by a null character.


## Testing Two Character Strings for Equality
You can directly test two strings to see if they are equal with a
statement such as
  if (string1 == string2)
    ....
because the equality operator can only be applied to simple variable
types, such as `floats`, `ints` and `chars`, and not to more
sophisticated types, such as structures or arrays.

To determine if two strings are equal, you must explicitly compare the
two character strings character by character. If you reach the end of
both character strings at the same time, and if all the characters up to
that point are identical, the two strings are equal; otherwise they are
not.


## Inputing Character Strings
By now you are used to the idea of displaying a character string using
the `%s` format characters. But what about reading in a character string
from your window (or your "terminal window")? Well, on your system,
there are several library functions that you can use to input character
strings. The `scanf` function can be used with the `%s` format
characters to read in a string of characters up to: 

- a blank space, 
- tab character, or 
- the end of the line, 

whichever occurs first. So the statements
  
  char string[81];

  scanf ("%s", string);

have the effect of reading in a character string typed into your
terminal window and storing it inside the character array `string`. Note
that unlike previous `scanf` calls, in the case of reading strings, the
`&` is not placed before the array name.

If the preceding `scanf` call is executed, and the following characters
are entered:

  Shawshank

the string `"Shawshank"` is read in by the `scanf` function and is
stored inside the `string` array. If the following line of text is typed
instead:

  iTunes playlist

just the string `"iTunes"` is stored inside the `string` array because
the blank space after the word "Itunes" terminates the string. If the
scanf call is executed again, this time the string `"playlist"` is
stored inside the `string` array because the scanf function always
continues scanning from the most recent character that was read in.

The `scanf` function automatically terminates the string that is read in
with a null character. So the execution of the preceding scanf call with
the line of text

  abcdefghijklmnopqrstuvwxyz

causes the entire lowercase alphabet to be stored in the first 26
locations of the string array, with `string[26]` automatically set to the
null character.

If `s1, s2` and `s3` are defined to be character arrays of appropriate
sizes, the execution of the statement

  scanf ("%s%s%s", s1, s2, s3);

with the line of text

  micro computer system

results in the assignement of the string "micro" to s1, "computer", to
s2, and "system" to s3. If the following line of text is typed instead:

  system expansion

it results in the assignement of the string "system" to s1, and
"expansion" to s2. Because no further charactes appear on the line, the
scanf function waits for more input to be entered from your terminal
window.

If you place a number after the % in the scanf format string, this tells
scanf the maximum number of charactes to read.

  scanf ("%80s80s%80s", s1, s2, s3);

You have to leave room for the terminating null character that scanf
stores at the end of the array.


## Single Character Input
`getchar()` : read in single character; scanf with %c can also work but
getchar is more appropriate as its more specialised and needs no
arguments.

`gets()` : reads in a line of text.


## The Null String
A character string that contains no characters other than the null
character has a special name in the C language; it is called the 
*null string*

In C, the null string is denoted by an adjacent pair of double quotation
marks. So, the statement

  char buffer[100] = "";

defines a character array called buffer and sets its value to the null
string. Note that that character string `""` is *not* the same as the
character string `" "` because the second string contains a single blank
character.


## Escape Characters
<table>
    <thead>
        <tr>
            <th>Escape Character</th> <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr> <td>\a</td> <td>Audible alert</td></tr>
        <tr> <td>\b</td> <td>Backspace</td></tr>
        <tr> <td>\f</td> <td>Form feed</td></tr>
        <tr> <td>\n</td> <td>New line</td></tr>
        <tr> <td>\r</td> <td>Carriage return</td></tr>
        <tr> <td>\t</td> <td>Horizontal tab</td></tr>
        <tr> <td>\v</td> <td>Vertical tab</td></tr>
        <tr> <td>\\</td> <td>Backslash</td></tr>
        <tr> <td>\"</td> <td>Double quotation mark</td> </tr>
        <tr> <td>\'</td> <td>Single quotation mark</td> </tr>
        <tr> <td>\?</td> <td>Question mark</td> </tr>
        <tr> <td> \nnn </td> <td> Octal character value nnn </td> </tr>
        <tr> <td> \unnnn</td> <td> Univeral character name </td> </tr>
        <tr><td> \xnn </td> <td>Hexadecimal character value</td></tr>
    </tbody>
</table>

It should once again be pointed out that these escape characters are
only considered a single character inside a string. So, the character
string "\033\"Hello\"\n" actually consists of nine characters (not
counting the terminating null): the character '\033', the double
quotation character '\"', the five cahracters in the word "Hello", the
double quotation character once againg, and the new line character.

A *universal character name* is formed by the character `\u` followed by
four hexadicimal numbers or the character \U followed by eight
hexadecimal numbers. It is used for specifying characters from extended
character sets; that is, character sets that require more than the eight
standard bits for internal representation. The universal character name
escape sequence can be used to form identifier names from extended
character sets, as well as to specify 16-bit and 32-bit characters
inside wide character string and character string constants.

## Character Operations
Character variables and constants are frequently used in relational and
arithmetic expressions. To properly use characters in such situations,
it is necessary for you to understand how they are handled by the C
compiler.

Whenever a character constant or variable is used in an expression in C,
it is automatically converted to, and subsequently treated as, an
integer value.

In chapter 6, "Making Decisions," we saw how the expression

    c >= 'a' && c <= 'z'

could be used to determine if the character variable `c` contained a
lowercase letter. As mentioned there, such an expression could be used
on systems that used an ASCII character representation because the
lowercase letters are represented sequetially in ASCII, with no other
characters in between. The first part of the preceeding expression,
which compares the value of `c` against the value of the character
constant 'a', is actually comparing the value of `c` against the
internal representation of the character `'a'`. In ASCII the character
`'a'` has the value `97`, the character `'b'` has the value `98` and so
on. Therefore, the expresion `c >= 'a'` is `TRUE` (nonzero) for any
lowercase character contained in `c` because it has a vaue that is
greater than or equal to `97`. However because there are characters
other than the lowercase letters whose ASCII values are greater than
`97` (such as the open and close braces), the test must be bounded on
the other end to ensure that the result of the expression is `TRUE` for
lowercase characters only. For this reason, `c` is compared against the
character `'z'`, which in ASCII has the value `122`.

Because comparing the value of `c` against the characters `'a'` and
`'z'` in the preceding expression actually compares `c` to the numerical
representations of `'a'` and `'z'`, the expression

    c >= 97 && c <= 122

could equivalently be used to determine if `c` is a lowercase letter.
The first expression is prefered, however, because it does not require
the knowledge of the specific numerical values of the characters `'a'`
and `'z'`, and because its intentions are less obscure.

The `printf` call
  printf ("%i\n", c);
can be used to print out the value that is used to internally represent
the character stored inside `c`. If your system uses ASCII, the
statement

  printf ("%i\n", 'a');
displays 97, for example.

Try to predict what the following two statements would produce:
  
  c = 'a' + 1;
  printf ("%c\n", c);

Because the value of 'a' is 97 in ASCII, the effect of the first
statement is to assign the value of 98 to character variable c. Because
this value represents the character 'b' in ASCII, this is the character
that is displayed by the `printf` call.

Although adding one to a character constant hardly seems practical, the
preceding example gives way to an important technique that is used to
convert the characters '0' through '9' into their correspoding numerical
values 0 through 9. Recall that the character '0' is not the same as the
integer 0, the character '1' is not the same as the integer 1 and so on.
In fact the character '0' has the numerical value 48 in ASCII, which is
what is displayed by the following `printf` call:

  printf ("%i\n", '0');

Suppose the character variable `c` contains one of the characters '0'
through '9' and that you want to convert this value into the
corresponding integer 0 through 9. Because the digits of virtually all
character sets are represented by sequential integer values, you can
easily convert `c` into its integer equivalent by subtracting the
character constant '0' from it. Therefore, if `i` is defined as an
integer variable, the statement

  i = c - '0';

has the effect of converting the chararacter digit stored in `c` into
its equivalent integer value. Suppose `c` contained the character '5',
which, in ASCII, is the number 53. The ASCII value of '0' is 48, so
execution of the preceding statement results in the integer subtraction
of 49 from 53, which results in the integer value 5 being assigned to
`i`. On a machine that uses a character set other than ASCII, the same
result would most likely be obtained, even though the internal
representations of '5' and '0' might differ.

The preceding technique can be extended to convert a character string
consisting of digits into its equivalent numerical representation.


# Pointers
In this chapter you examine one of the most sophisticated features of
the C programming language: *pointers*. In fact the power and
flexibility that C provides in dealing with pointers serve to set it
apart from many other programming languages. Pointers enable you to
effectively represent complex data structures, to change values passed
as arguments to functions, to work with memory that has been allocated
"dynamically" and to more concisely and efficiently deal with arrays.

To understand the  way in which pointers operate, it is first necessary
to industand the concept of <i>indirection</i>. You are familiar with
this concept from your everyday life. For example, suppose you need to
buy a new ink cartridge for your printer. In the company that you work
for, all purchases are handled by the purchasing department. So you call
Jim in purchasing and ask him to order the new cartridge for you. Jim in
turn, calls the local supply store to order the cartridge. This approach
to obtain your new cartridge is actually an indirect one because you are
not ordering the cartridge directly from the supply store yourself.

Thid same notion of indirection applies to the way pointers work in C. A
pointer provides an indirect means of accessing the value of a
particular data item. And just as there are reasons for going through
the purchasing department to order new cartridges (you don't have to
know which particular store the cartridges are being ordered from, for
example), so are there good reasons why, at times, it makes sense to use
pointers in C.

## Defining a pointer variable
It's time to see how pointers actually work. Suppose you define a
variable called `count` as follows:

    int count = 10;

You can define another variable called `int_pointer`, that can be used
to enable you to indirectly access the value of `count` by the
definition:

    int *int_pointer;

The asterisk defines to the C system that the variable `int_pointer` is
of type `pointer` to `int`. This means that `int_pointer` is used in the
program to indirectly access the value of one or more integer values.

You have seen how the `&` operator was used in the `scanf` calls of the
previous programs. This unary operator, know as the <i>address</i>
operator, is used to make a pointer to an object in C. So if `x` is a
variable of a particular type, the expression `&x` is a pointer to that
variable. The expresion `&x` can be assigned to any pointer variable, if
desired, that has been declared to be a pointer to the same type as `x`.

Therefore, with the definitions of `count` and `int_pointer` as given,
you can write a statement such as 

    int_pointer = &count;

to set up the indirect reference between `int_pointer` and `count`. The
address operator has the effect of assigning to the variable
`int_pointer`, not the value of `count` but a pointer to the variable
count.

Always remember that the value of a pointer in C is meaningless until it
is set pointing to something.

## Using Pointers in expressions
The pointer reference operator `*` has higher precedence than the
arithmetic operation of division. In fact, this operator, as well as the
address operator, &, has higher precedence than all binary operators in
C)

You can have as many pointers to the same item as you want.

## Working with Pointers and structures
Given

    struct date {
        int month;
        int day;
        int year;
    };

you can define a variable to be a pointer to a `struct date` variable:

    struct date *date_ptr;

The variable `date_ptr` can then be used in the expected fashion. For
example you can set it to point to `todays_date` with the following
statement

    date_ptr = &todays_date;

After such an assignment has been made, you can then indirectly access
any of the members of the `date` structure pointed to by `date_ptr` in
the following way:

    (*date_ptr).day = 21;

This statement has the effect of setting the day of the `date` structure
pointed to by `date_ptr` to 21. The parentheses are required because the
structure member operator `.` has higher precedence than the indirection
operator `*`.

To test the value of `month` stored in the `date` structure pointed to
by `date_ptr`, a statement such as

    if ( (*date_ptr).month == 12 )
        ....

can be used.

Pointers to structures are so often used that a special operator exists
in the language. The structure pointer `->`, which is a dash followed by
the greater than sign, permits expressions that would otherwise be
written as

    (*x).y

to be more clearly expressed as

    x->y

So the previous `if` statement can be conveniently written as

    if (date_ptr->month == 12)
        ...


## Structures Containing Pointers
Naturally, a pointer also can be a member of a structure. In the
structure definition

    struct int_ptrs {
        int *p1;
        int *p2;
    };

a structure called `int_ptrs` is defined to contain two pointers, the
first one called `p1` and the second one `p2`. You can define a variable
of type `struct int_pntrs` in the usual way:

    struct int_ptrs pointers;

The variable `pointers` can now be used in the normal fashion,
remembering that `pointers` itself is <i>not</i> a pointer, but a
structure variable that has two pointers as its members.

## Linked Lists
The concepts of pointers to structures and structures containing
pointers are very powerful ones in C, for they enable one to create
sophisticated data structures, such as <i>linked lists</i>, 
<i>doubly linked lists</i> and <i>trees</i>.

Suppose one defines a structure as follows:

    struct entry {
        int value;
        struct entry *next;
    };

This defines a structure called `entry`, which contains two members. The
first member of the structure is a simple integer called `value`. The
second member of the structure is a member called `next`, which is a
pointer to an `entry` structure.

The structure operator `.` and the structure pointer operator `->` have
the same precedence in the C language. In expressions where both
operators are used, the operators are evaluated from left to right.

As mentioned, the concept of a linked list is a very powerful one in
programming. Linked lists greatly simplify operations such as insertion
and removal of elements from large lists of sorted items.

In a linked list, members can appear anywhere in memory, unlike in
arrays where they have to appear sequentially in memory.

Before developing some functions to work with linked lists, two more
issues must be discussed. Usually associated with a linked list is at
least one pointer to the list. Often, a pointer to the start of the list
is kept. So, for your original three-element list, which consisted of
`n1`, `n2` and `n3` you can define a variable called `list_pointer` and
set it ti point to the beginning of the list with the statememt

    struct entry *list_pointer = &n1;

assuming that `n1` has been previously defined. A pointer to the list is
useful for sequencing through the entries in t he list, as we see
shortly.

The second issue to be discussed involves the idea of having some way to
identify the end of the list. This is needed so that a procedure that
searches through the list, for example can tell when it has reached the
final element in the list. By convention, a constant value of `0` is usd
for such a purpose and is known as the <i>null</i> pointer. You can use
the null pointer to mark the end of a list by storing this value in the
pointer field of the last entry of the list. This is needed so that a
procedure that searches through the list, for example, can tell when it
has reached the final element in the list. By convention, a constant
value of 0 is used for such a purpose and is known as the <i>null</i>
pointer. You can use the null pointer to mark the end of a list by
storing this value in the pointer field of the last entry of the list.

In your three-entry list, you can mark its end by storing the null
pointer in `n3.next`:

    n3.next = (struct entry *) 0;

The type cast operator is used to cast the constant 0 to the appropriate
type ("pointer to struct entry"). It's not required, but makes the
statement more readable.

Note
	A null pointer is not necessarily internally represented as a value 0.
	However, the compiler must recognize assignment of the constant 0 to a
	pointer as assigning the null pointer. This also applies to comparing
	a pointer against the consta 0: The compiler interpretes it as a test to
	see if the pointer is null.
end Note

begin side note
	*NULL*
	A pointer can be assigned the value 0 to explicitly represent that it
	does not currently have a pointee. Having a standard representation for
	"no current pointee" turns out to be vary handy when using pointers. The
	constant NULL is defined to be 0, a NULL pointer will behave like a
	boolean false when used in a boolean context. Dereferencing a NULL
	pointer is an error which, if you are lucky, the computer will detect at
	runtime -- whether the computer detects this depends on the operating
	system
end side note


## The Keyword `const` and Pointers
You have seen how a variable or an array can be declared as `const` to
alert the compiler as well as the reader that the contents of a variable
or an array will not be changed by the program. With pointers, there are
two things to consider: whether the pointer will be changed, and whether
the value that the pointer points to will be changed. 

Assume the following declarations:

    char c = 'X';
    char *char_ptr = &c;

The pointer variable `char_ptr` is set pointing to the variable `c`. If
the pointer is always set pointing to `c`, it can be declared as a
`const` pointer as follows:

    char * const char_ptr = &c;

(Read this as `char_ptr` is a constant pointer to a character.") So a
statement like:

	char_ptr = &d;	/* not valid */

causes the GNU C compiler to give a message like this:
	
	foo.c:10: warning: assignment of read-only variable 'charPtr'

Now if, instead, the location pointed to by `char_ptr` will not change
<i>through the pointer variable `char_ptr`</i>, that can be noted with
the declaration as follows:

    const char *char_ptr = &c;

(Read as, "char_ptr points to a constant character.") Now of course,
that doesn't mean that the value cannot be changed by the value `c`,
which is what `char_ptr` is set pointing to. It means, however, that it
won't be changed with a subsequent statement like this:

    *char_ptr = 'Y';	/* not valid */

which causes the GNU C compiler to issue a message like this:

<samp>foo.c:11: warning: assignment of read-only location</samp>

In the case in which both the pointer variable and the location it
points to will not be changeable through the pointer, the following
declaration can be used:

    const char * const *char_ptr = &c;

The first use of `const` says that the contents of the location the
pointer references will not be changed. The second use says that the
pointer itself will not be changed.

You should note the implication that a pointer is constrained to point
to a particular kind of object: every pointer points to a specific data
type. (There is one exception: a "pointer to void" is used to hold any
type of pointer but cannot be dereferenced itself)

## Pointers and Functions
One thing worth remembering when dealing with pointers that are sent to
functions as arguments: The value of the pointer is copied into the
formal parameter when the function is called. Therefore, any change made
to the formal parameter by the function does not affect the pointer that
was passed to the function. But here's the catch: Although the pointer
cannot be changed by the function, the data elements that the pointer
references can be changed!

## Pointers and Arrays
One of the most common uses of pointers in C is as pointers to arrays.

When you define a pointer that is used to point to the elements of an
array, you don't designate the pointer as type "pointer to array";
rather, you designate the pointer as pointing to the type of element
that is contained in the array.

If you have an array of characters called `text`, you could define a
pointer to be used to point to elements in `text` with the statement

    char *text_ptr;

Note to self
Given the assigment statement:

    pointer_var = value;

where `pointer_var` was declared as a pointer, say,

    int *pointer_var;

this statement indicates to us that `value` is also os type pointer.

If we had an address operator on the right hand value, `&value` that
would imply that value was a "normal type" variable and we'd be
assigning its memory address to the left hand side pointer.
end note to self

The increment and decrement operators `++` and `--` are particularly
handy when dealing with pointers. Applying the increment operator has
the same effect as adding 1 to the pointer, while applying the decrement
operator has the same effect as subtracting one from the pointer. So, if
`text_ptr` is defined as a `char` pointer and is set pointing to the
beginning of an arrat of chars called text, the statement

	++text_ptr;

sets the `text_ptr` pointing to the next character in `text`, which is
`text[1]`. In a similar fashion, the statement

	--text_ptr;

sets `text_ptr` pointing to the previous character in `text`, assuming,
of course, that `text_ptr` was not pointing to the beginning of text
prior to the execution of this statement.

It is perfectly valid to compare two pointer variables in C. This is
particularly useful when comparing two pointers in the same array. For
example, you can test the pointer `values_ptr` to see if it points past
the end of an array containing 100 elements by comparing it to a pointer
to the last element in the array. So the expression

	values_ptr > &values[99]

is TRUE (nonzero) if `values_ptr` is pointing past the last element in
the `values` array, and FALSE (zero) otherwise. Recall from previous
discussions that you can replace the preceeding expression with its
equivalent

	values_ptr > values + 99


## Pointers to Character Strings
One of the most common applications of using a pointer to an array is as
a pointer to a character string. The reasons are ones of notational
convenience and efficiency.

## Constant Character Strings and Pointers
When a constant character string is passed as an argument to a function,
what is actually passed is a pointer to that character string. In fact
it can be generalized that whenever a constant character string is used
in C, it is a pointer to that character string that is produced. So, if
`text_ptr` is declared to be a character pointer, as in

    char *text_ptr;

then the statement

    text_ptr = "A character string.";

assigns to `text_ptr` a pointer to the constant character string "A
character string"." Be careful to make the distinction here between
character pointers and character arrays, as the type of assignment just
shown is not valid with a character array. So, for example, if `text` is
defined instead to be an array of `chars`, with a statement such as

	char text[80];

then you <i>could not</i> write a statement such as

	text = "This is not valid.";

The only time that C lets you get away with performing this type of
assigment to a character array is when initializing it, as in

	char text[80] = "This is okay.";

Initializing the `text` array in this manner does not have the effect of
storing a pointer to the character string "This is okay." inside text,
but rather the actual characters themselves inside corresponding
elements of the `text` array.

If `text` is a character pointer, initializing `text` with the statement

	`char *text = "This is okay.";`

assigns to it a pointer to the character string "This is okay."

As another example of the distinction between character strings and
character string pointers, the following sets up an array called `days`,
which contains pointers to the names of the days of the week.

    char *days[] = { "Sunday", "Monday", "Tuesday", "Wednesday",
                                     "Thursday", "Friday", "Saturday" };

The array `days` is defined to contain seven entries, each a pointer to
a character string. So days[0] contains a pointer to the character
string "Sunday", days[1] contains a pointer to the string "Monday", and
so on. You could display the name of the third weekday, for example,
with the following statement:

	printf("%s\n", days[3]);


## The `printf` Function
You have seen in various program examples how you could place certain
characters between the `%` character and the specific so-called
conversion character to more precisely control the formatting of the
output. For example, you saw in Program 5.3A how an integer value before
the conversion character could be used to specify a <i>field width</i>.
The format characters `%2i` specified the display of an integer value
right-justified in a field width of two columns. You saw in exercise 6
in Chapter 5, "Program Looping", how a minus sign could be used to
left-justify a value in a field.

The general format of a `printf` conversion specification is as follows:

	%[flags][width][.prec][hlL]type

Optional fields are enclosed in brackets and must appear in the order
shown. Tables 16.1, 16.2 and 16.3 summarize all possible characters and
values that can be placed directly after the % sign and before the type
specification inside a format string.

## Table 16.1 `printf` Flags
Flag		Meaning
----------------------------------------------------------------------
-		Left-justify value
+		Precede value with + or -
(space)		Precede positive value with space character
0		Zero fill numbers
#		Precede octal value with 0, hexadecimal value with 0x
		(or 0X); display decimal point for floats; leave
		trailing zeroes for g or G format.
-----------------------------------------------------------------------


## Table 16.2 `printf` Width and Precision Modifiers
Specifier	Meaning
-----------------------------------------------------------------------
number		Minimum size of field.
`*`		Take next argument to printf as size of field
.number		Minimum number of digits to display for integers; number
		of decimal places for e of f formats; maximum number of
		significant digits to display for g; maximum number of
		characters for s format.
`.*`		Take next argument to `printf` as precision (and
		interprete as indicated in preceding row)         
------------------------------------------------------------------------


## Table 16.3 `printf` Type Modifiers
Type		Meaning
-----------------------------------------------------------------------
hh		Display integer argument as a character
`h*`		Display short integer
`l*`		Display long integer
`ll*`		Display long long integr
L		Display long double
`j*`		Display `intmax_t` or `uintmax_t` value
`t*`		Display `ptrdiff_t` value
`z*`		Display `size_t` value
-----------------------------------------------------------------------


## Table 16.4 `printf` Conversion Characters
--------------------------------------------------------------------------
*Char*		*Used to Display*
---------------------------------------------------------------------------
i or d		Integer
--------------------------------------------------------------------------
u		Unsigned integer
--------------------------------------------------------------------------
o		Octal integer
--------------------------------------------------------------------------
x		Hexadecimal integer, using a-f
--------------------------------------------------------------------------
X		Hexadecimal integer, using A-F
--------------------------------------------------------------------------
f or F		Floating-point number, to six decimal places by default
--------------------------------------------------------------------------
e or E		Floating-point number in exponential format (e places
		lowercase e before the exponent, E places uppercase E
		before exponent)
--------------------------------------------------------------------------
g		Floating-point number in f or e format
--------------------------------------------------------------------------
G		Floating-point number in F or E format.
--------------------------------------------------------------------------
a or A		Floating-point number in the hexadecimal format
		0xd.ddddp±d
--------------------------------------------------------------------------
c		Single character
--------------------------------------------------------------------------
s		Null-terminated character string
--------------------------------------------------------------------------
p		Pointer
--------------------------------------------------------------------------
n		Doesn't print anything; stores the number of characters
		written so far by this call inside the `int` pointed to
		by the corresponding argument
--------------------------------------------------------------------------
%		Percent sign.
---------------------------------------------------------------------------


## Pointers and Memory Addresses
When you apply the address operator to a variable in C, the value that
is generated is the actual address of that variable inside the computer's memory.


A *structure* declaration that is not followed by a list of variables
reserves no storage; it merely describes a template or shape of a
structure.

Pointers provide for machine-independent address arithmetic.

C provides the fundamental control-flow constructions required for
well-structured programs: statements grouping:
- Decision making (`if-else`),
- Selecting one of a set of possible values (`switch`)
- Looping with termination test at the top (`while`, `for`) or
- at the bottom (`do`), and
- early loop exit (`break`).

Variables may be internal to a function, external to a function,
external but known only within a single file, or visible to the entire
program.

## Expressions
+ Variable names
+ Function names
+ Array names
+ Constants
+ Function calls
+ Array references
+ Structure references
+ Union references
The above are all considered expressions. Applying a unary operator
(where appropriate) to one of these expressions is also an expression,
as is combining two or more of these expressions with a binary or
ternary operator. Finally, an expression enclosed within parentheses is
also an expression.

An expression of any type other than `void` that identifies a data
object is called an *lvalue*. If it can be assigned a value, it is known
as a *modifiable lvalue*.

Modifiable lvalue expressions are required in certain places. The
expression on the left-hand side of an assignment operator must be a
modifiable lvalue. Furthermore, the incerement and decrement operators
can only be applied to modifiable lvalues, as can the unary address
operator `&` (unless it's a function).

##6.2 Structures and functions
153 (153 of 318) TCPL R&K
