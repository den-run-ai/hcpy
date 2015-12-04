

This page contains a casual introduction to the
calculator with the intent of demonstrating some
of the common things that can be done with it.
I'll start off at an elementary level, then look
at some of the fancier things that can be done.

Alas, when showing verbatim input and output in
google's wiki, the code sections get
color-formatted and there doesn't appear to be any
way to turn it off.  If you know of a way, please
send me an email at someonesdad1@gmail.com.

# Typical Usage #

Here's a typical use of the calculator -- it
demonstrates the type of thing I do with the
calculator in day-to-day use.  I wanted to know
the mean density of the earth.  I had the earth's
radius and mass.  I started the program (I use the
name `hc`).  The `>` is the program's prompt for
user input.  The `#` character denotes the start
of a comment to the end of the line.  The volume
of a sphere is 4/3 times pi times the radius
cubed.  The first line calculates the volume of
the earth in cubic meters.  The `>volume` saves
the value in a register named `volume`.  The mass
of the earth is entered, then the `xch` command
changes the position of the mass and volume on the
stack.  The `/` command performs the division and
the result is shown in kg per cubic meters.
Dividing by 1000 yields g/cc.  The mean density of
the earth is 5.5 times the density of water.

<pre>
--> hc<br>
> # Mean earth density, r = 6378 km, m = 5.98e24 kg<br>
> 4 3 / pi * 6378e3 3 pow *<br>
x:  1.087e+21<br>
> >volume<br>
x:  1.087e+21<br>
> 5.98e24 xch /<br>
x:  5502.<br>
> # kg/m3<br>
> # Convert to g/cc:  multiply by 1/1000<br>
> 1000 /<br>
x:  5.502<br>
> q<br>
--><br>
</pre>

Truthfully, I would never type in the above
comments in my usual work (unless I had turned on
logging to a file and wanted to document what I
was doing).  Here's the way the calculation would
actually look:

<pre>
> 4 3 / pi * 6378e3 3 pow *<br>
x:  1.087e+21<br>
> 5.98e24 xch /<br>
x:  5502.<br>
> 1000 /<br>
x:  5.502<br>
</pre>

I would have looked at the first line and mentally
calculated that the result was about 1e20 and
dividing into 1e24 would give about 1000.  Thus,
I'd know the answer of about 5000 was not too far
off the true number.  This habit hails from the
day before calculators when calculations were more
laborious and prone to errors, but it's still a
good habit today -- mainly because it's easy to
make a mistake typing the numbers in.

# Start of Tutorial #

I'll assume that you've installed python, mpmath,
and the hcpy program's code.  You've run the
command `python hc.py` and now you see the
program's prompt:

`>`

## Getting help on commands ##

First, type in the `?` command to see a list of
the commands that are available.  This list may be
a bit intimidating at first, but you only need to
learn the commands that you'll use a lot.  Here's
what the output looks like on my system at the
time of writing, but this will change over time as
the program is enhanced.  You can change the
commands that the program executes and their names
if these aren't to your tastes.

<pre>
List of all commands:<br>
! % %ch & * + - . / // 1/x 2deg 2hms 2hr 2rad << == >> >>. ? C I Q QQ R<br>
V ^ abs acos and apart arg asin atan atan2 bin brief ceil cfg chop chs<br>
clrg clst comb conj cos debug dec deg del denom digits down ec eng<br>
engsi enter eps er es exp fact fix floor fp gamma hex hypot im in int<br>
inv ip iva ivb ivc lastx ln ln2 log mid mixed mod ncdf none numer oct<br>
off on or perm phi pi polar pow prec prr prst quit rad rand re rect<br>
roll sci show sig sin sqrt square stack tan uint width xch xor | ~<br>
</pre>

You can get help on any command by typing `?`
followed by the command's name with no space after
the `?`.  Here's the help on the `atan2' command:

<pre>
> ?atan2<br>
Arc tangent of y/x; returns a number -pi < x <= pi.  Gets the quadrant<br>
correct.<br>
</pre>

You can shorten a command as long as it isn't
ambiguous.  If it is ambiguous, the possible
commands are printed out.

# The Stack #

