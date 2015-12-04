

If you think you might want to use this calculator, be aware that
<font color='red'>it is intentionally made to appear simple</font>.
There are around 100 commands, but most of them will be pretty obvious
and familiar, such as `sin`, `cos`, `sqrt`, `+`, `pow`, etc.  Most of
the remaining commands have to do with either changing the
configuration, display, or manipulating the stack.

The calculator does not include statistics, programmability, root
finding, and other such things found on modern calculators.  If I need
these things, I'll turn to other programs or write a python script
using numpy or mpmath.

<font color='red'><b>Warning:  this calculator code is intended to be<br>
used by a single user and the code is not thread-safe.  If you wish to<br>
have it installed for a number of users, give each user a copy of the<br>
code.  I could have made it thread safe and multi-user, but I deemed<br>
there was no need for this, as it's "personal" code and users will<br>
probably want to change the code to their own tastes.</b>
</font>



<a href='Hidden comment: ------------------------------------------------------------'></a>
# Comparison to the HP-42s Calculator #

About half of the commands of the hcpy calculator map directly to commands
of the HP-42s calculator.  The x register is the top of the stack and
the y register is the next element.  By the way, if you want an
emulator for the HP-42s, a nice one can be found at
http://free42.sourceforge.net/ that runs on a variety of platforms.

