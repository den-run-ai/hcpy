hcpy is a yet another RPN calculator.  There have been many of these
written over the years, so why another one?  I couldn't find one with
the features I wanted:

  * Simple console-based interface, no mouse or GUI needed
  * Basic scientific calculator functionality similar to HP calculators
  * Perform precise calculations when needed
  * Easy to extend
  * Flexibility in formatted output



# Updates #

**19 Jul 2014**:  The Downloads button has been disabled.  Use the Source button above
to get the Mercurial repository containing the code.

**22 Jun 2014**:  I've moved the project's source code files to the
Mercurial repository and **deprecated the zipfile under the Downloads
tab above**.  To get the project's files, install Mercurial on your
system, click on the Source link above,  and execute the `hg` command
there.  If you're intimidated by revision control systems like
Mercurial, git, etc., see the bottom of this page for a quick
description of using them.  The March 2009 zipfile revision
(hcpy\_version6.zip) has the tag `17Mar2009` in the Mercurial
repository (the files 0readme, project\_home\_page, wiki\_documentation,
and wiki\_tutorial are new).

<font color='red'>Warning:  hcpy does not work with recent versions of<br>
mpmath.  </font> If you want to use hcpy, you'll have to download the
0.12 version of mpmath.  I recently moved to a new computing platform,
so I'll be updating and fixing hcpy to work again, but it will be a
while before I have the time to work on it.

# Examples of use #

I use this calculator for quick answers to arithmetic problems.  Here
are some examples that show some features:

<pre>
python hc.py<br>
> 1 2 +<br>
x:  3<br>
> deg 17.7 tan 23.45M *<br>
x:  7,484,000.<br>
> engsi<br>
x:  7.484 M<br>
> sig 1Jan2009 10 +<br>
x:  11Jan2009:00:00<br>
> now<br>
x:  9Feb2009:08:57:33.0<br>
> pi Q<br>
x:  3 16/113<br>
> mix<br>
x:  355/113<br>
> 1/2 14.134725i + zeta<br>
x:  0.00000 00176 7 - 0.00000 01110i<br>
> 60 width brief 10000 fac<br>
x:  284625968091705451890641...00000000000000000000000000<br>
> R<br>
x:  2.846e+35659<br>
> 100 prec 100 dig i i pow R<br>
x:  0.20787 95763 50761 90854...13 97886 00277 86542 60353<br>
</pre>

The program is run by running the hc.py file.  The `>` is the
calculator's prompt.  The first example shows simple addition.  The
input is parsed on whitespace.

The second example sets the calculator's angle mode to degrees, takes
the tangent of 17.7 degrees, then multiplies it by 23.45x10<sup>6</sup>; note
that SI prefixes are allowed as substitutes for exponents.  The
output demonstrates that 4 significant figures are being used and that
comma delimiting is turned on.  The `engsi` command shows switching
the display to show numbers in engineering mode with SI abbreviations
for magnitude.  The `sig` on the next line switches back to
significant figure display.

The next example shows the date/time that is 10 days after 1Jan2009.
Dates/times are just real numbers (astronomical Julian day numbers)
behind the scenes and arithmetic with the other types of numbers can
be done.  The calculator knows the dates `now` and `today`.

`pi Q` converts the constant pi to a rational number to the current
display precision (the `QQ` command converts them to the current full
real number precision).  Rationals can be displayed as either proper
or improper fractions; the display is toggled by the `mixed` command.
The use of `mix` demonstrates that the calculator's commands can be
shortened as long as the abbreviation is unique.  Since rationals use
python's arbitrary-size integers, rational arithmetic is exact.

The next example is a little more complex (pun intended).  It shows
the evaluation of the Riemann zeta function at its first nontrivial
zero.  The decimal fraction output is spaced at every 5 digits (also
part of the comma delimiting feature).

The next example changes the width of the output screen and turns on
brief mode, which forces the output to fit on one line.  The factorial
of 10,000 is calculated exactly.  The R command converts it to a real
number, which shows it has more than 35,000 digits.