An RPN calculator is based on the stack.  I won't
try to give a tutorial on using an RPN calculator;
if you need one, I suggest
[http://www.hpmuseum.org/rpn.htm
http://www.hpmuseum.org/rpn.htm].

The stack holds numbers and is modelled after a
typical HP RPN calculator.  The first four stack
registers are x, y, z, and t, which is how HP
first named the registers.  If you prefer to have
them numbered, it's easy to modify the
Stack._string() method in stack.py._

After the calculator finishes executing a line of
commands you've typed in, the stack is displayed.
By default, one element is shown (the x register),
but you can set how many registers are shown by
using the `stack` command.  Use `0 stack` to have
all the registers printed after each line is
executed.

The `clst` command deletes the elements of the
stack.

# Arithmetic #

The four basic arithmetic operations are +, -,
`*`, and /.  To use an RPN calculator, you first
enter the two numbers, then you enter the
operation you want to do on them.  Thus, to
multiply 2 times pi, we enter:

<pre>
> 2 pi *<br>
x:  6.283<br>
</pre>

The number entries are separated by spaces -- this
is how the calculator tells them apart.  You can
also enter the calculation as one item per line:

<pre>
> 2<br>
x:  2<br>
> pi<br>
x:  3.142<br>
> *<br>
x:  6.283<br>
</pre>

The only difference is that the calculator shows
you the top register of the stack after you press
the return key (some people and HP call it the
bottom of the stack), the x register.  You can use
the `stack` command to have the calculator show
you more registers of the stack when it is
printed.  Enter the number of registers you want
to see and then `stack`.  Enter `0` to have all
the registers printed.

# Numbers #

## Integers ##

You have some choices on how you want to display
integers.  The `dec` command displays them as we
are most used to them in decimal format.  You can
also choose to view them in base 16 (hexadecimal)
mode (`hex`), base 8 (octal) mode (`oct`), or base
2 (binary) mode (`bin`).

Sometimes when e.g. multiplying integers or
calculating powers of them, you get large numbers.
You can use the `brief` command to force the
calculator to show you the result on one line.  To
do this, it chops the middle out of the number and
just shows the right and left ends.  This works
for any type of number so that each stack line
fits on one display line.

Here's an example.  100! (`100*99*98...*1`) is a
pretty big number.  It won't fit on a single 80
column line.  But by first executing the `brief`
command, it will:

<pre>
> brief<br>
Stack is empty<br>
> 100 fact<br>
x:<br>
933262154439441526816992388562667...210916864000000000000000000000000<br>
> 10000 fact<br>
x:<br>
284625968091705451890641321211986...000000000000000000000000000000000<br>
</pre>

You'll note we also calculated 10000!, which a
much larger number.  Yet it also fits on one line.
If you execute `brief` again, you'll see all
35000+ digits of this number printed out.  Now you
see why the `brief` command is useful!

> The cfg dictionary has a key
> `"factorial_limit"` that, if not zero, is used
> to change when the factorial of large numbers
> is calculated via the gamma function versus
> exact integer arithmetic.  I leave mine set to
> around 20000, as it takes about 5 seconds to
> calculate and display the 77000 digits of the
> result.

## Rational Numbers ##

Arithmetic should be pretty straightforward.
However, one thing that might surprise you is when
you divide two integers -- you'll get a rational
number.  Here's an example of dividing 3 by 12:

<pre>
> 3 12 /<br>
x:  1/4<br>
</pre>

The division of two integers will always result in
a rational number unless you have downcasting
turned on (see the help on the down command using
`?down`).  The rational number is automatically
reduced to lowest terms.  You can switch between
showing the fraction as a mixed fraction and an
improper fraction by using the `mix` command.

With a rational number in the x register, you can
use `numer` to take its numerator and `denom` to
take its denominator.  `apart` puts the numerator
in the y register and the denominator in the x
register.

## Real Numbers ##

Most of the time we probably deal with real
numbers.  In the calculator, these are numbers
with a decimal point or an exponent.  Since pi is
so commonly used, it is a real number with its own
special name.  You can add more such constants by
changing the code (or put them into registers as
we'll see later).

You can choose what precision you want to
calculate real numbers with.  The default is 15
digits, which is more than enough for the majority
of calculations.  Change the precision by the
`prec` command.  First, enter the number of digits
of precision you want, then execute `prec`.
Changing the precision is useful to see if a
calculation is being affected by rounding off,
which always happens with floating point
calculations.

You can also change the number of digits shown on
the display by using the `digits` command.  The
calculations, of course, are done to the precision
you've set and rounded to the number of digits you
desire.  You also can choose between different
types of real number displays: `sig`, `fix`,
`sci`, `eng`, and `engsi`.  Suppose we calculated
the time constant of an RC filter and got 26.369
microseconds.  Here are the different real number
displays with digits set to 4 (the first is in
`sig` mode, which is what I use virtually all the
time):

<pre>

> 26.369e-6<br>
x:  0.00002637<br>
> fix<br>
x:  0.0000<br>
> sci<br>
x:  2.637e-5<br>
> eng<br>
x:  26.37e-6<br>
> engsi<br>
x:  26.37 u<br>
</pre>

Note that `fix` mode displays the number as zero.
This is because fix mode means to display 4 digits
after the decimal point and this number is zero to
that resolution.

If you're not sure whether it's zero or not, you
can always use the `show` command to see the
number expressed to the current precision:

<pre>
> show<br>
prec = 15 digits<br>
2.63690000000000011e-5<br>
x:  0.0000<br>
</pre>

The `brief` command also causes long real numbers
to fit on one line.

A small shortcut is that powers of 10 may be
entered by just typing in `e` and the exponent:

<pre>
> e14<br>
x:  1.000e+14<br>
</pre>

With a real number displayed in the x register,
the `chop` command replaces the number with the
real number exactly as the original number was
displayed.

<pre>
> pi<br>
x:  3.142<br>
> show<br>
prec = 15 digits<br>
3.14159265358979311e0<br>
x:  3.142<br>
> chop<br>
x:  3.142<br>
> show<br>
prec = 15 digits<br>
3.14199999999999990e0<br>
x:  3.142<br>
> 8 dig<br>
x:  3.1420000<br>
</pre>

## Complex Numbers ##

Complex numbers are entered as two real numbers
with the second number having an `i` or `j` prefix
or suffix.  The real and imaginary parts are
separated by the sign of the imaginary part.  Here
are some examples:

<pre>
> 3+j<br>
x:  3.000 + 1.000i<br>
> -3.14-i17<br>
x:  -3.140 - 17.00i<br>
> i<br>
x:  i<br>
> -18j<br>
x:  -18.00i<br>
</pre>

The cfg dictionary in hc.py has some entries that
let you control how complex numbers are displayed.
Read the comments and set things up how you wish.

The `re` command takes the real part of a complex
number and `im` takes the imaginary part.  `arg`
calculates the argument in the current angle
measure (radians or degrees).  Use `deg` or `rad`
to set the angular mode.  `conj` calculates the
conjugate.

`rect` displays complex numbers in rectangular
form and `polar` shows them in polar form.

<pre>
> -1-i<br>
x:  -1.000 - 1.000i<br>
> polar<br>
x:  1.414 <| -135.0 deg<br>
> sqrt<br>
x:  1.189 <| -67.50 deg<br>
> i *<br>
x:  1.189 <| 22.50 deg<br>
> arg<br>
x:  22.50<br>
</pre>

## Interval Numbers ##

[Interval numbers](http://en.wikipedia.org/wiki/Interval_arithmetic)
are useful when you perform calculations with
uncertain numbers.  The calculation's result will
be an interval number expressing the interval
within which the answer lies.  Here's an example
of using interval numbers.

I'm examining a satellite photograph of a
building.  Based on the fuzziness of the cars, I
estimate that length and width measurements are
uncertain to 1 m.  Estimating the height of the
building is harder because I'd have to know the
time of day and location to calculate the height
from the building's shadow.  I estimate the height
uncertainty at 5 m.  From the photograph and other
data, I get the dimensions of the building as
height = 25 m, width = 70 m, and length = 100 m.
What is the range of the estimated volume?  Here's
the calculation:

<pre>
> 25+-5<br>
x:  25.00 +- 5.000<br>
> 70+-1<br>
x:  70.00 +- 1.000<br>
> 100+-1<br>
x:  100.0 +- 1.000<br>
> * *<br>
x:  175900. +- 39260.<br>
> ivb<br>
x:  175900. (22.32%)<br>
</pre>

The `ivb` command changes the interval display to
showing the interval half-width as a percentage of
the interval midpoint.

## n-bit Integers ##

Programmers sometimes have to work with
fixed-width integers.  For example, working with
integers in C often means calculating results with
32 or 64 bit integers.  hcpy can simulate these
integers, both signed and unsigned.  The `int`
command followed by an integer n specifies signed
n-bit integers; uint followed by an integer n
specifies unsigned n-bit integers.  You can do the
usual arithmetic and bit manipulations with them.
The following example shows some typical
calculations.  <font color='red'>(The display is<br>
messed up because google's wiki still doesn't<br>
display <code>&lt;pre&gt;</code> tags correctly.) </font>

<pre>
> int16<br>
Stack is empty<br>
> 42<br>
x:  42<s16><br>
> 21<br>
x:  21<s16><br>
> *<br>
x:  882<s16><br>
> hex<br>
x:  0x0372<s16><br>
> bin<br>
x:  0b0000001101110010<s16><br>
> oct<br>
x:  0o001562<s16><br>
> bin<br>
x:  0b0000001101110010<s16><br>
> 'z<br>
x:  0b0000000001111010<s16><br>
> and<br>
x:  0b0000000001110010<s16><br>
> 'r<br>
x:  0b0000000001110010<s16><br>
> xor<br>
x:  0b0000000000000000<s16><br>
> 'r<br>
x:  0b0000000001110010<s16><br>
> xor<br>
x:  0b0000000001110010<s16><br>
> -1<br>
x:  -0b0000000000000001<s16><br>
> uint16<br>
x:  0b1111111111111111<u16><br>
>    0b0101010101010101<br>
x:  0b0101010101010101<u16><br>
> xor<br>
x:  0b1010101010101010<u16><br>
> int<br>
x:  0b1010101010101010<br>
> int<br>
UserWarning: Warning:  changing signed to True<br>
warn("Warning:  changing signed to True")<br>
x:  0b1010101010101010<br>
</pre>

Some comments on what you see above:

  1. You can enter numbers in decimal, hex, octal or binary.  For the last three forms, use `0x`, `0o`, and `0b` prefixes, respectively.
  1. int16 represents signed 16 bit numbers.  This is indicated in the display by e.g. `42<s16>`.  Note the switch to unsigned 16 bit integers near the bottom.
  1. Note signed integers will have a sign displayed with them when they are negative, even in hex or binary.  Unsigned integers won't.
  1. You can enter the ASCII value of a character by prefixing the character with `'`, the apostrophe character.
  1. You can change the size of the n-bit integers at any time.  If you downsize, the lower-order bits will be saved.  If you upsize, the added bits will be zero except if the number represented is -1, in which case it will remain -1.
  1. int by itself changes the calculator back to normal arbitrary-size python integer arithmetic.  Note the warning message, which occurs when int is executed when unsigned n-bit integer mode is on.  This warning is given because it is impossible to represent unsigned arbitrary-size integers in python.  The `<u16>` suffix dropped from the displayed number in the last step.
  1. There are subtleties to using signed n-bit integers.  Execute `?int` to read about them.

## Changing a number's type ##

You can force a conversion from any type to any
other type:

|Command| Converts to|
|:------|:-----------|
|I      |integer     |
|Q      |rational (current displayed number of significant figures) |
|QQ     |rational (full precision) |
|R      |real        |
|C      |complex     |
|V      |interval    |

If n-bit numbers are active, an integer
conversions will be made to the current number of
bits.

Information can be lost when making conversions.
When converting a complex to a real, the
conversion method is to take the magnitude
(similarly when converting a complex to a
rational, integer, or interval number).  If you'd
prefer different behaviors, the code is easy to
change in the convert.py file.

Here are some example conversions:

<pre>
> pi 10 *<br>
x:  31.42<br>
> Q<br>
x:  31 5/12<br>
> pi 10 *<br>
x:  31.42<br>
> QQ<br>
x:  31 2265603/5447123<br>
> V<br>
x:  31.42 (0.000%)<br>
> 1(.01%) *<br>
x:  31.42 (0.01000%)<br>
> C<br>
x:  31.42<br>
> i *<br>
x:  31.42i<br>
> R<br>
x:  31.42<br>
> I<br>
x:  31<br>
</pre>

# Elementary Functions #

Numerous unary and binary functions are provided.
More are defined in the calculator's code, but
commented out to keep the command count down.
They're easily added by uncommenting them.

Most of the functions will be familiar, such as
trigonometric, logarithmic, exponential, and power
functions.  They all work over the complex domain
where appropriate.  If you do lots of work with
complex functions, you might want to consult the
mpmath documentation to know where the branch cuts
are.

## Trigonometric Functions ##

The sine, cosine, and tangent functions are
provided, along with their inverses.  Auxiliary
trig functions such as the secant and hyperbolic
functions are defined in the hc.py file, but are
commented out.

Here's an example of iteration to find the value
of x where cos(x) and x in radians are equal.

<pre>
> rad<br>
Stack is empty<br>
> 0<br>
x:  0<br>
> cos<br>
x:  1.000<br>
> cos<br>
x:  0.5403<br>
> cos<br>
x:  0.8576<br>
> 20*cos<br>
x:  0.7391<br>
> cos<br>
x:  0.7391<br>
</pre>

Note the `20*cos` command.  This is a shorthand to
tell the calculator to repeat the command after
the asterisk 20 times.  It works with any command
or number entry.  For example, you could enter the
complex number `-1-14i` ten times onto the stack
by the command `10*-1-14i`.  Programmatically,
this was easy to do because python supports a
similar type of syntax for sequences.

## Logarithmic and Exponential Functions ##

e<sup>x</sup> is given by the `exp` function.  Thus, we get
the value of e:

<pre>
> 1 exp<br>
x:  2.718<br>
</pre>

To raise any other number to a power, use the
`pow` function.

<pre>
> i i pow<br>
x:  0.2079<br>
</pre>

If this surprises you, realize that i<sup>i</sup> is
e<sup>i(ln(i))</sup> = exp(-pi/2).

`ln` is the natural logarithm; `log` is the base
10 logarithm.  The base 2 logarithm is `ln2`.

How many binary bits are needed to represent
4294967297 different states?

<pre>
> 4294967297 ln2<br>
x:  32.00<br>
> ceil<br>
x:  33.00<br>
</pre>

I deliberately chose this number as 1 more than
2<sup>32</sup> to illustrate a point.  When dealing with
floating point numbers, you sometimes need to see
the full precision (thus, a `show` command would
have been useful here, as it would have given
`3.20000000003359019e1`).  Since I knew the answer
was just slightly over 32, I used the `ceil`
command, which returns the smallest integer
greater than or equal to the x register.  Thus, 33
bits are required, not 32 as it first appeared.

## The Factorial Function ##

`fact` returns the factorial of the number in the
x register.  This will be an integer when x is an
integer (and it is less than the `factorial_limit`
key in the `cfg` dictionary) and a floating point
number otherwise.  Note that the argument is
allowed to be any complex number.  This is because
mpmath actually computes the gamma function for
real or complex arguments and also uses it for
factorials of the same arguments.

If `cfg["factorial_limit"]` is zero, the exact
integer factorial will be calculated for integer
arguments.  This can lead to excessive computation
times and the output of large numbers for
arguments in excess of around 10,000 (depends on
the speed of your computer).  Example:  the
factorial of 1e5 has around half a million digits.
Just printing this number in my 60-line terminal
window takes about 15 seconds on my computer.

## Degrees and Hours ##

Time in hours, minutes, and seconds (and degree
measure in degrees, minutes, and seconds) can be
converted to decimal form.  The form to enter is
h.mmssss where h represents the hours, mm the
minutes from 0 to 59, and ssss, the seconds with
an implied decimal point after the second s.  Use
`2hr` to convert to decimal hours.  If you start
with decimal hours, use `2hms` to convert to
hours, minutes, and seconds.

`2deg` converts radians to degrees and `2rad`
converts degrees to radians.

# Other Functions #

mpmath has lots of other functions used in math
and physics.  The univariate and bivariate ones
can be used directly by the calculator if you add
them to the `commands_dict` dictionary in hc.py
(at the end of the file).  Others might need for
you to write a python wrapper function for them.
With mpmath's ability to do numerical calculus and
built-in functions, you can add a huge number of
special functions to the calculator should you
need to.

# Testing Equality #

The `==` command is useful for testing when things
are equal.  It works exactly for integers and
rationals and returns True or False (which behave
like the integers 1 and 0).  For real and complex
numbers, the numbers are considered equal **if
their string representations in the current
display mode are equal**.  For exact comparisons,
make the display number of digits equal to the
precision.

This functionality is really intended to be used
for testing the calculator in combination with the
`-s` and `-t` command line options.  The `-s`
option tells the calculator to read its input from
stdin.  You perform a series of operations, then
put the correct answer into the x register.  When
you execute the `==` function and the `-t` option
is active, the program will exit with status 1 if
the comparison is unequal.  This lets you run
testing scripts and find out whether they passed
or failed by examining the return status.

Example:  when `echo "1 2 ==" | z -s -t` was
executed in a bash shell, the return status was 1.