Streams
=======

:term:`UNIX` uses the concept of :term:`stream` s to model input and output.
Streams behave a lot like files, allowing programs to be agnostic about where
they get information and how they return it to the user.  The three standard
streams are :term:`stdin`, :term:`stdout`, and :term:`stderr`, which represent
the standard input, standard output, and standard error streams, respectively.
By default, :term:`stdin` is the command line and :term:`stdout` and
:term:`stderr` are directed to the terminal.

Redirection
===========

Streams can be redirected to create or modify files or change the behavior of
commands.  To create a new file from the :term:`stdout` of a command, we can
use the ``> filename`` construction.  To append the output of a command to an
existing file (or create a new one if the file doesn't already exist) we have
the ``>> filename`` construction.

.. code-block::

   $ # Redirect output to file
   $ date > rightnow.txt
   $ cat rightnow.txt
   Mon Oct 21 10:10:10 CDT 2012
   $
   $ # Append output to file
   $ date >> rightnow.txt
   $ cat rightnow.txt
   Mon Oct 21 10:10:10 CDT 2012
   Mon Oct 21 10:10:15 CDT 2012
   $
   $ # Unconditionally overwrite file with output
   $ date >| rightnow.txt
   $ cat rightnow.txt
   Mon Oct 21 10:10:20 CDT 2012

.. note::

   The ``>`` redirect character will overwrite an existing file if the
   ``noclobber`` option is disabled.  If it's enabled, the command will fail
   with an error.  To override ``noclobber``, use ``>|`` instead.

When redirecting output, you should notice that messages sent to :term:`stderr`
continue to appear in the terminal.  Like :term:`stdout`, we can redirect
stderr; in this case we use ``2>`` and ``2>>``.

.. note::

   :term:`UNIX` uses the concept of :term:`file descriptor` s to refer to keep
   track of files and streams internally, which are basically just tags with
   id numbers that are assigned in ascending order.  Because :term:`stdin`,
   :term:`stdout`, and :term:`stderr` are essentially always present, they are
   assigned the first three file descriptors: 0, 1, 2.  stdout can actually be
   redirected using ``1>`` and ``1>>``, but it's unnecessary to be explicit
   because that is the default behavior.

.. code-block::

   $ # If mycommand creates output and reports a lot of errors and/or status
   $ # updates via stderr along the way, it might be good to keep track of
   $ # the errors and updates without cluttering up the terminal window.  The
   $ # command.log can then be examined at our leisure.
   $ mycommand > output 2>> command.log

To redirect stdout and stderr to the same place, we use ``&>`` and ``&>>``
for writing and appending, respectively.

Pipes
=====

A core point of the :term:`UNIX` philosophy is the development of small,
highly specialized programs that do one thing really, really well.  By treating
:term:`stdin`, :term:`stdout`, and :term:`stderr` like generic files,
:term:`UNIX` allows string several of these small programs into chains of
program execution called :term:`pipe` s or :term:`pipeline` s.  In short, the
stdout of one program becomes the stdin of the next one until we reach the
end of the pipe, at which point we get our desired output.

Some of the benefits of working with plain text files become readily apparent
when we consider that a program designed to accept plain text input and return
plain text output can be dropped into an arbitrary pipeline and expected to
work without having to use a bunch of format-specific code.  Pipes can be used
to rapidly build complex programs out of simple to use, modular subprograms.

As an example, let's say we have 2 comma-separated value files, each filled
with thousands of rows of data.  Because of the analysis we're doing, we need
to identify all of the rows that are found in both files.  While the file
formats are identical, the data was gathered by untrained research assistants
and consequently hasn't been ordered in a meaningful way.  We could combine
the files in Excel, sort the rows, and then write a macro to eliminate the
unique rows (or for that matter, do it manually), but that will take a lot of
time and Excel macros can be kind of tricky to get right.  What are we to do?
It just so happens that :term:`UNIX` provides us with small programs that do
each of three major portions of the task at hand very well: ``cat`` is used
to concatenate, or join, files; ``sort`` is adept at sorting input; ``uniq``
is used specifically to identify unique lines in a sorted input (and with 
the ``-d`` flag will output only the duplicated ones).  With these building
blocks, a :term:`pipe` that can accomplish our seemingly time consuming task
ends up being fairly simple to describe.

.. code-block::

   $ cat file1.csv file2.csv | sort | uniq -d > dupes.csv

Sometimes debugging pipes can be difficult, and sometimes it's nice to be able
to grab the output from an intermediate step in the pipe without breaking the
pipe into smaller pieces.  To address these problems, you can use the ``tee``
command to capture output at key points in your pipe.  ``tee`` is named for
the T-shaped piece of plumbing that can be used to direct water to two
different locations at the same time.  Using ``tee``, we can direct the
:term:`stdout` to both the next program in the pipe and a file (or the
terminal) that we can then use to sort out problems or examine the
intermediate data.

.. code-block::

   $ cat file1.csv file2.csv | sort | tee sorted.csv | uniq -d > dupes.csv

This modified version of our original solution creates two files:
``sorted.csv`` and the original ``dupes.csv``.
