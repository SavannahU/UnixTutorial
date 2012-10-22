Navigation
==========

If you can't find your way around a computer, you're unlikely to be able to do
much of anything else with it.  This is usually achieved via the point and
click interface of your :term:`GUI`.  Since the mouse isn't something that we
have generally meaningful access to on the command line, the first step toward
becoming a literate :term:`shell` user is figuring out how to replace it.

Luckily, if you know how to navigate a directory structure, you already have
the intellectual scaffolding.  You still have a structure divided into files
and directories, and you can still have a sense of position in that filesystem.
The biggest difference is how important that sense of position is.

.. note::

   Because you can have multiple windows open in a :abbr:`GUI`, you can
   maintain multiple views into the file system.  With the :term:`shell`,
   you're firmly situated in a particular directory at all times which is
   roughly equivalent to an active Finder window.

Your current position influences the programs available to you, the manner in
which you address files you want to interact with, and what the :term:`shell`
will actually allow you to do.


Getting around
==============

After opening a terminal, the first question to answer is "Where am I?"  This
is a simple question and can be answered easily with a simple command.

.. code-block::

   $ pwd
   /home/bentrofatter

``pwd`` means "print working directory", and that's exactly what it does.  In
keeping with the :term:`UNIX` philosophy, this is a command that has a single
duty that it performs admirably.

.. note::

   As you'll notice, :term:`shell` commands often have terse names that, while
   often related to the function of the command, are not always obvious and can
   be difficult to remember.  This is a holdover from the days when computers 
   were the size of rooms and the people using them had to do so over a
   connection that was slow enough that every character you typed took a
   measurable amount of time to actually show up on your screen.  It's probably
   the biggest challenge for new users and requires some practice, but
   clearly demonstrates that :term:`UNIX` has been doing a few things right
   since its development in the 1970s.

Now that we know which directory we're in, we probably want to have an idea
about what files are available to work on.  To do this, we have to tell the
computer to list them out.

.. code-block::

   $ ls
   Desktop          Library         Pictures        code
   Documents        Movies          Public          
   Downloads        Music           Sites
   $ # You can also specify a different directory to list
   $ ls code
   c/               python/         myscript.sh

This is equivalent to a simple icon view in the :term:`GUI`, but we can do
better.  Like many other commands, ``ls`` has a wide variety of options, or
:term:`flag` s, that control the way it does its job.  In this case, we're
interested in getting some summary information about the files, a description
of their :term:`permissions`, and when they were last modified.

.. code-block::

   $ ls -lh
   drwx------+   9 bentrofatter  staff   306B Apr 22 10:54 Desktop
   drwx------+  21 bentrofatter  staff   714B Apr 20 17:25 Documents
   drwx------+ 187 bentrofatter  staff   6.2K Apr 21 17:00 Downloads
   drwx------+  35 bentrofatter  staff   1.2K Feb 22 14:27 Library
   drwx------+   6 bentrofatter  staff   204B Mar 25 22:41 Movies
   drwx------+   5 bentrofatter  staff   170B Jul 24  2010 Music
   drwx------+  35 bentrofatter  staff   1.2K Apr 13 13:36 Pictures
   drwxr-x---+   6 bentrofatter  staff   204B Dec  2 13:35 Public
   drwxr-xr-x+   4 bentrofatter  staff   136B Jan  7 16:53 Sites
   drwxr-xr-x   15 bentrofatter  staff   510B Apr 20 16:54 code

This is called a long lising (``-l``) with a human friendly (``-h``) readout,
and tells you a lot about the individual files.  The first column describes
the types and :term:`permissions` of the items.  Without going into too much
detail, we can quickly see that these are directories (as indicated by the
``d`` at the beginning of each line) and only the owner, ``bentrofatter``, is
allowed to examine the contents of all but the Public, Sites, and code 
directories.  For more information on :term:`permissions`, check out 
:ref:`the-shell-permissions`.

The filenames in the rightmost column are :term:`relative path` s that
indicate the file's position in the filesystem relative to the current
working directory.  When we combine them with the output of ``pwd``, we get the
file's :term:`absolute path`, which is their unique identifier within the
filesystem.  Relative paths are convenient as a shorthand for addressing files
if you're paying attention to where you are in the filesystem, but absolute
paths will always work from any working directory.

.. note::

   Absolute paths are composed of a directory name and a base filename.
   Sometimes it's convenient to parse these names out of an absolute path.
   The programs ``dirname`` and ``basename`` do just that and nothing else.

