Shell Expansions
================

The :term:`shell` provides a mechanism for simplifying commands while
simultaneously making them much more powerful called :term:`expansion`.  In
brief, it allows us to use a variety of special characters and constructions
that the shell will automatically interpret and replace with a more complex set
of arguments before passing them along to the program being run.  This can be
particularly useful if, for example, we want to create, modify, or delete a
large number of files that are named in a similar way without having to
manually type out the name of each and every one.

.. note::

   Expansions can be a little tricky to master, but you can learn the basics
   relatively quickly through some experimentation.  If you are nervous about
   the results of applying a particular expansion and want to find out exactly
   how the shell will interpret it, you can use the ``echo`` command for
   debugging purposes.

Pathname Expansion
==================

Pathname expansion is probably the type of expansion the average user employs
the most in a normal shell session.  It uses wildcards and matching rules to
generate long lists of file and directory names with a minimum of typing.

================ ==============================================================
Construct        Meaning
================ ==============================================================
*                Matches zero or more of anything
?                Matches any single character
[...]            Matches any single character appearing within the brackets
[^...] or [!...] Does not match any character appearing within the brackets
================ ==============================================================

.. code-block::

   $ ls
   Desktop          Downloads           Movies           a.txt
   Documents        Dropbox             Music            b.txt
   $ ls M*
   Movies           Music
   $ ls ?.txt
   a.txt            b.txt
   $ ls D[er]*
   Desktop          Dropbox
   $ ls D[^er]*
   Documents        Downloads

.. note::

   The bracket notation above supports a standard shorthand that uses "class
   names" to represent a range of values.  This include, but are not limited
   to, ``alnum``, ``alpha``, ``ascii``, ``blank``, ``digit``, ``lower``,
   ``upper``, and ``punct``.  When you use a class, surround it with colons,
   i.e. ``[:alpha:]`` will match any single alphabetical character.

We can specify the ``extglob`` shell option with ``shopt`` to gain a richer
set of shell expansion characters and rules that more or less follow standard
:term:`regular expression` rules.  See the :term:`bash` :term:`man page` for
details.

Brace Expansion
===============

Brace expansion lets us dramatically reduce the length and quantity of
arguments involving a regularly varying element with optional preceding and
trailing text.  A typical use case is building a regular directory structure
quickly with ``mkdir -p`` or creating a bunch of files with a common type
extension.

.. code-block::

   $ # Ranges
   $ echo {1..5}
   1 2 3 4 5
   $ echo {a..g}
   a b c d e f g
   $ echo {a..3}
   {a..3}
   $ 
   $ # Lists
   $ echo {this,that,the,other}
   this that the other
   $ 
   $ # Creates a data directory with 20 subject subdirectories, each containing
   $ # 10 sample subdirectories
   $ mkdir -p data/subject{1..20}/sample{1..10}
   $ 
   $ # Creates an address-book directory with a subdirectory for each letter in
   $ # the lexicographic range a-z
   $ mkdir -p address-book/{a..z}
   $ 
   $ # Creates the files __init__.py, constants.py, functions.py, and
   $ # schemas.py, which might be the files required for a planned Python
   $ # library you're just starting
   $ touch {__init__,constants,functions,schemas}.py

When using ranges specified by ``..``, always make sure the types of the
beginning and ending of the range are the same.  As shown above, the range
``{a..3}`` doesn't really make sense and consequently will be used as-is
without expansion.

Tilde Expansion
===============

Sometimes we know we want a command to act on a particular location in the
file system that is easily accessible relative to our :term:`home directory`
or the :term:`current working directory`.  Tilde expansion can make addressing
such locations simpler.  If an argument starts with a tilde (and is optionally
followed by a specifier), that tilde/specifier combination will be replaced by
a path.

========= ====================================================================
Construct Meaning
========= ====================================================================
~         Substitutes in the value stored in the HOME environment variable
~+        Substitutes in the value stored in the PWD environment variable
~-        Substitutes in the value stored in the OLDPWD environment variable
========= ====================================================================

Most users will only regularly use the first construction, but it's nice to
know that other options are available.

.. code-block::

   $ echo ~/code
   /home/bentrofatter/code
   $ pwd
   /home/bentrofatter/some/deep/directory/hierarchy/location
   $ cd ~/code
   $ pwd
   /home/bentrofatter/code
   

Command Expansion
=================

It can be convenient to use the output of one command in the invocation of
another.  Command expansion accomplishes this by wrapping the command to run
first in back tics (``\```) or a dollar sign/parantheses combination.  This
might come in handy if we were writing a script to regularize the names of all
the files in a directory if each of the current file names falls into one of
a set of patterns or somehow wanted to include the output of a command in the
name of a newly created file.

.. code-block::

   $ for file in `ls`
   > do
   >   command $file # In this case, perhaps the command takes but one argument
   > done
   $
   $ touch entry_$(date +%d-%m-%Y).txt
   $ ls
   entry_25-05-2012.txt


That's Not All
==============

:term:`bash` actually supports several other types of expansion, including
arithmetic evaluation and word splitting.  These can be useful when writing
scripts or performing other tasks, but are not necessary for enjoyment of the
command line.  If you're interested, see the bash man page for more
information.
