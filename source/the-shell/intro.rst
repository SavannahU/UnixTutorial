What's in an interface?
=======================

The :term:`operating system` controls the hardware of your computer, but that
alone doesn't make the machine a useful tool.  It requires a substantial 
amount of input from you to get things done.  However, since you can't talk
directly to the :abbr:`OS (operating system)`, you need something to act as an
intermediary that can translate your requests into a form the underlying
system software can understand. The most common way for the average end user
to interact with the :abbr:`OS (operating system)` -- be it OS X, Windows, or
the typical desktop Linux distribution -- is through a windowed,
:term:`graphical user interface`, or :abbr:`GUI (graphical user interface)`.

The :abbr:`GUI (graphical user interface)` is controlled by user input devices
like keyboards, mice, tablet/pen combinations, and touch screens and offers an
easy to understand representation of the computer's file system and programs.
While the keyboard is certainly the biggest standard input device most people
use, people use the mouse to accomplish most tasks, particularly navigating
through folders, selecting files, browsing the web, and issuing commands via
application menus.  (Typing is certainly important for document authoring, but
you can get through a surprising amount of your daily computer-related tasks
without having your hands constantly hovering over the keys.)

This desktop metaphor is easy to learn because it works conceptually and
visually in a manner familiar to anyone who's ever seen a desk, filing cabinet,
and manilla folder, but it's not the only way to convey your intentions to a
computer.  Afterall, :abbr:`GUIs (graphical user interface)` have only even
been around since the late sixties (and only in common use amongst end-users
since the introduction of the Macintosh in the early eighties), while
programmable machines have been available in one form or another for something
like 800 years.  The principle alternative available today is the
:term:`command line interface` or :abbr:`CLI (command line interface)` --
usually referred to simply as the :term:`shell`.

.. note::

   Like the :abbr:`GUI`, the :term:`shell` sits between the user and the 
   underlying system, acting as a translator from human-to-machine and vice
   versa.  The interface is low level, and can be thought of as wrapping
   the :term:`kernel`, the most important part of the :abbr:`OS`, hence the
   name.

The :term:`shell` is a simple, elegant way of interacting with an operating
system via textual messages typed solely at a keyboard.  Like :abbr:`GUIs`,
shells come in a variety of flavors, including the classic :term:`sh`, its
more modern replacement :term:`bash`, Microsoft's :term:`cmd` prompt (a
descendant of :term:`DOS`), and many others.  Some have special features
like limited support for mice and primitive clickable interfaces, but the
value of any shell comes primarily from its rich collection of specialized
commands (as opposed to the generic mouse-click) and programmable, highly
customizable environment.


What difference does it make?
=============================

:abbr:`GUIs` and :abbr:`CLIs` are fundamentally very similar in that they both
facilitate:

- Navigation
- Display and manipulation of data
- Interaction with programs
- Creation of output

However, they differ dramatically in terms of flexibility, extensibility,
efficiency, and overall control provided to the user.  Judicious use of the
:term:`shell` can often cut hours of tedium down to the amount of time it
takes to carefully describe what you want to get done.  Best of all, depending
on how you choose to execute your solution, you can probably save or record it
for future, general use, substantially reducing the amount of repetition
that might otherwise be necessary to apply it to a large number of files or
a separately generated dataset at a later time.  Remember, computers are at
their best when handling the types of tedious tasks that humans have trouble
with because of inattention to detail or inconsistent application of actions.


The UNIX way
============

Before diving into the :term:`shell`, it's probably worth mentioning a few
details about how a :term:`UNIX` system is organized and what assumptions are
being made about files, programs, and you, the user.

Broadly, everything in :term:`UNIX` can be thought of as a file, including
normal files, directories, the terminal window itself, and disk drives.  By
abstracting all of these components into roughly the same sorts of things, a
:term:`UNIX` system is able to handle and interact with them in roughly
similar ways.  This means that programs that expect data coming from the 
command line might just as easily be able to accept input from a pre-written
script file or another program.

Second, :term:`UNIX` programs tend to small and specialized.  For example, a
common program that we'll discuss later is the ``wc`` program, which is used
to count the number of characters, words, and lines in a text file.  ``wc``
doesn't do anything else, and at first blush that might seem kind of silly,
but it is very good at its particular task.  Because ``wc`` counts characters,
words, and lines so well, you'll never have to write your own program to do 
the same job, which means that if you're working on a larger project that
requires line counts, you can just directly integrate ``wc`` into your own
work with the confidence that it will do its part correctly.  Modularity makes
it much easier to write more complicated, special purpose programs from well
tested, general components.  To reiterate the point, ask yourself, when was
the last time you had to build your own LEGO brick?

Finally, :term:`UNIX` is really at its best when it's working with plain text.
The fact that most file formats that you use regularly are not plain text
might not be entirely obvious to you since you're probably not in the habit of
opening, say, an Excel spreadsheet with TextEdit.  Furthermore, the value of 
having data in plain text might not be immediately evident unless you've tried
to open an SPSS proprietary file in Excel (or vice versa) and found yourself
unable to do so.  The nice thing about plain text files, like .csv and .tab for
tabular data, is that they tend to be universally supported, consistently
structured regardless of origin, and easy to use with the aforementioned small
tools available on the command line.  You might lose some of the niceties that
certainly do come with binary data files, such as formatting and macros, but
you gain the ability to interchange technology without having to put in a lot
of extra work and, more importantly, the power to write simpler analytic tools
that leverage the programs already available from :term:`UNIX` without having
to have a lot of code that does nothing but deal with ambiguously interpreted
data.

The :term:`UNIX` way and its application to the :term:`shell` is both simple
and complex in the same sense that DNA is made up of a few amino acids,
phosphates and sugars but can generate essentially infinite biological variety.
Staying true to the basic precepts outlined above and learning how to use some
fundamental tools will give you a power of expression and control over your 
data that is otherwise very difficult to achieve.
