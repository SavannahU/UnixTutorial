Control Structures
==================

Picture a waterworks play set.  It can be assembled to direct water straight
through from top to bottom, or the various parts can be put together to cause
water to take different paths, possibly at the exclusion of others, to find its
way through.  Program execution is like water flowing through these pipes in
that it has to find a way from the beginning of your code to the end and its
route can be as simple or as complex as you make it.

Neither approach is more "right" than the other, as quicky one-off programs can
be just as useful and save just as much time as longer, more agile code.
However, the ability to organize common functionality and make programmatic
decisions can ultimately be beneficial by reducing the amount of code you have
to write and can even be kind of interesting.

Decision Making
===============

Control structures are only as smart as you make them and are entirely useless
without guidance.  Most control structures must evaluate at least one
conditional statement to determine how to direct the flow of program execution.
This can be accomplished in either of two ways:

-Evaluating an expression using the ``test`` command or its shorthand ``[..]``
 construction
-Checking the :term:`exit status` of a the a program

In normal circumstances, the first option will be preferable and can in fact
subsume the second.

``test`` is used to compare and/or investigate string, integer, and file
arguments and determine if a desired property about them or their relationship
to one another is true or false.  For example, given a location name, ``test``
can determine if it is a file or directory and what kinds of :term:`permission`
s the current user has on it.

.. code-block::

   $ ls
   c/               python/             myscript.sh
   $ # $? is a special variable in bash that holds the return status of the
   $ # most recently executed command.  Usually, 0 is good and anything else
   $ # means the command exited with an error.  In this case, 0 means "true"
   $ # and 1 means "false".
   $ [ -d c ]; echo $?
   0
   $ [ 'dog' = 'dog' ]; echo $?
   0
   $ [ 'dog' = 'cat' ]; echo $?
   1
   $ [ 5 -gt 8 ]; echo $?
   1
   $ [ "dog" -eq 12 ]; echo $?
   bash: [: dog: integer expression expected
   $ # You can make a compound test with -a for "and" and -o for "or"
   $ [ 'dog' = 'dog' -a 5 -gt 8 ]; echo $?
   1
   $ [ 'dog' = 'cat' -o 5 -lt 8 ]; echo $?
   0

.. note::

   The syntax for the square brackets version of ``test`` requires white space
   immediately after the opening bracket and immediately before the closing
   bracket.

.. warning::

   When using ``test`` for comparisons, it's important to make sure you're
   comparing items of the same type, as otherwise you're likely to throw an
   error or get unexpected results.

If Statements
=============

The ``if`` statement is probably the simplest form of program flow control.  It
checks a condition and, if it's true, executes a set of statements.  The ``if``
statement can check subsequent conditions if the first one is false using an
arbitrarily long set of ``elif`` s, and can implement a default action with an
``else`` clause.  ``if`` structures are terminated by ``fi``.

.. code-block::

   if condition
   then
      statements
   [elif condition
      then statements ... ]
   [else
      statements ... ]
   fi

   $ animal='dog'
   $ if [ 'dog' = $animal ]
   > then
   >    echo "The animal was a dog"
   > endif
   The animal was a dog
   $
   $ if [ 'cat' = $animal ]
   > then
   >    echo "Meow"
   > elif [ 'dog' = $animal ]
   >    then
   >        echo "Ruff"
   > else
   >    echo "Animal was $animal"
   > fi
   Ruff


Case Statements
===============

``case`` statements take an expression and compare it to a list of patterns.
If the expression matches a pattern, the code immediately following that
pattern will be executed.  At the end of each pattern's code, you must include
a ``;;`` to indicate a "case break".  An alternate, though typically undesired
option, is to end a pattern's code with ``;&``.  This will cause execution to
"fall through" to the next pattern's code.  Like ``if`` statements, a ``case``
structure is terminated with the control structure's name in reverse, ``esac``.

.. code-block::

   case expression in
     pattern1)
         statements ;;
     pattern2)
         statements ;;
     ...
     [* ) # catchall, used as default
         statements ;;]
   esac

   $ file=a.txt
   $ case $file in
   >    \*.csv)
   >        echo "File is a comma-separated values file"
   >        ;;
   >    \*.txt)
   >        echo "File is a text file"
   >        ;;
   >    \*.tab)
   >        echo "File is a tab-delimited file"
   >        ;;
   >    \*) echo "File type unknown"
   >        ;;
   > esac
   File is a text file.
   $
   $ number=1
   $ case $number in
   >    0)
   >        echo "Number is less than 1"
   >        ;&
   >    1)
   >        echo "Number is less than 2"
   >        ;&
   >    2)
   >        echo "Number is less than 3"
   >        ;;
   >    \*)
   >        echo "$number is not a number between 0 and 2"
   > esac
   Number is less than 2
   Number is less than 3

