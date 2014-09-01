---
layout: post
title: "Ruby Regular Expressions"
description: ""
category: Programming
tags: ["Ruby","Regular Expressions", "Text Processing"]
---
{% include JB/setup %}


# Regular expressions in Ruby

    /three/                # A regular expression literal

Note that interpolation is done early, before the content of the
regular expression is parsed. This means that any special characters
in the interpolated expression become part of the regular expression
syntax. Interpolation is normally done anew each time a regular
expression literal is evaluated. If you use the o modifier, however,
this interpolation is only performed once, the first time the code is
parsed. The behavior of the o modifier is best demonstrated by example: 

    [1,2].map{|x| /#{x}/}   # => [/1/, /2/]
    [1,2].map{|x| /#{x}/o}  # => [/1/, /1/]

## Regexp Factory Methods
As an alternative to regexp literals, you can also create regular
expressions with Regexp.new, or its synonym, Regexp.compile: 

    Regexp.new("Ruby?")                          # /Ruby?/
    Regexp.new("ruby?", Regexp::IGNORECASE)      # /ruby?/i
    Regexp.compile(".", Regexp::MULTILINE, "u")  # /./mu

Use the Regexp.escape to escape special regular expression characters
in a string before passing them to the Regexp constructor: 

    pattern = "[a-z]+"                # One or more letters
    suffix = Regexp.escape("()")      # Treat these characters literally
    r = Regexp.new(pattern + suffix)  # /[a-z]+\(\)/

In Ruby 1.9, the factory method Regexp.union creates a pattern that
is the "union" of any number of strings or Regexp objects. (That is,
the resulting pattern matches any of the strings matched by its
constituent patterns.) Pass any number of arguments or a single array
of strings and patterns. This factory method is good for creating
patterns that match any word in a list of words. Strings passed to
Regexp.union are automatically escaped, unlike those passed to new
and compile: 

    # Match any one of five language names.
    pattern = Regexp.union("Ruby", "Perl", "Python", /Java(Script)?/)

    # Match empty parens, brackets, or braces. Escaping is automatic:
    Regexp.union("()", "[]", "{}")   # => /\(\)|\[\]|\{\}/

## Regular Expression Syntax
Many programming languages support regular expressions, using the
syntax popularized by Perl. This book does not include a complete
discussion of that syntax, but the following examples walk you
through the elements of regular expression grammar. The tutorial is
followed by Table 9-2, which summarizes the syntax. The tutorial's
focus is on Ruby 1.8 regular expression syntax, but some of the
features available only in Ruby 1.9 are demonstrated as well. For
book-length coverage of regular expressions, see Mastering Regular
Expressions by Jeffrey E. F. Friedl (O'Reilly). 

# Literal characters
    /ruby/             # Match "ruby". Most characters simply match themselves.
    /¥/                # Matches Yen sign. Multibyte characters are suported
                       # in Ruby 1.9 and Ruby 1.8.
# Character classes

    /[Rr]uby/          # Match "Ruby" or "ruby"
    /rub[ye]/          # Match "ruby" or "rube"
    /[aeiou]/          # Match any one lowercase vowel
    /[0-9]/            # Match any digit; same as /[0123456789]/
    /[a-z]/            # Match any lowercase ASCII letter
    /[A-Z]/            # Match any uppercase ASCII letter
    /[a-zA-Z0-9]/      # Match any of the above
    /[^aeiou]/         # Match anything other than a lowercase vowel
    /[^0-9]            # Match anything other than a digit

# Special character classes

    /./                # Match any character except newline
    /./m               # In multiline mode . matches newline, too
    /\d/               # Match a digit /[0-9]/
    /\D/               # Match a nondigit: /[^0-9]/
    /\s/               # Match a whitespace character: /[ \t\r\n\f]/
    /\S/               # Match nonwhitespace: /[^ \t\r\n\f]/
    /\w/               # Match a single word character: /[A-Za-z0-9_]/
    /\W/               # Match a nonword character: /[^A-Za-z0-9_]/

# Repetition

    /ruby?/            # Match "rub" or "ruby": the y is optional
    /ruby*/            # Match "rub" plus 0 or more ys
    /ruby+/            # Match "rub" plus 1 or more ys
    /\d{3}/            # Match exactly 3 digits
    /\d{3,}/           # Match 3 or more digits
    /\d{3,5}/          # Match 3, 4, or 5 digits

# Nongreedy repetition: match the smallest number of repetitions

    /<.*>/             # Greedy repetition: matches "<ruby>perl>"
    /<.*?>/            # Nongreedy: matches "<ruby>" in "<ruby>perl>" 
                   # Also nongreedy: ??, +?, and {n,m}?
# Grouping with parentheses

    /\D\d+/            # No group: + repeats \d
    /(\D\d)+/          # Grouped: + repeats \D\d pair
    /([Rr]uby(, )?)+/  # Match "Ruby", "Ruby, ruby, ruby", etc.
    # Backreferences: matching a previously matched group again
    /([Rr])uby&\1ails/ # Match ruby&rails or Ruby&Rails
    /(['"])[^\1]*\1/   # Single or double-quoted string
                   #   \1 matches whatever the 1st group matched
                   #   \2 matches whatever the 2nd group matched, etc.

# Named groups and backreferences in Ruby 1.9: match a 4-letter palindrome

    /(?<first>\w)(?<second>\w)\k<second>\k<first>/
    /(?'first'\w)(?'second'\w)\k'second'\k'first'/ # Alternate syntax

# Alternatives

    /ruby|rube/        # Match "ruby" or "rube"
    /rub(y|le))/       # Match "ruby" or "ruble"
    /ruby(!+|\?)/      # "ruby" followed by one or more ! or one ?

# Anchors: specifying match position

    /^Ruby/            # Match "Ruby" at the start of a string or internal line
    /Ruby$/            # Match "Ruby" at the end of a string or line
    /\ARuby/           # Match "Ruby" at the start of a string
    /Ruby\Z/           # Match "Ruby" at the end of a string
    /\bRuby\b/         # Match "Ruby" at a word boundary
    /\brub\B/          # \B is nonword boundary:
                       #   match "rub" in "rube" and "ruby" but not alone
    /Ruby(?=!)/        # Match "Ruby", if followed by an exclamation point
    /Ruby(?!!)/        # Match "Ruby", if not followed by an exclamation point

# Special syntax with parentheses

    /R(?#comment)/     # Matches "R". All the rest is a comment
    /R(?i)uby/         # Case-insensitive while matching "uby"
    /R(?i:uby)/        # Same thing
    /rub(?:y|le))/     # Group only without creating \1 backreference

# The x option allows comments and ignores whitespace

    /  # This is not a Ruby comment. It is a literal part
       # of the regular expression, but is ignored.
       R      # Match a single letter R
       (uby)+ # Followed by one or more "uby"s
       \      # Use backslash for a nonignored space
    /x                 # Closing delimiter. Don't forget the x option!


`/[a-z0-9A-Z]+/` => matches alphanumerics, pattern used to match words.

This is a list of some of the elements which can be used in regular expressions...

    ^  => beginning of a line or string
    $  => end of a line or string
    .  => any character except newline
    *  => 0 or more previous regular expression
    *? => 0 or more previous regular expression (non greedy)
    +  => 1 or more previous regular expression
    +? => 1 or more previous regular expression (non greedy)
    [] => range specification (e.g. [a-z] means a character in the range ‘a’ to ‘z’)
    \w => an alphanumeric character
    \W => a non-alphanumeric character
    \s => a white space character
    \S => a non-whitespace character
    \d => a digit
    \D => a non-digit character
    \b => a backspace (when in a range specification)
    \b => word boundary (when not in a range specification)
    \B => non-word boundary
    *  => zero or more repetitions of the preceding
    +  => one or more repetitions of the preceding
    {m,n} => at least m and at most n repetitions of the preceding
    ?  => at most one repetition of the preceding
    |  => either the preceding or next expression may match
    ()  => a group

escape "special" chars with `\`

    puts( 'ask who?/what?'.match( /who\?\/w..t\?/ ) )
    #=> who?/what?


`//` => Regex delimeter
`[]` => Range delimiter

When you have two captures in your regexp delimiter, say,
`/(.)(.)/`, they both have to evaluate to true or else the
response will be nil and the $ variables will not be initialized.

    .sub(pattern, hash) -> string => replaces the first occurrence
    .gsub => replaces all occurrences.

`/(.)(.)(.)/` => The results got by comparing the three different
captures are stored in variables $1, $2 and $3. They are like
3 different matches being made "in turn", on the same string. Each
comparison is made on its own but they all have to pass otherwise non
of the $ variables will be initialized and it'll return nil.

    /[Rr]uby/        # Matches "Ruby" or "ruby"
    /\d{5}/          # Matches 5 consecutive digits

Regexp and Range objects define the normal == operator for testing
equality. In addition, they also define the === operator for testing
matching and membership. Ruby's case statement (like the switch
statement of C or Java) matches its expression against each of the
possible cases using ===, so this operator is often called the case
equality operator. It leads to conditional tests like these: 


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
