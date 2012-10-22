Getting Help
============

The :term:`shell` is an intimidatingly large collection of features that
requires a lot of practice to use effectively.  There's a lot to learn, but
luckily the developers of the :term:`UNIX` system recognized this.  Their
answer was to provide a set of "online manual pages", or :term:`man page` s,
that describe the use and implementation of the system, its commands, and its
software libraries.  Man pages are indexed, cross-indexed, and searchable.
They are also displayed with a pager like ``less``, allowing you to jump around
in them and close them quickly once you've found what you're looking for.  Best
of all, as a shell user, they're always at the tip of your fingers.

Let's say we want to learn more about using the bash shell.

.. code-block::

   $ man bash
   .......  Long paged document describing the shell in great detail  .......

Sometimes, we're not sure exactly which article we want to read.  We might
think it has something to do with permissions, but we're not sure.  Another
command lets us search the man page index.

.. code-block::

   $ apropos permissions
   .......  Long list of man page article names  ........
   chmod
   .......      More man page article names      ........
   $ man chmod
   .......  Long document describing chmod .......

:term:`man pages` are an invaluable tool.  They give us a wonderful starting 
point for self-reliant, fast problem solving by providing detailed descriptions
and representative examples of proper command and program use.  Making a point
to use man pages early and often will make you a better computer user faster
than going straight to the internet or your local guru for answers.