While and Until Loops
=====================

The ``while`` loop is the simplest type of looping control structure.  Before
executing the statements in the loop, ``while`` loops check the condition
provided to them.  If the condition is true, the loop is executed, otherwise,
it is skipped.

.. code-block::

   while condition
   do
      statements ...
   done

   $ COUNTER=0
   $ while [ $COUNTER -lt 3 ]
   > do
   >    echo "The counter is $COUNTER"
   >    let COUNTER=COUNTER+1
   > done
   The counter is 0
   The counter is 1
   The counter is 2

``until`` loops behave exactly like ``while`` loops except that the condition
is negated.

.. code-block::

   while condition
   do
      statements ...
   done

   $ COUNTER=5
   $ until [ $COUNTER -lt 3 ]
   > do
   >    echo "The counter is $COUNTER"
   >    let COUNTER-=1
   > done
   The counter is 5
   The counter is 4
   The counter is 3

For Loops
=========

There are two types of ``for`` loop available in :term:`bash`.  The first
iterates through a list of items, potentially executing a set of statements
for or on each one.  On each iteration, it saves the current item to the
target variable.  The list can be explicitly listed or can be the result
of a script or command substitution.  This ``for`` loop is fairly natural to
non-programmers since it doesn't rely on a conditional setup.

.. code-block::

   for target in list of items
   do
      statements ...
   done

   $ ls
   a.txt            b.txt           c.txt
   $ for file in `ls \*.txt`
   $ # or equivalently in this case, though less flexible:
   $ # for file in a.txt b.txt c.txt
   > do
   >    echo "$file has `wc -l $file` lines"
   > done
   a.txt has 10 lines
   b.txt has 250 lines
   c.txt has 17 lines
   $

The second type of ``for`` loop uses a syntax familiar to anyone who has worked
with the :term:`C` programming language.  The first line uses a three part
syntax composed of an initialization expression (expr1 below, used, for example,
to set a counter to an initial value), a continuation condition that must be
true for the loop to execute (expr2 below), and an expression to execute at the
end of each iteration (expr3 below, used, for example, to increment a counter).

.. code-block::

   for ((expr1; expr2; expr3))
   do
      statements ...
   done

   $ for ((i=0; i<5; i++))
   > do
   >    echo "On iteration $i"
   > done
   On iteration 0
   On iteration 1
   On iteration 2
   On iteration 3
   On iteration 4
   $

.. note::

   The previous example may be confusing to anyone who has never programmed a
   ``for`` loop.  Looking at the first line, we're saying that given a starting
   value of 0 for the variable ``i``, as long as i < 5, go into the loop and
   ``echo`` the statement.  At the end of each iteration, increase the value of
   ``i`` by 1 using the increment operator ``++``.  When ``i`` is incremented
   to 5, the continuation condition will fail, causing the loop to end.

Select Loops
============

The ``select`` structure provides a method of getting user input during the
execution of a script.  Provided a set of options, it prints them to
:term:`stderr` in a numbered list.  The user enters the number of the option
desired and that response is stored in the target variable.  Until the loop
is explicitly stopped with ``break``, it will continue to execute its code
and prompt the user for input indefinitely.  Ending the loop unconditionally
after a single execution can prevent this, as can coding an option that
indicates the loop should end.

.. code-block::

   select target in list of options
   do
      statements ...
      break;
   done

   $ select topping in peppers mushrooms cheese
   > do
   >   echo "You picked $topping"
   >   break
   > done
   1) peppers
   2) mushrooms
   3) cheese
   #? 3
   You picked cheese
   $ # Setting PS3 to a different value will customize the input line
   $ PS3="Choose a topping: "
   $ select topping in peppers mushrooms cheese
   > do
   >   echo "You picked $topping"
   >   break
   > done
   1) peppers
   2) mushrooms
   3) cheese
   Choose a topping: 2
   You picked mushrooms