Now, let's explore the ``code`` directory.  To change our
:term:`working directory``, we do the following.

.. code-block::

   $ cd code
   $ # To show that we've change directories, use pwd (btw, this is a comment)
   $ pwd
   /home/bentrofatter/code
   $ ls
   python           c           myscript.sh

So ``cd`` is like double-clicking on a directory, but it's not limited in
the same way.  If we're in our home directory and know we want to go to the 
directory called ``python`` inside of ``code``, rather than traversing the
file system one directory at a time, we can jump directly there using the
:term:`relative path` ``code/python`` or the :term:`absolute path`
``/home/bentrofatter/code/python``.  To make things even faster, the
tab-completion feature will automatically finish partial but uniquely
identifying names for files and directories.

.. code-block::

   $ pwd
   /home/bentrofatter
   $ cd code/python
   $ pwd
   /home/bentrofatter/code/python
   $ # This will work from anywhere in the filesystem because it is an absolute
   $ # path:
   $ cd /home/bentrofatter/code/python

A convenient feature of modern file browsers is a history that permits movement
"Back" and "Forward" in order of most recently visited.  The :term:`shell`
implements a similar feature with the ``pushd``, ``popd``, and ``dirs``
commands.  Shell navigation history is an optional feature that acts like a
:term:`stack` that you can push on and pop off directory names.

.. code-block::

   $ pwd
   /home/bentrofatter/some/deeply/nested/directory/that/takes/too/long/to/type
   $ # Some implementations require that the directory to push be explicitly
   $ # named.  pushd will print the current stack.
   $ pushd .
   /home/bentrofatter/some/deeply/nested/directory/that/takes/too/long/to/type
   $ cd ~
   $ pwd
   /home/bentrofatter
   $ # If you forget what's on the stack, you can always view it with dirs
   $ dirs
   /home/bentrofatter/some/deeply/nested/directory/that/takes/too/long/to/type
   $ # we've done some work and want to return to our deeply nested directory 
   $ popd
   bash: popd: directory stack empty
   $ pwd
   /home/bentrofatter/some/deeply/nested/directory/that/takes/too/long/to/type

The amount of time you spend navigating the filesystem actually does add up.
Navigating the shell is easy with a little practice, and can be used to quickly
zip around the filesystem without the hassle and interuption of taking your
hands off the keyboard.


Replacing right-click
=====================

Continuing with our discussion about eliminating the necessity of the mouse,
let's talk about some basic filesystem managment.  With a :term:`GUI`, we
click and drag or use in-place context menus to create, move, and otherwise
manipulate files and directories.  The shell supplies these functions via a
simple set of commands.

First, we can make directories with the ``mkdir`` command.

.. code-block::

   $ ls
   c/              python/              myscript.sh
   $ mkdir data1
   $ ls
   c/              data1/              python/               myscript.sh
   $ # You can make an entire filesystem with the -p flag.  This will create
   $ # all intermediate parent directories
   $ mkdir -p data2/subject1
   $ ls data2
   subject1/
   $ # You can use shell expansions to form more complex filesystems.
   $ mkdir -p data3/subject{1..50}/{data,timings,scores}

.. note::

   ``mkdir -p`` might seem like a relatively small convenience, but it can save
   you a large amount of time if used creatively.  That last command will
   create an entire subdirectory structure inside the new ``data3`` directory:
   for each of 50 subjects, we've just created 3 subdirectories for storing
   data.  Had we done this with a mouse, it would have required right-clicking, 
   making a new directory, and naming it up to 200 individual times.  That
   could easily take more than 10 minutes.

Removing directories is just as simple.

.. code-block::

   $ ls
   c/           data2/      python/
   data1/       data3/      myscript.sh
   $ rmdir data1
   $ ls
   c/           data2/      data3/      python/     myscript.sh
   $ # You can't remove a directory that isn't empty.  This is one of only a
   $ # few default safe-guards provided by the shell.  It can, of course, be
   $ # overridden.
   $ rmdir data2
   rm: cannot remove `temp': Is a directory
   $ # It's possible to delete all parent directories with a 
   $ rmdir -p data3/subject{1..20}/{data,timings,scores}


Files can be created in many ways, but one of the simplest is using ``touch``
to create an empty file with the indicated name.  Files and directories can
be removed with ``rm``.

.. code-block::

   $ ls myfile
   ls: cannot access myfile: No such file or directory
   $ touch myfile
   $ ls myfile
   myfile
   $ rm myfile
   $ ls myfile
   ls: cannot access myfile: No such file or directory
   $ # rm can also be used to override the previously mentioned restriction on
   $ # deleting non-empty directories with its recursive flag
   $ rm -r data3