![http://www.gdssw.com/img/free42.jpg](http://www.gdssw.com/img/free42.jpg)

| **HP-42s** | **hcpy** | **Meaning** |
|:-----------|:---------|:------------|
|+           | +        | Addition    |
|-           | -        | Subtraction |
|`*`         | `*`      | Multiplication|
|/           | /        | Division    |
| y<sup>x</sup>     | pow      | Raise y to the power x |
|%CH         | %ch      | Calculate 100`*`(x - y)/y|
|%           | no equiv.| Calculate x% of y|
|COMB        | comb     | Number of combinations of y items taken x at a time|
|PERM        | perm     | Number of permutations of y items taken x at a time|
|AND         | and      | Bit-wise AND|
|OR          | or       | Bit-wise OR |
|NOT         | ~        | Bit-wise negation|
|XOR         | xor      | Bit-wise exclusive OR|
| 1/x        | 1/x      | Reciprocal  |
|+/-         | chs      | Change the sign of x|
|ABS         | abs      | Take the absolute value of x|
|pi          | pi       | Put pi in the x register|
|x2          | square   | Calculate x`*`x|
|sqrt        | sqrt     | Calculate the square root of x|
|10<sup>x</sup> | Use pow  | Raise 10 to the power of x|
|LOG         | log      | Base 10 logarithm|
|e<sup>x</sup> | exp      | Exponential function|
|LN          | ln       | Natural logarithm|
|SIN         | sin      | Trigonometric sine|
|COS         | cos      | Trigonometric cosine|
|TAN         | tan      | Trigonometric tangent|
|ASIN        | asin     | Trigonometric inverse sine|
|ACOS        | acos     | Trigonometric inverse cosine|
|ATAN        | atan     | Trigonometric inverse tangent|
|FP          | fp       | Take the fractional part of x (i.e., x - int(x))|
|IP          | ip       | Take the integer part of x (i.e., int(x))|
|N!          | fact     | Calculate x!|
|GAM         | gamma    | Gamma function of x|
|ROLL        | roll     | Roll the stack|
|x<>y        | xch      | Exchange x and y|
|CLRG        | clrg     | Clear the registers|
|CLST        | clst     | Clear the stack|
|LAST x      | lastx    | Recall the last x value used|
|-->HR       | 2hr      | Convert from hours, minutes, seconds to decimal hours|
|-->HMS      | 2hms     | Convert from decimal hours to hours minutes, seconds |
|-->DEG      | 2deg     | Convert x from radians to degrees |
|-->RAD      | 2rad     | Convert x from degrees to radians |
|SHOW        | show     | Show the value of x to the current precision|
|RAN         | rand     | Generate a random decimal fraction between 0 and 1|
|PRST        | prst     | Print the stack|
|FIX         | fix      | Display numbers to a fixed number of decimal places|
|ENG         | eng      | Display numbers in engineering notation |
|SCI         | sci      | Display numbers in scientific notation |
|DEG         | deg      | Use degrees for trigonometric calculations|
|RAD         | rad      | Use radians for trigonometric calculations|
|RECT        | rect     | Display complex numbers in rectangular form|
|POLAR       | polar    | Display complex numbers in polar form|




<a href='Hidden comment: ------------------------------------------------------------'></a>
# Installation #

Unzip the distribution package and run the program by typing

> `python hc.py`

The other python files in the package need to be imported by hc.py, so
you may have to make sure they are in a directory in you python search
path.  I'm using python 2.5 and python seems to find the associated
files fine.  I run the program from a bash shell under cygwin.

To run this program, you'll need to have python set up on your system
(get it free at http://www.python.org/).  You'll also need mpmath (a
free pure-python multiprecision library; get it at
http://code.google.com/p/mpmath/).

<font color='red'>
You'll want to customize the things in the <code>cfg</code> dictionary in the<br>
<code>hc.py</code> file</font>.  Make sure to set things like the editor program,
and, if you want persistence, the file names to persist the settings
to.  Personally, I often set cfg to the way I want the calculator to
wake up, then leave all persistence off.  If there are settings I want
to have handy, I just put them in a file and source the file with the
<<file command.

Once you have the calculator running, the two most important commands
to know are `quit` to exit the calculator (can be shortened to `q`)
and the `?` command, which provides help.  By itself, you get a list
of available commands.  With a command name (no space between the name
and the `?`), you'll get help on that command.

Commands don't have to be entered in their entirety:  you can enter
just enough so they're not ambiguous.

## Changes needed to mpmath ##

In using version 0.10 of mpmath, I had to make some changes to get
things to work  These are all changes to the class mpi in mptypes.py:

```
Add the method:

    def __str__(self):
        return mpi_str(self._val, mp.prec)

Change the __repr__ method of class mpi to:

    return "mpi(" + repr(self.a) + ", " + repr(self.b) + ")"

Comment out the line that reads

    __str__ = __repr__

Add the method:

    def __ne__(self, other):
        return not (self == other)
```

If you're using version 0.11 of mpmath, the last change doesn't need
to be made.

## Setting up your editor ##

The `hc.py` file has two entries in the `cfg` dictionary keyed by
`editor` and `tempfile`.  If you wish to use the `ec` command to
be able to edit the configuration, you'll want to set the `editor`
entry to point to your editor program.

The tempfile entry was originally set to a statically-defined name
during development for debugging things.  This is no longer needed,
as the temporary file's name is gotten via python's standard library
module tempfile.  Thus, you should leave the tempfile entry blank
unless you want to debug the dictionary and list editing functionality
of the program.

<a href='Hidden comment: ------------------------------------------------------------'></a>
# The Configuration #

The default configuration of the calculator is initially controlled
by the cfg dictionary in the hc.py file (near the top of the file).
The settings are documented with comments as to what they do and how
you might want to change them.  Some of the settings are things that
would likely be changed while using the calculator, so they have
calculator commands associated with them (e.g., the number of digits
displayed).  Others are settings you'll probably set to your tastes
and not change again.

The mpFormat object in the mpformat.py file offers relatively fine
control over how real numbers are changed into strings.  You may
want to edit some of the class variables to suit your tastes.

While running the calculator, you can edit the configuration at any
time by the `ec` command.  This invokes your editor on a text
representation of the cfg dictionary (minus the comments, however).
This was deemed more appropriate than cluttering the user interface
with lots of commands.

If you turn persistence on and save the configuration, the saved file
will override the default settings.  If you want to restart using the
default settings, use the `-d` command line option.

<a href='Hidden comment: ------------------------------------------------------------'></a>
# Architecture #

Here, by architecture, I mean how you can mentally think of the
calculator, not the software architecture of the code.  The
architecture is simple:

<pre>

+-------------------------------------------+<br>
|                                           |<br>
| Configuration data (indirectly accessible |<br>
| to user via commands or editing).         |<br>
|                                           |<br>
+-------------------------------------------+<br>
|                                           |<br>
| Stack data                                |<br>
|                                           |<br>
+-------------------------------------------+<br>
|                                           |<br>
| Named registers                           |<br>
|                                           |<br>
+-------------------------------------------+<br>
<br>
</pre>

These are all the features that are available to you as a user.

<a href='Hidden comment: ------------------------------------------------------------'></a>
# Numbers #

The calculator uses the following number types:

  1. Integers
  1. n-bit integers
  1. Rational numbers
  1. Real numbers
  1. Complex numbers
  1. Date/times
  1. Interval numbers

## Integers ##

The integers are the ordinary integers of python, which are signed
and of arbitrary precision.  They will behave as you expect an integer
to.  When they get too large, you can use the `brief` command to make
them fit on one line (use the command `1 brief`).

There are two types of division with integers.  Normal division using
the `/` operator will, in general, return a rational number when
dividing two integers.  Integer division using the `//` operator will
always return an integer.  However, there's something you should know
about this integer division.

In python, integer division is called floor division because it
essentially works by performing the division in floating point using
the absolute values, calculating the floor of the result, then
attaching the sign.  This behavior can be surprising to e.g. a C
programmer.  For example, 3//8 is 0, as one would expect.  However,
(-3)//8 is -1, whereas in a language like C, this integer division
would result in zero.

Because of this behavior, the cfg dictionary in the hc.py file has an
entry keyed by `C_division`.  When the setting is True, (-3)//8 will
return 0.  Since this is probably what most people expect, it is
set to True by default in the code.

## n-bit integers ##

Since python's integers are of arbitrary size, it sometimes would be
nice to be able to simulate a fixed-size integer.  The n-bit integer
facility in the calculator allows this.  You can simulate both signed
and unsigned integers.  The signed integers simulate using 2's
complement arithmetic.

> Please consider the n-bit integer functionality freshly-developed
> code as, well, that's what it is.  Both python's arbitrary
> integers and the n-bit integers are implemented in the Zn class in
> the integer.py file.  While I think I've gotten most things right,
> I recommend you assume there are bugs in this code until it's been
> wrung out pretty well and declared production-ready.  Thus, take
> its output with some hefty grains of salt.  If you find the
> calculator makes a mistake, **please** submit an issue report so it
> can be fixed.  An integer bug will automatically become the
> highest priority defect!

To start using n-bit integers, you use the `int` and `uint` commands.
For illustration purposes here, I'll assume we want to calculate with
4-bit integers.  The int4 command turns on signed 4-bit arithmetic.
Any integers which are subsequently entered will be converted to 4-bit
integers, which of course means large integers will have upper bits
chopped off.  This will also happen to any integers on the stack or in
registers (i.e., all integer objects in the calculator).  If you enter
the number -5 on the stack, you'll see it displayed as

` x:  -5<s4>`

indicating it's a signed 4-bit integer.

Use the `int` command to set the calculator back to regular python
integers.

Use `uint4` to change to unsigned 4-bit integers.  The above stack
display will change to

` x:  11<u4>`

This is because the number -5 in 2's complement format is represented
by the 0b1011 bit pattern (i.e., the unsigned number 11).

There are two subtleties to n-bit integers using 2's complement.  The
first is that there's a most-negative number.  For 4-bit integers,
this number is -8.  If you execute `chs` on this number to change the
sign, the result is -8.  This is correct for 2's complement
arithmetic.  You can change this behavior if you want by changing the
`negate_zero` class variable in the Zn class in integer.py.  Read the
comments on the Zn.neg method for the details of what's going on.

The second subtlety is that you can't take the absolute value of the
most-negative number because there is no corresponding positive
number.  You'll get an error message if you try to.

If you are using n-bit integers and you exit the program, the n-bit
integer state is not persisted if you have persistence turned on.
This is by design, but changing the code so it does persist would
not be too difficult.

Note that n-bit integers and regular python integers cannot coexist in
the calculator.  While this wouldn't have been hard to support, it's
not clear how things should behave.  For example, what would the sum
of a python integer and a 4-bit integer be?  Should it be a regular
python integer (thus "promoting" the 4-bit integer) or should it be a
4-bit integer (thus chopping bits off the python integer)?  And
because the python integer is signed, what should be the result of an
operation with an unsigned n-bit integer?  Because of these questions,
I chose to make python integers and n-bit integers mutually exclusive
(i.e., you can only use one of these types at a time in the
calculator).

## Rational numbers ##

Rational numbers are supported by the calculator and are implemented
as two python arbitrary integers, one for the numerator and one for
the denominator.  The `mix` command lets you toggle between displaying
improper fractions and mixed fractions.  If an operation results in a
rational number with a denominator of 1, the number is displayed as if
it was an integer (i.e., its type is still a rational unless
downcasting is on).

The rational number is always reduced to its lowest terms.

Sometimes you might want integer division to result in a real.  The
cfg["no\_rationals"] setting can be used to turn off Rational results
for division.  The `rat` command also changes this setting (use
`0 rat` to turn rationals off).

## Real numbers ##

The real numbers are implemented with mpmath's mpf numbers and are
thus multiprecision numbers.  The default precision is 15 digits.  You
can set it using the `prec` command.  The default precision should be
adequate for most work, although it's handy to change the precision to
see if roundoff error is affecting your calculations.  This works in
general, but if your calculations are done with the mpmath functions
that support mpmath's interval numbers, you'll probably want to use
interval numbers instead, as they can quantify the effects of roundoff
error.

`inf` and  `-inf` are supported numbers and represent positive and
negative infinity.  You can do arithmetic with them where it makes
sense.  If the `allow_divide_by_zero` is True in the cfg dictionary
in hc.py, you can also perform division by zero, which will result in
infinity of the proper sign.

Some operations will result in "not a number", symbolized by `NaN`,
although you can't enter this element directly.  Example:  `inf sin`
will result in `NaN`.

## Complex numbers ##

Complex numbers are implemented with mpmath's mpc numbers, which are
two mpf numbers.  Virtually all of the mpmath functions are defined
over the complex domain, so this is also true of the hcpy calculator.

You enter complex numbers using the `i` or `j` symbols or as an
ordered pair.  For example, here are some complex numbers being
entered on the stack:

<pre>
> i<br>
x: i<br>
> 1+j<br>
x:  1.000 +  1.000i<br>
> -7+i43<br>
x: -7.000 +  43.00i<br>
> i17<br>
x:  17.00i<br>
> (-1,-1)  # Note there can't be a space between the numbers<br>
x: -1.000 - 1.000i<br>
</pre>

You can display complex numbers in rectangular (`rect`) or polar
(`polar`) form.  If you like to see them as ordered pairs rather than
with `i` or `j`, you can change a setting in the `cfg` dictionary.

## Date/times ##

Dates and times are real numbers that are interpreted to be
astronomical Julian day numbers.  Here's an example using the `T`
command to convert 0 to a date:

<pre>
> 0 T<br>
x:  1Jan-4712:12:00<br>
</pre>

Note it's displayed as a date followed by a time.  The fractional
part of a Julian day number represents the fraction of a 24 hour day.

The zeroth Julian day is a little over 6700 years ago.  If
you're interested in the details of the algorithms used, consult
_Astronomical Algorithms_, by Jean Meeus, 2nd ed., Willman-Bell,
1998.

Internally, the dates are stored as either a real number or an
interval number.  Thus, any number can be converted to a date because
any number can be converted to a real or interval number.  You can
perform the usual arithmetic operations on dates, but you can't
divide by a date.  Other operations will give error messages.

Here are some different ways of entering dates:

<pre>
22Feb1937<br>
22Feb1937:23:18<br>
1.5Jan<br>
:8<br>
:8:30<br>
</pre>

The first two represent a date in 1937.  If the time is not entered,
it will be 00:00 am (00:00:00).  You can also enter the day and a
decimal fraction of a day for the time.  If you use a ":" to enter
the time, it will take "precedence" over the fractional part of the
day.

If you leave out the year, it defaults to the current year.  If you
leave out the month, it defaults to the current month.  If you just
give a time like the last two, it defaults to the current day.

The julian.py file contains the object that implements the Julian
object.  You can edit its class variables to your taste -- for
example, you can rename the month names to whatever you prefer.

The main arithmetic operations intended with these date numbers is
to add integers and reals to them, which are in units of days.  This
makes it easy to find a date a particular number of days in the future
or past.  You can also subtract two dates to find the number of days
between two dates, but since the result is another date, you need to
use the `I` or `R` command to convert it to an integer or real.

Suppose you were born on 1Jan1970 and suppose your life expectancy is
80 years.  How many days do you have left to live?

<pre>
> 1Jan1970 80 365.25 * +<br>
x:  1Jan2050:00:00<br>
> today - I<br>
x:  14937<br>
</pre>

Note the use of `today`, which results in 00:00 am for today.  You can
also use `now` which represents the current local time.

## Interval numbers ##

Interval numbers are implemented with mpmath's mpi numbers, which are
implemented using two mpf numbers.  You can enter them in three
different ways.  The following example shows the different ways of
entering the interval number `[3, 5]`:

  1. [3,5]
  1. 4+-1
  1. 4(1)
  1. 4(25%)

Note there can be no space characters in the number.  The last three
forms show you can enter the midpoint of the interval and the
half-width, either expressed as a number or a percentage of the
midpoint value.

You can perform interval arithmetic with the usual arithmetic
functions and call the functions `sqrt`, `exp`, `ln`, `log`, `pow`,
`sin`, `cos`, and `tan` with interval numbers.  You will get "strange"
error messages if you call other functions with interval numbers
(these are strange because they are mpmath's exception messages being
passed on).  Here's an example:

<pre>
> 3(1)<br>
x:  3.000 ( 33.33%)<br>
> gamma<br>
gamma of a <class 'mpmath.mptypes.mpi'><br>
</pre>

The `iva`, `ivb`, and `ivc` commands control how interval numbers are
displayed.

## Number Conversions ##

The various number types can be converted into the other number types.
Some conversions will lose information, such as converting a complex
number to an integer, rational, or real.

These conversions are done in the `Convert` function in the convert.py
file.  This file is easy to change; for example, you might want to
raise an exception when converting a complex number to a real; this
would force the user to decide how to make the conversion.


## SI Prefixes ##

In engineering and scientific work, it is quite common to use the SI
prefixes with units.  The calculator uses these prefix symbols in two
ways.  First, the `engsi` command causes real numbers (including
complex and interval numbers too) to be displayed with an SI prefix
letter as a suffix if one is in range.

Second, you can use the SI prefixes when you enter numbers.  Note that
the prefix letter applies to the whole number.

<pre>
> 1m<br>
x:  0.00100 0<br>
> 1/2k<br>
x:  500<br>
> 1+iu<br>
x:  0.00000 1000 + 0.00000 1000i<br>
> [1,2]M<br>
x:  1,500,000. +- 500,000.<br>
</pre>

If the SI prefix is a positive power of ten, then integers and
rationals will remain integers and rationals.  Otherwise, they are
converted to reals.

SI prefixes are not allowed with date/time numbers.

<a href='Hidden comment: ------------------------------------------------------------'></a>
# Commands by Function #

Most of these functions' purposes should be pretty obvious by their
name.  If you're not fond of the names, it's trivial to change them
(see the customization section).

I won't discuss the "obvious" functions -- if you're not sure what
something is, use the `?` command on it.  Also, some of the functions
may already have been explained adequately in the table above relating
the calculator's commands to the HP-42s calculator.

## Arithmetic and bit manipulation ##

> <b><font color='green'><code>% * + - / // &lt;&lt; &gt;&gt; and int or uint xor ~</code></font></b>

`%` will do modular arithmetic by dividing y by x and returning the
remainder.  This works with all numbers except complex and interval
numbers.  The result will be an integer if both x and y are integers;
otherwise the result will be real.  See also the `mod` command.

The other commands are:

  * `<<` Shift y left x bits
  * `>>` Shift y right x bits
  * `and` Logical AND of x and y
  * `or` Logical OR of x and y
  * `xor` Logical XOR of x and y
  * `~` Negate all the bits of x

## Elementary functions ##

> <b><font color='green'><code>1/x abs acos asin atan atan2 ceil chs conj cos exp fact floor</code></font></b>
> <b><font color='green'><code>im ln ln2 log log10 log2 pow re sin sqrt square tan</code></font></b>

  * 1/x calculates the reciprocal of x.
  * abs calculates the absolute value of x (magnitude of a complex number).
  * atan2 is the arc tangent of y/x and it gets the quadrant correct.
  * ceil is the ceiling function (smallest integer >= x).
  * floor is the floor function (largest integer <= x).
  * conj calculates the complex conjugate of x.
  * re and im take the real and imaginary parts of a complex number.

fact is the factorial of x.  It will calculate the exact factorial for
integer values and use the gamma function for other values.  If you
want all factorials of integer values to be calculated exactly, set
the `factorial_limit` key in the cfg dictionary in hc.py to 0.
Otherwise, it defines the limit over which the gamma function is used.

## Conversions ##

> <b><font color='green'><code>2deg 2hms 2hr 2rad C I Q QQ R T V chop</code></font></b>

`2deg` converts a value in radians to degrees.  `2rad` converts a
value in degrees to radians.

`2hms` converts a decimal number of hours to a decimal format where
the integer part represents hours, the first two digits of the decimal
fraction represent minutes, and the remaining parts of the decimal
fraction represents decimal seconds.  `2hr` converts a number in this
hour, minute, second format back into a decimal number of hours.

  * `I` converts x to an integer (n-bit integer if n-bit integer mode is on).  If x is a complex number, the magnitude is taken first.  If x is an interval number, the midpoint is taken first.
  * `Q` converts x to a rational number.  The conversion precision is 1/10<sup>digits</sup> where digits is the current number of display digits.
  * `QQ` converts x to a rational number.  The conversion precision is 1/10<sup>digits</sup> where digits is the current floating point precision.
  * `R` converts x to a real number.
  * `C` converts x to a complex number.  If the imaginary part is zero, the number will become a real number.
  * `T` converts x to a date/time.
  * `V` converts x to an interval number.

`chop` converts x to a floating point number that is represented by
its current string representation.  Here's an example of the effect of
`chop`:

<pre>
> 2 pi *<br>
x:  6.283<br>
> show<br>
6.28318530717958623e0<br>
> chop<br>
x:  6.283<br>
> show<br>
6.28300000000000036e0<br>
</pre>

## Other unary functions ##

> <b><font color='green'><code>2V apart denom fp gamma invn ip ncdf numer</code></font></b>

`2V` converts y and x to the interval number [y, x].

`apart` takes a number apart.  For a rational, the numerator goes into
the y register and the denominator goes into the x register (put the
rational back together by executing `/`).  For a complex number, the
real part goes into the y register and the imaginary part goes into
the x register (put the complex number back together by executing `i * +`).
For an interval number, the least value goes into the y register
and the largest value goes into the x register (put the interval
number together by executing `2V`).  Note a date/time number that is
an interval number can also be taken apart.

`denom` and `numer` take the denominator and numerator, respectively,
of the rational number in x.

`ip` and `fp` take the integer part and fractional part, respectively,
of the real number in x.  If x is not a real number, it is converted
to one.

`ncdf` is the normal distribution CDF (cumulative distribution
function, the integral of the probability density function).
`invn` is the inverse normal distribution (the inverse of `ncdf`).

## Other binary functions ##

> <b><font color='green'><code>%ch = == != &lt; &gt; &lt;= &gt;= comb hypot perm round</code></font></b>

The `==` command compares the numbers in the x and y register and
returns True or False (which behave as 1 or 0, respectively).  The
comparison is exact for integers, and rationals.  For reals, complex
numbers and interval numbers, the comparison is at full precision.
The `!=`, `<`, `>`, `<=`, and `>=` commands work analogously.

The `=` command compares the _strings_ representing the numbers in the
current display configuration.  This command is primarily intended for
scripts and tests where you want to e.g. make sure that a calculation
came out just as it did before.

These comparison commands have special behavior when the -t command
line option is used.  If a comparison is False, the calculator
exits with status 1.  This is intended to be used e.g. by shell
scripts.  The calculator tests are in fact run this way.

Here's an example using interval numbers.  The two left-hand endpoints
of these numbers differ by 1 part in 10<sup>11</sup>.  We save them to two
registers so we can compare them with both `=` and `==`.  You can
see that the `=` comparison is True, but the `==` comparison is False.

<pre>
> [1,2]<br>
x:  1.500 (33.33%)<br>
> [1.00000000001,2]<br>
x:  1.500 (33.33%)<br>
> >a<br>
x:  1.500 (33.33%)<br>
> xch >b<br>
x:  1.500 (33.33%)<br>
> =<br>
x:  True<br>
> <a <b ==<br>
x:  False<br>
</pre>

`hypot` calculates sqrt(x`*`x + y`*`y).

`comb` calculates the binomial coefficient b(y, x) where x and y must
be integers.  This is also the number of ways of choosing x items at a
time from y total items.  `perm` is the number of permutations of x
items at a time taken from y total items.

`round` rounds y to the nearest x.  Examples:

<pre>
> 3 dig sig pi<br>
x:  3.14<br>
> 0.05 round<br>
x:  3.15<br>
> 3.12 0.05 round<br>
x:  3.10<br>
> 1234 300 round<br>
x:  1,200.<br>
</pre>


## Display ##

> <b><font color='green'><code>. bin brief comma dec digits eng engsi fix hex iva ivb ivc mixed</code></font></b>
> <b><font color='green'><code>oct off on polar prr prst rect sci show sig stack width</code></font></b>

The following commands are used to change the displays of integers:
`bin`, `dec`, `hex`, and `oct`.  All except decimal include a prefix
to unambiguously identify them.

These commands are used for real numbers and complex numbers:
`digits`, `eng`, `engsi`, `fix`, and `sci`.  `digits` controls how
many significant figures are displayed; however, for `fix` mode,
`digits` controls how many digits come after the decimal point.

Here's how the different display modes work.  For the number
12345.67890123456789 with comma decorating true (use `1 comma` and
with `10 dig` executed, this number will display as:

<pre>
12,345.67890 12346      fix<br>
12,345.67890            sig<br>
1.234567890e+4          sci<br>
12.34567890e+3          eng<br>
12.34567890 k           engsi<br>
</pre>

If you prefer the SI symbol without a space between it and the number,
there's a setting in mpformat.py that allows this (although this is
not correct SI practice).

If you have rationals turned on (`1 rat`), you can choose to display
them as improper or proper (mixed) fractions.  Use `1 mixed` to turn
on the mixed mode display.

`iva`, `ivb`, and `ivc` are used to change how interval numbers are
displayed.  Examples are, respectively, `1.500 +- 0.5000`,
`1.500 (33.33%)`, and `<1.000, 2.000>`.

Some calculations result in numbers with lots of digits.  For example,
the factorial of 10,000 will result in over 35,000 digits when
calculated exactly.  To make this all fit on one line, execute the
`brief` command.  This will shorten all number output so it will fit
in the given line width (set the line width with the `width` command).
The program will read the console's width from the COLUMNS environment
variable if it can.  Otherwise, the width defaults to 75 columns (you
can change this default in the cfg dictionary in hc.py).

The display of the stack can be turned on and off by the `on` and
`off` commands.  These are probably mostly of use in scripts that
you want to execute.

`prr` prints the contents of the storage registers and `prst` prints
the stack.  Because printing the stack is a common operation, you
can also use the `.` command for doing it.  If you want to see more
than one stack register displayed, use the `stack` command.

I should provide a warning to users of this calculator.  I've always
wanted calculators that would display the results of calculations in
a given number of significant figures.  This is because I'm usually
working with uncertain data and it's annoying to have to look at
10 or 12 digits when only two are significant.

Now that I have this capability of viewing just the significant
figures, I realize that because calculators have always displayed all
these digits in non-scientific and non-engineering display modes, when
people see e.g. `314,200,000.`, they might assume all those zeros are
significant (even though customarily they're not).  So I just want to
alert you to this possible interpretation.  If you're worried it might
happen to you, you can switch to using `fix` mode and see all those
digits.  Or, you can always use the `show` command to see the number
at full precision.

## Stack and register manipulations ##

> <b><font color='green'><code>clrg clst del roll xch</code></font></b>

`clrg` clears the registers; `clst` clears the stack.  `del` deletes
the x register.  `roll` rolls the stack so y becomes x and x goes to
the bottom of the stack.  `xch` exchanges x and y.
## Configuration ##

> <b><font color='green'><code>cfg deg down ec er es prec rad</code></font></b>

`cfg` shows the current configuration (not all settings, just the ones
that are probably of interest).  `deg` and `rad` set degree and radian
angle modes, respectively.  `1 down` turns downcasting on.
`ec`, `er`, `es` edit the configuration, registers, and stack,
respectively.  Here, editing means that they are represented in text
form and brought up in your editor.  When you finish, they are
re-evaluated by python and, if there were no errors, represent the new
values.  `prec` sets the number of digits of precision in real
numbers.  This defaults to 15 in mpmath, which is similar to the
typical IEEE floating point numbers on typical platforms.

## Constants ##

> <b><font color='green'><code>eps pi</code></font></b>

`eps` is a measure of the smallest increment possible in a real number
in the current precision.  If you get a small result and divide it by
eps and the magnitude of the result is on the order of one or less, it's
likely the original result was zero.

`pi` is the ratio of a circle's circumference to its diameter.

## Other ##

> <b><font color='green'> <code>! &gt;&gt;file &gt;&gt;. ? debug in lastx mid mod quit rand</code></font></b>

`!` by itself shows you scripts that can be executed.  ! with a script
name appended executes that script.  See the section on the ! Command
below.

`debug` is useful for debugging the calculator.  It changes a global
debug variable that, when True, will cause the file and line number to
be printed in exception messages.  This can save you tedious tracing
with the debugger.  Use `1 debug` to turn this debugging on.

`in` is used to test whether x is in the interval represented by the
interval number in y.  x can either be a number or an interval number.

`lastx` returns to x the last value used in a calculation.

`mid` turns an interval number into its midpoint.

`mod` turns on a Modulus function.  This feature causes every result
to be returned modulo the x value when `mod` was executed.

`quit` or `q` exit the calculator.

`rand` produces a uniformly distributed random number x such that
0 <= x < 1.

You can enter the ASCII value of a character as an integer by typing
the single quote character followed by the character.  This is handy
for bit manipulation calculations.  You can set the integers to 8
bits and display in binary mode to compare bits amongst the ASCII
characters.

## Command Line Options ##

  1. `-c` will print a message about any commands that don't have an associated help string.
  1. `-d` Use the default configuration only (internal to hc.py), not any persisted configuration.
  1. `-s` causes the calculator to read its input from stdin rather than prompting you; this is useful for scripts.
  1. `-t` is for testing mode:  when the `==` operator returns false, the program exits with status 1 (this also happens with the `in` command for intervals).
  1. -v` shows the program version.



<a href='Hidden comment: ------------------------------------------------------------'></a>
# Registers #

You can store the x register into a named register by using the
`>name` command.  `name` can be any set of characters except
whitespace.

Recall a named register to the x register with the command `<name`.

You can edit the named registers with the `er` command.  Change their
values or delete the variable's line to delete the variable.

Variables disappear when you exit the calculator unless you have
persistence turned on and a register file named in the `cfg`
dictionary in hc.py.


<a href='Hidden comment: ------------------------------------------------------------'></a>
# Customization #

## Adding a new command ##

One of the goals of the calculator was that it be easy to change to
adapt its behavior.  Adding new functions is perhaps the easiest thing
to do, assuming the code for the function is available.  Here's an
example: suppose we want to support the hyperbolic sine function.
mpmath already provides this function as sinh, so there's virtually no
work.

First, edit hc.py and go to the end of the file where the function
main() is.  In main, the `commands_dict` dictionary is defined.  This
dictionary maps commands to the functions that get called to implement
the command.  Search for the sin command and you'll find the following
line:

> `"sin" : [m.sin, 1, {"pre" : Conv2Rad}],`

The dictionary key is the string "sin", which is the command the user
types in.  The associated list contains three things.  First is the
function that gets called that actually implements the sine.  Here, it
is mpmath's sine function (mpmath was imported as m to save a bit of
typing).  Next is the number of arguments from the calculator's stack
that must be passed to the function.  The third argument, which is
optional, is a dictionary of other functions that might need to be
called before or after the main function is called.  The sine function
needs the `Conv2Rad` function called before the sine is called.  The
`Conv2Rad` function converts the x register to radians if the
calculator is in degree mode.  If you look at its implementation,
you'll see that it **doesn't** convert to radians if the x register is a
complex number.  You may or may not want this behavior, but I felt
that if I was working with complex numbers, I'd not want any such
conversions to take place.  However, when I work with real numbers and
integers, most of the time my work with angles is in degrees rather
than radians.

To add the `sinh` function, you'd just add a new line to the
dictionary:

> `"sinh" : [m.sinh, 1],`

In fact, if you go to do this, you'll find the hyperbolic functions
and other trig functions like the secant are already defined in the
dictionary but commented out (to keep the command count down).

The final step in adding a command is adding a line to the helpinfo.py
file.  This file defines the **helpinfo** dictionary that maps the
command to a help string.  When you type the command `?sinh`, you'll
see the associated help string printed.

## Adding a new command with a new function ##

A slightly more complicated case is when you also have to write the
function that implements the new command.  You can add the
implementation to the hc.py file or put it into another file and
import it into hc.py.  An example is the `Ncdf()` function which
calculates the CDF of the normal probability distribution.  mpmath
already defined the CDF, but it requires three arguments:  the
abscissa value, the mean, and the standard deviation.  The `Ncdf(x)`
function simply returns the value of the mpmath `ncdf(x, 0, 1)`
function (mean of 0 and standard deviation of 1).

When I'm fooling around with statistics, one table I use a lot is the
inverse normal CDF.  mpmath doesn't provide this function, but it was
pretty easy to add using the ncdf (normal CDF function) and mpmath's
ability to do root finding.  It took around 5 minutes to add the
command (as `invn`) and that includes figuring out how to use
`findroot`.

As mentioned elsewhere, you can also effectively add functionality
by using the ! command.  A future enhancement I plan to add is a
python script that will return the commonly used statistics CDFs
(Student's t, F, chi-square, etc.).  The script will prompt you for
the distribution, significance level, etc. and return the appropriate
statistic in the calculator's x register.  An added bonus of this
architecture is that the script can prompt for the needed information,
obviating the need for more commands or looking things up in a manual
somewhere.

## How the Code Works ##

The main loop of the calculator is in the `main()` function in hc.py.
You can look at it to see how the calculator works; the main logic is:

<pre>
while not finished:<br>
cmd_line = GetLineOfInput()<br>
cmds = ParseCommandInput(cmd_line)<br>
for cmd in cmds:<br>
status = ProcessCommand(cmd)<br>
if status == status_quit:<br>
finished = True<br>
</pre>

In turn, `ProcessCommand()` does the majority of the work.  Its logic
is

<pre>
ProcessSpecialCommand(cmd)<br>
if that wasn't a recognized command:<br>
if it was a number, push it onto the stack<br>
if it was a help request, show the help string<br>
otherwise, identify the command<br>
if it was a command, execute it<br>
if all ok and that was the last command in a string of commands<br>
display the stack<br>
</pre>

The `ProcessSpecialCommand()` function handles special functions or
function names that e.g. can have variable length, such as
`>>filename`, which turns logging on to a specified file.  It's just
basically one long switch statement.

If you're wondering what those questions marks are, they're bugs in
google's code that generates this web page (the code is inside a
`<pre>` HTML tag, but their parser interprets it anyway).



<a href='Hidden comment: ------------------------------------------------------------'></a>
# Logging and Executing #

`>>file` opens a log stream to a file.  `>>>file` does the same thing
but appends to the file if it already exists.

`<<file` executes commands from the file as if you had typed them in
at the keyboard.  This can be useful for e.g. defining some special
registers.

I found putting commands in a file useful when adding the ability to
use dates in the calculator.  I coded up the algorithms and checked
them against other algorithms I had written a number of years ago.
But one particular conversion from Julian day to a date and time gave
me a technically correct answer like `1Jan2009:21:59:59.999999...`
rather than the correct `1Jan2009:22:00` that I expected to see.  I
put the algorithm into a file and ran it with the calculator:

<pre>
# Meeus, algorithm on page 63<br>
off<br>
clst clrg<br>
2454871.4166666665<br>
0.5 + >jd<br>
ip >z<br>
<jd <z - >f<br>
<z 1867216.25 - 36524.25 / ip >alpha<br>
<z 1 + <alpha + <alpha 4 // - >a<br>
1524 + >b<br>
122.1 - 365.25 / ip >c<br>
365.25 * ip >d<br>
chs <b + 30.6001 / ip >e<br>
<b <d - 30.6001 <e * ip - <f + >day<br>
<day ip - 24 * >hr<br>
on<br>
prr<br>
</pre>

The intermediate results were stored in register names that coincided
with the variables that Meeus used, making checking easier.  When it
was run using `<<meeus` (meeus was the name of the file), I got the
following display

<pre>
> 10 dig fix<br>
Stack is empty<br>
> <<meeus<br>
a       2454884<br>
alpha   16<br>
b       2456408<br>
c       6724<br>
d       2455941<br>
day     8.91666 66665<br>
e       15<br>
f       0.91666 66665<br>
hr      21.99999 99963<br>
jd      2,454,871.91666 67200<br>
z       2454871<br>
<br>
x:  21.99999 99963<br>
</pre>

This let me check exactly what the calculator was calculating and
compare it to the results by hand calculation I had gotten.  This
example caused me to decide to round the time to the nearest tenth
of a second.

<a href='Hidden comment: ------------------------------------------------------------'></a>
# Environment #

In the cfg dictionary in hc.py, the `"environment"` key is used to
define a list of environment variables that are read and executed at
program startup.  These are handy for making sure some commands are
executed every time the program is started.

For example, I keep the following commands in the HCPYINIT variable:
`"off >>>d:/p/math/hcpy/.commandlog on"`.  Every time the calculator
starts, it logs the commands I input and the output.  Thus, I have
a record of what happened should I need it.  Each time the program
starts, it logs the time so you can separate sessions.

Note:  <font color='red'> This logging feature will create a mess if you<br>
use more than one invocation of the program logging to the same file<br>
</font>.  Thus, be sure to have only one invocation running at a time
to avoid simultaneous logging.  Or, start the program with the -d
option, which causes the environment variables to be ignored.
Finally, you could also use separate logging files by e.g. using a
different process ID number for each logging file name.

The environment is examined for the COLUMNS variable.  If it is
present, it is used to set the maximum line width for output to the
screen.  If it is not present, you can manually set it using the
`width` command.



<a href='Hidden comment: ------------------------------------------------------------'></a>
# Repeating commands #

If the stack has 20 elements on it and you want to roll it down 19
times to get the 20th element into the x register, it's a pain to have
to type `roll` 19 times.  You could put it into a file and execute the
file, but there's an easier way.  The syntax `n*x` is a shorthand for
executing the command `x` for n times.  This also works for putting a
bunch of the same numbers on the stack.  Example:  to put five values
of pi on the stack, execute `5*pi`.

Instead of counting, I would simply edit the stack and move the number
of interest to the bottom of the file (where the top of the stack is).


<a href='Hidden comment: ------------------------------------------------------------'></a>
# The ! Command #

The `!` command lets you add functionality to the calculator without
increasing the number of commands or changing the calculator's code.
It works by using python scripts in a specified directory.  When the
`!` command is used, it lists these script names without the `.py`
suffix.  When you type `!` followed by a script's name without the
suffix, that script is called and used to return a number, which is
put into the calculator's x register (it could also return a list of
numbers).  There are two example scripts included with the program,
cons.py and h.py, which I use.  cons.py prompts you to choose a
physical constant and returns your choice in SI units; it's an
interval number if it's not exact.  The h.py script lets me interface
with a help system using the vim editor I've had for a number of years
on my computer.  I can view one of the help files and use the mouse to
copy a number to the clipboard.  When I quit the help system, I'm
prompted for a number, which then gets returned into the x register.
This lets me keep a lot of information handy without burdening the
calculator with more commands and code.

A future revision will include the ability to extract information from
running other programs.  For example, I use the GNU units program a
lot.  A python script will invoke units, you get the conversion you
want, then the python script will extract the last number and return
it in the calculator's x register.




<a href='Hidden comment: ------------------------------------------------------------'></a>
# Examples #

## Uncertainty propagation ##

You've measured the hypotenuse of a triangle as 27.23 m and you
estimate the uncertainty is 0.01 m.  An associated angle of the
triangle is measured at 37 degrees and you estimate the error is +/- 1
degree.  What is the estimated uncertainty in the length of the base
of this triangle, calculated as `27.23*cos(37)`?  This calls for the
use of interval calculations:

<pre>
> 27.23(0.01)<br>
x:  27.23 +- 0.01000<br>
> 37(1)<br>
x:  37.00 +- 1.000<br>
> cos<br>
x:  0.7985 +- 0.01050<br>
> *<br>
x:  21.74 +- 0.2940<br>
</pre>

Realize that such calculations, while mathematically bounding the
error (which is the purpose of interval calculations), usually
overestimate the error in physical predictions.  It makes more sense
to probabilistically propagate the error rather than use the
worst-case estimates of interval arithmetic.

Another use of interval arithmetic is to help estimate round-off
error.  If you perform your calculations with interval numbers, the
growth in the width of the interval gives you a clue to the effects of
round off.  You may also want to combine this work with increasing the
precision, as mpmath's support for interval numbers with all of its
functions isn't complete yet.

## Complex arithmetic ##
What is the square root of i<sup>i</sup>?

<pre>
> i i pow sqrt<br>
x:  0.4559<br>
</pre>

Do these two complex numbers have the same magnitudes?
`36.73-98.11i` and `-73.84+74.30i`?

<pre>
> 36.73-98.11i<br>
x:  36.73 - 98.11i<br>
> abs<br>
x:  104.8<br>
> -73.84+74.30i<br>
x:  -73.84 + 74.30i<br>
> abs<br>
x:  104.8<br>
> =<br>
x:  True<br>
</pre>

This was pretty obvious, as we can see by inspection they were equal.
But if we use the `==` comparison, we find they are not equal, because
they differ in the fifth decimal place:

<pre>
> 36.73-98.11i abs<br>
x:  104.8<br>
> -73.84+74.30i abs<br>
x:  104.8<br>
> ==<br>
x:  False<br>
</pre>

For comparing numbers that are being displayed to a few significant
digits, the `=` and `==` commands aren't really needed.  They do prove
useful, however, for comparing numbers when many digits are shown on
the screen (especially when `brief` is on).  or when you want to test
from within a script.

## Rational approximations ##

Converting a real number to a rational number using the `Q` command
generates a rational approximation.  The precision of the
approximation is controlled by the number of digits in the display
(i.e., 10^(-digits)).  For a conversion to full precision, use the
`QQ` command.

For example, setting the display to 3 significant figures using
`3 dig`, we can get a rational approximation for pi:

<pre>
> 3 dig pi Q<br>
x:  22/7<br>
> 4 dig pi Q<br>
x:  355/113<br>
> pi QQ<br>
x:  80143857/25510582<br>
</pre>

`355/113` is quite a good approximation for pi:

<pre>
x:  355/113<br>
> R pi / 1 -<br>
x:  0.00000 00849 1<br>
</pre>

## Viewing the configuration ##

The `cfg` command shows the current configuration:

<pre>
> cfg<br>
Configuration:<br>
line width = 75, mixed fractions = True<br>
Downcasting is off<br>
deg dec rect   stack_display:1<br>
Display = sig, 4 dig, prec = 15, brief = False<br>
</pre>

This shows that we're in degree mode, integers are displayed in
decimal, and complex numbers are shown in rectangular form.  4
significant figures are displayed, the calculation precision is 15
digits, and large numbers won't be collapsed into a compact form that
fits on one line.

One of the problems with console programs like this one is that
commands proliferate as new functionality is added.  One of the
reasons this configuration editing is a feature is that it can
potentially reduce the number of commands.  However, this also makes
the assumption that interactive use is the way the program will be
used -- removing commands then affects people who want to e.g. run
commands from files.  A way around this is to offer a method to select
features at runtime.  This probably won't be conceptually difficult to
do, but there will be a fair bit of refactoring because it's a
significant change to the architecture.  In other words, I'd like to
have it, but other things come first.

## Modulus calculations ##

The `mod` command stores the x register and calculates the remainder of
every result divided by this number (use `1 mod` to turn it off).  This
can be handy for modular arithmetic.  If the x register and the
modulus are both integers, then integer division is used and you'll
see the integer remainder.  Otherwise, floating point arithmetic is
used.

We use this to explore whether 11549 is prime.  Fermat's little
theorem says that if p is prime, then x<sup>p</sup> = x (mod p).

<pre>
> 11549 mod<br>
Stack is empty<br>
(mod 11549)<br>
> 11549 >a<br>
x:  11549<br>
(mod 11549)<br>
> 39 <a pow<br>
x:  39<br>
(mod 11549)<br>
> 87 <a pow<br>
x:  87<br>
(mod 11549)<br>
> 123 <a pow<br>
x:  123<br>
(mod 11549)<br>
> q<br>
</pre>

Because the remainder each time was the value of the input x, we can
strongly suspect that 11549 is prime (it is in fact a prime number).
The `(mod 11549)` is shown every time to remind you that modular
arithmetic is on.

The `brief` command is useful if you're not doing modular arithmetic
to avoid seeing lots of digits.  `brief` toggles the display between
showing all the digits and just those digits that fit on one line.
Here's the brief display of 39<sup>11549</sup>:

<pre>
> 39 11549 pow<br>
x: 160378648372188857097067960937320...255155627398518975845038364356359<br>
</pre>

Because python's integers can be of arbitrary length, these integer
calculations are exact (and, by extension, so are the rationals).

## Pathological and Big Numbers ##

The configuration file has a key named `allow_divide_by_zero` that,
when true, will let you divide by zero as long as the numerator isn't
also zero.  The result will be `-inf` or `+inf` depending on the sign of
the numerator.

Some operations will result in `NaN` (not a number), such as `inf sin`.

Operations with `+inf` or `-inf` will make sense where they can; at other
times, they will result in `NaN`.

Since python integers can be of arbitrary size, you can easily cause
calculations that will take a long time, such as raising an integer to
a big power.  I've only used hcpy under cygwin on Windows; I've found
that a `ctrl-C` is needed to stop these long calculations.
Unfortunately, this leads to an immediate exit from the program and
the operands will be lost and current settings will not persist.  If
someone knows of a platform-independent way of interrupting processing
in python, please submit an enhancement request on the issues tab.



<a href='Hidden comment: ------------------------------------------------------------'></a>
# History #

This implementation is the third I've made in the last 15 years.  The
first version started out as a hex calculator for helping with
determining the results of integer calculations in C on a particular
platform; hence the name `hc`.  The next version was in C++ and added
floating point capabilities.  That effort taught me that the details
of different C++ implementations can make it frustrating to write
platform-independent code.  I used that second version essentially
every day since; that experience guided me in what to add and what to
throw out for the third version.  Since I kept the name `hc`, I now let
it abbreviate "handy calculator".

I chose to implement this version in python, which gives me assurance
that it will run portably on any platform I'm likely to use.

An advantage of python is that it makes it easy to add or change
functionality.  If you have a python function that takes one or two
arguments and returns a result, you can add it as a new command to the
calculator in about a minute.