The last example shows switching to 100 digits of real number
precision and displaying 100 digits.  The number displayed is i<sup>i</sup>
(i.e., e<sup>-pi/2</sup>), but shortened to fit in the 60 column screen width.

Head to this [page](WikiDocumentation.md) for the documentation.  A
[tutorial](Tutorial.md) is here.

# Comment on RPN Calculators #

It might be prudent to explain a little about RPN calculators, as they
are not as common as they once were.  I won't go into the details of
how to use a typical calculator such as an HP calculator, as they are
covered nicely [here](http://www.hpmuseum.org/rpn.htm).  For
information on the variety of HP calculators made, go
[here](http://www.hpmuseum.org).

In 1972 when the first "pocket calculator" came out, the HP-35, it
created a sensation.  It let you do mind-numbingly boring and
error-prone calculations faster and more accurately than you could
do them by hand.  In those early years of pocket-sized calculators,
the HP models captured a large share of the calculator market. Other
models started to come out (notably from TI) that used "algebraic"
entry and divided calculator users into two groups.

Some people (like me) became accustomed to using RPN calculators for
the usual calculations calculators are used for.  I still "think" in
RPN when evaluating formulas, as it maps nicely to the way we had to
manually work out formulas before calculators became available.

Today, there's little reason to learn RPN because one can write
formulas directly in typical infix programming languages like C or
python.  And algebraic entry calculators are powerful enough and
inexpensive enough to let users enter their formulas directly.  Most
users would understandably see no need for an RPN calculator.  Yet I
still have and use two of my favorite HP calculators from the 70's and
80's:  the HP-42s (hands-down the best calculator I've ever used) and
the HP-41.  Around 1987 I gave my HP-41 to my nephew, who was in
college and he used it through college.  At Christmas 2013, he found
it in his stuff and gave it back to me -- it still works fine and the
magnetic card reader can still read the programs I wrote for it over
30 years ago.

# Using revision control tools #

Revision control tools like Subversion, Mercurial, and git can be
intimidating the first time you come across them, as they have lots of
commands and it's not obvious what they do and how they do it.  But
you can start off with little knowledge and at least be able to clone
repositories, which means you create a copy of a project's source code
files on your local computer.

> A number of years ago I was working with some other folks around the
> country and I needed to pick a version control system to use.  If I
> was just working by myself or with some experienced software
> engineers, I'd pick git.  However, the folks I was working with were
> often on Windows machines and would want a GUI interface, so I
> picked Mercurial as the version control tool because it had the best
> Windows GUI support at the time (TortoiseHG, which integrates nicely
> with Explorer).  I'm still using Mercurial because I've written a
> number of scripts that help me work in Mercurial repositories.

To get hcpy's files, you first need to install Mercurial.  For a
typical Linux system, this requires a command such as

> `apt-get install Mercurial`

It will be a similar command for other UNIX-style systems.
Alternatively, for a system like Ubuntu, open the Ubuntu Software
Center, type `mercurial` in the search box at the upper-right-hand
corner of the GUI window, and you should see the needed choice as the
first item in the list.  Click on the first item and click the Install
button and it will be on your system shortly (the whole system is only
about 200 kilobytes, so it doesn't take much space).

For Windows, you'll need to download the Mercurial MSI file, then
right click on it in Explorer and select **Install** (you may also need
to adjust your path to allow it to be seen in a DOS console).

Once you have Mercurial working in a console (type `hg` and you
should see a list of basic commands), you then cd to the directory where
you want to look at `hcpy`'s files and execute the command

> `hg clone https://someonesdad1@code.google.com/p/hcpy/`

Then cd to the `hcpy` directory and start working.

Unless you're planning on doing software development and using
Mercurial as your version control system, that's all you need to know
about using Mercurial to get Google Code projects.  Using git or
Subversion are very similar (I keep all three revision control systems
installed on my computer).