.. warning::

   ``rm -r`` is a powerful command that should be used carefully as it will
   erase everything below the directory name provided to it.  When combined
   with the ``-f`` flag (force), it can be absoutely devastating.  Never, ever
   issue the command ``rm -rf /``, as it could delete your entire filesystem
   in a way that would be very difficult or impossible to recover from.

We can copy and move files and directories with the ``cp`` and ``mv`` commands.

.. code-block::

   $ ls
   c/           data2/      python/     myscript.sh
   $ ls data2/
   subject1/
   $ mv data2 data1
   $ ls
   c/           data1/      python/     myscript.sh
   $ ls data1/
   subject1/
   $ cp myscript.sh script.sh
   $ ls
   c/           data1/      python/     script.sh
   $ # Copy can recursively copy a directory with the -R flag
   $ cp -R data1 newdata
   $ ls newdata
   subject1/

One last convenience provided by the graphical desktop is the ability to create
aliases or shortcuts to files and directories.  In the :term:`shell` these are
referred to as links, which come in two varieties: :term:`hard link` s and 
:term:`soft link` s.  A hard link gives the file or directory an additional
reference in the file system and makes it so that, after creating a hard link,
deleting the original file or directory will not result in a loss of data.  A
soft link, on the other hand, acts more as a sign pointing to a location in the
file system, regardless of whether there is actually something at that location.

.. code-block::

   $ # The general syntax creates a hard link in the current working directory
   $ ls
   c/       data1/      python/     script.sh
   $ cd python
   $ ls
   $ ln ../script.sh
   $ ls
   script.sh
   $ rm ../script.sh
   $ # Note, after deleting the original script.sh reference, the file still
   $ # exists in the python directory
   $ ls -l
   -rwxr--r-- 1 olduvaihand olduvaihand 13 Apr 22 23:33 script.sh
   $ cd ..
   $ # A soft link is just a sign that points to the actual file
   $ ln -s python/script.sh
   $ ls -l
   drwxrwxr-x 1 olduvaihand olduvaihand 13 Apr 22 23:33 c/
   drwxrwxr-x 1 olduvaihand olduvaihand 13 Apr 22 23:33 data1/
   drwxrwxr-x 1 olduvaihand olduvaihand 13 Apr 22 23:33 python/
   lrwxrwxrwx 1 olduvaihand olduvaihand 13 Apr 22 23:33 script.sh -> python/script.sh
   $ # Deleting python/script.sh breaks the link, causing it to point to
   $ # an empty file
   $ rm python/script.sh
   $ rm script.sh

We've just about replaced the mouse.  Now that we can manage the file system,
the last major thing we're missing is the ability to examine files and
introspect on them.


Examining and locating files
============================

Depending on how your :term:`GUI` is set up, it can probably show you a large
amount of summary data about or a preview of a file or directory when you click
on it.  Again, the :term:`shell` provides you with similar functionality.

Because file extensions can't always be trusted, you can determine the type of
a file using the ``file`` command.  If the file isn't a type stored in ``file``
database of formats, it will simply tell you if the data is binary or plain
text.

.. code-block::

   $ file python
   python: directory
   $ file script.sh
   script.sh: Bourne-Again shell script text executable

There are several programs which can be used to view the contents of a text
file.  We can print the full contents of a file with ``cat`` or page through
them with ``more`` or ``less``.  ``less`` is an improved version of ``more``
that can react gracefully when you accidentally (or intentionally) use it to
view a binary file.  ``head`` and ``tail`` are used to print out the first
and last lines of file.  The default number of lines to print can be overridden
with the ``-n`` flag and an integer argument.

.. code-block::

   $ cat haiku.txt
   Hunger consumes me
   If I could consume hunger
   I would be so full
   $ head -n 4 catch_22.txt
                                          1
                                      The Texan

   It was love at first sight.
   $ tail -n 3 catch_22.txt
       "Jump!" Major Danby cried.
       Yossarian jumped.  Nately's whore was hiding just outside the door.
   The knife came down, missing him by inches, and he took off
   
If you don't know exactly where a file is, it's nice to be able to search for
it.  The :term:`shell` gives you a way to do that with ``find`` and ``locate``.
``find`` uses a somewhat complicated set of arguments and so is a bit advanced,
but ``locate`` is fairly straight-forward.  After setting up the database it
uses to search for files, it can inform you of paths matching the patterns you
give it.

.. code-block::

   $ pwd
   /home/bentrofatter
   $ locate script.sh
   /home/bentrofatter/code/script.sh

Moving On
=========

Some conclusion-ish remarks about replacing the mouse.
