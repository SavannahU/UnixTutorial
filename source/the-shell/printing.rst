Printing
========

If we're working on some plain text files, printing them from the :term:`shell`
is simple and fast.  As with the typical printing interface provided through
:term:`GUI` programs, the shell comes complete with a way of keeping track of
pending print jobs and, if necessary, canceling them.

To print, simply use the ``lp`` command followed by the file name.  ``lp``'s
output can be customized via a variety of options configurable with the ``-o``
flag.  Cancelling a print job requires the ``cancel`` command.

.. code-block::

   $ # Note, calling lp returns a print job id.  This can be used to cancel
   $ # the job if we need to.
   $ lp myscript.sh
   request id is Mitanni-44 (1 file(s))
   $
   $ # We've just realized that what we meant to print was myscript2.sh
   $ cancel Mitanni-44
   cancel Mitanni-44
   $
   $ lp -o sides two-sided-long-edge -o pretty-print myscript2.sh
   request id is Mitanni-45 (1 file(s))

.. note::

   ``lp`` stands for line printer and is one of many UNIX commands with a name
   that dates it to an earlier age in computing.  While line printers are still
   used in situations that require high reliability, they have been largely
   replaced in the consumer market with more modern alternatives.

A more complete listing of the options available when printing with ``lp`` can
be found in the :term:`man page` s for ``lp`` and ``lpoptions``, but some
common ones are as follows.

+----------------------------+-----------------------------------------------+
| Option                     | Effect                                        |
+============================+===============================================+
| sides one-sided            | Prints on one or both sides of the paper      |
|       two-sided-long-edge  | (assuming your printer supports duplexing).   |
|       two-sided-short-edge |                                               |
+----------------------------+-----------------------------------------------+
| number-up                  | Sets the number of pages printed per side     |
+----------------------------+-----------------------------------------------+
| cpi                        | Sets characters per inch.  Defaults to        |
|                            | something like 10.                            |
+----------------------------+-----------------------------------------------+
| lpi                        | Sets lines per inch.  Defaults to something   |
|                            | like 6.                                       |
+----------------------------+-----------------------------------------------+
| page-bottom                | Sets the size of the indicated page margin in |
| page-left                  | inches.                                       |
| page-right                 |                                               |
| page-top                   |                                               |
+----------------------------+-----------------------------------------------+
| pretty-print               | Varies by system, but generally adds page     |
|                            | numbering, headers, footers, page outlines if |
|                            | number-up is greater than 1, and syntax       |
|                            | highlighting if the file is a recognized      |
|                            | programming language.                         |
+----------------------------+-----------------------------------------------+

``lpq`` and ``lpstat`` can be used to see the current print queue or glean
information about the current state of the print service.

.. code-block::

   $ lpq
   Rank    Owner           Job     File(s)                         Total Size
   active  bentrofatter    65      intro.rst                       8192 bytes
   1st     bentrofatter    66      navigation.rst                  12000 bytes
   2nd     bentrofatter    67      index.rst                       4500 bytes

   $ lpstat
   Mitanni-66              olduvaihand      15360   Sun 21 Oct 2011 02:36:40 AM
