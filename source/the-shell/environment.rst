The bash Environment
====================

The environment provided by the bash :term:`shell` is highly customizable and
configurable.  We can take advantage of this by creating configuration files
that the shell loads when we open a terminal that set up environment
variables, identify default programs for various tasks, and adjust the look
and feel of the terminal window.

Depending on how your personal system is set up, you will probably have at
least one of the following hidden configuration files already sitting in your
home directory::

   -.bash_profile
   -.bash_login
   -.profile
   -.bashrc

.. note::

   If you're unsure about whether one or more of these files exist, open a
   shell into your home directory and list out all hidden files with ``ls -a``.
   In doing so, you'll probably see several other hidden files whose names end
   in ``rc``; these :term:`rc file` s are your user-specific configuration
   files and are used by a wide variety of programs.

When you open an interactive shell, bash will look for a valid file having one
of these names (see your local :term:`man page` for the exact order) and
execute its commands.  

bash Settings
=============

The bash runtime can be modified with the built-in ``set`` command.  Changing
the runtime can affect the controls available for interacting with the
:term:`shell`, its default file-handling behaviors and safeguards, and/or the
amount of feedback it gives you.  Some common runtime options include:

========= ==================================================================
Command   Effect
========= ==================================================================
emacs     Puts the shell in :term:`emacs` mode, which allows command editing
          similar to that available in the emacs text editor
vi        Puts the shell in :term:`vi` mode, which allows command editing
          similar to that available in the vi text editor (vi and emacs are
          mutually exclusive options)
history   Record a command history that is accessible via the ``history``
          command.  (Enabled by default)
noclobber Prevents the shell from overwriting existing files when using the
          ``>`` stream redirection operator.
verbose   Print input lines as they are read.
========= ==================================================================

``set`` can be used in configuration files or directly on the command line.
The syntax is identical regardless of context.

.. code-block::

   $ set -o emacs
   $ # emacs editing mode is now enabled
   $ set -o vi
   $ # vi editing mode is now enabled
   $ set -o verbose
   $ ls
   ls
   c/           python/             myscript.sh

Another command associated with setting shell options is ``shopt``, which can
be used to add some higher level interaction behaviors.  The syntax is similar
to ``set`` in that you pass ``shopt`` the name of a behavior or setting, but
it doesn't require an ``-o`` flag.  Two particularly nice ``shopt`` options are
``cdspell``, which can figure out the intended target of a ``cd`` command when
the directory is misspelled, and ``cmdhist``, which will attempt to store a 
multiline command in a single ``history`` entry for easier editing.

Environment Variables
=====================

The :term:`shell` (and many programs run from it) rely on
:term:`environment variable` s to determine how they should behave.  You can
override the default values of these variables in a configuration file or from
the command line.  Some environment variables you might be interested in
setting yourself include:

======== ======================================================================
Variable Effects
======== ======================================================================
PATH     The list of directories the shell goes through when trying to find a
         program you have asked it to run.  While we probably don't want to 
         completely replace the default value, if we've written our own scripts
         and put them all into ``~/bentrofatter/scripts``, appending that
         directory to the default PATH will let us execute those scripts like
         any other program.
HISTSIZE The number of commands to keep in the history.
PAGER    The default paging program to use when displaying things like man
         pages.  It's usually pretty safe to use the ``less`` pager, which can
         typically be found at ``/usr/bin/less`` or some place similar.
EDITOR   The default editor program to use when scripts require file editing.
         Set this to your preferred text editor.  Values might include 
         ``/usr/bin/vi``, ``/usr/bin/emacs``, or ``usr/bin/nano`` if you have
         the ``vi``, ``emacs``, and/or ``nano`` editors installed in
         ``/usr/bin``.
PS1      The primary command prompt.  This can be heavily customized, and can
         show something as simple as a single ``$`` or something as complex as
         a script generated string that indicates your username, your current
         working directory, the date and time, and whether or not the last
         command issued failed or succeeded.  See the bash man page for an
         introduction to some of the options available.
PS2      The secondary command prompt.  PS2 (and for that matter, PS3 and PS4)
         works just like PS1 with respect to customization.  You see PS2 when
         you issue a multiline command, e.g. an if-then conditional structure
         that you've split across multiple lines.
======== ======================================================================

See the :term:`bash` :term:`man page` for a full listing of all bash-specific
environment variables.  Other programs may use their own custom environment
variables.  Man pages are a great place to find out exactly what options are
available.

.. code-block::

   $ # Append a directory to PATH
   $ echo $PATH
   .:/bin:/sbin:/usr/bin:/usr/sbin
   $ # The colon separating the current value of PATH from the new directory
   $ # is required.  Also, note the lack of spaces in the variable assignment.
   $ # That's syntatic and very important.
   $ PATH=$PATH:~/scripts
   $ echo $PATH
   .:/bin:/sbin:/usr/bin:/usr/sbin:/home/bentrofatter/scripts
   $ export PATH

.. note::

   To make the new value of the variable accessible throughout your shell
   session, always add an ``export`` line in your configuration file somewhere
   after setting it.

Alias
=====

The ``alias`` command lets you conveniently create shortcut commands that 
automatically include commonly used option flags.  Let's say we know that when
we list out the contents of a directory, we always want ``ls`` to use a long,
human-readable format.  This could be accomplished in a number of ways without
typeing ``ls -lh`` every single time.  To see a list of all aliases currently
in use, we can execute ``alias`` without additional arguments.  To disable an
alias, use ``unalias`` and the name of the alias to remove.

.. code-block::

   $ # You could make a new command
   $ alias myls="ls -lh"
   $ # You could override the ls command
   $ alias ls="ls -lh"
   $ alias
   alias myls='ls -lh'
   alias ls='ls -lh'
   $ unalias myls
   $ alias
   alias ls='ls -lh'
