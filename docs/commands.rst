.. _commands:

Basic commands
==============

Below is a list of commands that are very commonly used. Of these, `cd`_ and `ls`_
are essential to using the command line, and you should be familiar with them.

Each command described below has a syntax line, which gives a brief description of how it
should be used. The first item on the line is always the name of the command,
followed by a number of arguments. Arguments given in square brackets are
optional and can be omitted. If an argument is followed by three dots ('...'),
it means that any number of that type of argument can be provided, and each
will be processed in turn. In addition, any options that are of particular
interest are listed in the corresponding section for each command. 

``pwd``: print working directory
-----------------------------

.. code-block:: console

  $ pwd

Print the current working directory onto the screen.


``cd``: change working directory
-----------------------------

.. code-block:: console

  $ cd [folder]

Change the current working directory. If no folder is specified, the current
working directory will be set to the home folder. Otherwise, it will be set to
``folder``, assuming that it is a valid path.  

Note that in this command, the token ``-`` has special meaning when passed
instead of ``folder``: it refers to the previous working directory. This can be
useful to rapidly change back and forth between folders, e.g.:

.. code-block:: console

  # our current working directory is /home/donald/Documents

  $ cd ../data/project/patient2
  ...         # current working directory is now /home/donald/data/project/patient2
  ...         # do something useful

  $ cd -      # switch back to /home/donald/Documents when you're done
  


``ls``: list files
---------------

.. code-block:: console

  $ ls [option]... [target]...

List files. If no target is specified, the contents of the current working
directory are listed. Otherwise, the files specified are listed in the order
provided, assuming that they are valid paths. If a target is a folder, its
contents will be listed.

Options:

- ``-a``
  
  list all files, including hidden files (hidden files are those that start with a full stop '.').

- ``-l``

  provide a long listing, including file size, ownership, permissions, and modification date.


``cp``: copy files or folders
--------------------------

.. code-block:: console

  $ cp [option]... source target
  $ cp [option]... source... folder

In its first form, copy the file ``source`` to create the file ``target``, assuming
both are valid paths. You should be aware that in general, if the file
``target`` already exists, it will be overwritten (although this behaviour can
be modified through an `alias`_).

The second form is used to copy one or more ``source`` files into the
``target`` folder. In this case, target must be a pre-exising folder. Each
newly created file will have the same base name as the original.

Options:

- ``-i``

  ask for confirmation before overwriting any files (default for MR2).

- ``-r``

  recursive copy: use this option to copy an entire folder and its contents.


``mv``: move/rename files or folders
---------------------------------

.. code-block:: console

  $ mv [option]... source target
  $ mv [option]... source... folder

In its first form, move or rename the file (or folder) ``source`` to ``target``,
assuming both are valid paths. Note that renaming is essentially equivalent to
moving the file to a different location, if ``source`` and ``target`` reside in
different folders.

The second form is used to move one or more ``source`` files into the
``target`` folder. In this case, ``target`` must be a pre-existing folder.

Options:

- ``-i``

  ask for confirmation before overwriting any files (default for MR2).


Examples of typical command use
-------------------------------

Below are some examples of commands in typical use, illustrating some of the
concepts explained in this document. To fully understand the examples, you may
need to refer back to the sections on `paths`_, using special filenames, or using wildcards.

- To change your current working directory to its parent folder (move one branch down the directory tree):

  .. code-block::console
  
    $ cd ..


- To change your current working directory from whatever it was to the ``data``
  folder in your home directory:

  .. code-block:: console

    $ cd ~/data

- To list the headers for all images (with the ``.png`` suffix) whose filename start
  with ``ns`` from the ``controls`` folder:
  
  .. code-block:: console

  $ ls controls/ns*.png

- To move the file ``data.mat``, residing in the current working directory,
  into the parent folder of that directory:

  .. code-block:: console

  $ mv data.mat ..

  
- To copy the file ``info.txt`` from the folder ``important`` into the current working directory:

  .. code-block:: console

  $ cp important/info.txt .

- To copy all shell script files from the ``data`` folder in your home
  directory into the ``scripts`` folder in the current working directory:

  .. code-block:: console
  
  $ cp ~/data/*.hdr scripts/

- To copy all images for study 3 of patient *Joe Bloggs* from the ``/data``
  folder into the current working directory:

  .. code-block:: console

  $ cp /data/bloggsj_010203_123/*-3-*.ima .


.. _alias: http://linuxcommand.org/lc3_man_pages/aliash.html


