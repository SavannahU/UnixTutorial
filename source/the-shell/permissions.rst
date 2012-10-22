A Multiuser System
==================

:term:`UNIX` was originally designed as a time-sharing operating system,
meaning it allowed multiple users to be signed into a single machine and run
programs on it simultaneously.  Users had login names and were categorized into
groups according to what they were allowed to do on the system.  Each human
user could be assigned a home directory and manage there own files.  This model
has stayed with us, as anyone who has ever used a Windows or OS X system can
attest.

To see who is currently signed into your computer, you can use the ``users``
command.  The ``groups`` command tells you which groups a user belongs to.  If
a username is not provided, the output is a list of the groups you belong to.

.. code-block::

   $ users
   bentrofatter     someoneelse
   $ groups
   staff            wheel           coders
   $ groups someoneelse
   staff

.. note::

  Typing this command on your laptop is unlikely to do anything other
  than list your own username since it's unlikely that anyone other than you is
  signed into the system, but trying it out on a server like cronusx would
  probably get you a short list of names.

If you're not sure who you are for some reason, you can type ``who am i`` to
get your own username.

Permissions
===========

Central to organization scheme is the concept of the :term:`permission`.
Permissions are attributes of files and directories that indicate who is
allowed to access them and how.  Permissions are divided into three
categories (``read``, ``write``, and ``execute``) and apply to three groups
(``user``, ``group``, and ``other``).  To view a file or directory's
permissions, request a long listing:

.. code-block::

   $ ls -lh
   drwxr-xr-x   15 bentrofatter  staff   510B Apr 20 16:54 c/
   drwxr-xr-x   15 bentrofatter  staff   510B Apr 20 16:54 python/
   -rwxr-xr-x   15 bentrofatter  staff   510B Apr 20 16:54 myscript.sh

The first column of the output can be parsed as follows:

   - The first character indicates the file type.  Here, ``d`` tells us the
     item is a directory, and ``-`` tells us it is a normal file.
   - The next three characters describe the user's (or owner's) permissions.
     Here, the owner of the file is the ``bentrofatter``, as shown in the third
     column of the output.  ``r`` means the user can read the file, ``w``
     means the user can write or modify the file, and ``x`` means the user
     can execute the item.  If the user lacks the individual permission, ``-``
     will appear instead of the letter.
   - Characters 5 through 7 are the group permissions.  Anyone in the
     ``staff`` group read and execute all three items, but they cannot change
     them.
   - The last three characters are the other permissions.  They tell us that
     anyone can read and execute the three items.

.. note::

   The ``x`` permission means slightly different things depending on the file
   type.  A file for which you have the ``x`` permission can be run as a
   program.  If the item is a directory, ``x`` means that you can navigate into
   it with the ``cd`` command and use ``ls`` to list its contents.

Super Users
===========

Not possessing a certain permission on a file might not be a problem depending
on how your user account is set up.  In fact, sometimes it's better to make it
so that only, say, an administrator, can change system configuration files that
could render the computer unusable.  For the situation wherein you want to
modify a start-up file that restricts writes to a system administrator, if
you're in an administrator group, you can probably use ``sudo`` to temporarily
upgrade your credentials to those of a term:`superuser` and make the change.

.. warning::
   ``sudo`` privileges are incredibly powerful.  A command executed by ``sudo``
   will be final.  Great care should always be taken to ensure that you
   have correctly and accurately conveyed exactly what you want the computer to
   do when using this command

.. code-block::

   $ # With great power comes great responsibility
   $ # DO NOT EXECUTE THIS COMMAND!
   $ sudo rm -rf /
   Password: [Enter password]
   $ # Congratulations, you've just wiped out your entire system

You can also use ``su`` to switch user accounts.  This can be convenient if you
are running a program that requires a dedicated user, like a full-featured SQL
database server, and want to interact with it.

.. code-block::

   $ who am i
   bentrofatter
   $ su -
   Password: [Enter root password]
   % who am i
   root
   % exit
   $ su postgres
   Password: [Enter root password]
   $ who am i
   postgres
   $ exit
   $ who am i
   bentrofatter

Adjusting Permissions
=====================

If you're the owner of a file or acting as the superuser, you can modify its
permissions and ownership attributes with ``chmod``, ``chown``, and ``chgrp``.

.. code-block::

   $ ls -lh
   -rwxr-xr-x   15 bentrofatter  staff   510B Apr 20 16:54 myscript.sh
   $ whoami
   bentrofatter
   $ # Now we want to allow people in the staff group to modify the file and
   $ # prevent everyone else from being able to execute it.
   $ chmod g+w,o-x myscript.sh
   $ ls -lh
   -rwxrwxr--   15 bentrofatter  staff   510B Apr 20 16:54 myscript.sh
   $ sudo chown someoneelse myscript.h
   $ ls -lh
   -rwxrwxr--   15 someoneelse   staff   510B Apr 20 16:54 myscript.sh
   $ sudo chgrp wheel myscript.h
   $ ls -lh
   -rwxrwxr--   15 someoneelse   wheel   510B Apr 20 16:54 myscript.sh
   $ sudo chown bentrofatter:staff myscript.h
   $ ls -lh
   -rwxrwxr--   15 bentrofatter  staff   510B Apr 20 16:54 myscript.sh

Notice that in the last line of the previous example, we changed both the owner
and the group of ``myscript.sh`` by appending the group name to the argument 
passed to chown.
