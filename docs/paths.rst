.. _paths:

Specifying filenames: paths
===========================

Filenames are often supplied to programs as arguments. For this reason, it is
essential to have a good understanding of how files are specified on the
command line. In Unix, a *path* is a term commonly used almost interchangeably
with *filename*, for reasons that will hopefully become clear in this section.

Files and folders
-----------------

Files and folders are stored on computers using a folder or directory
structure. For example, on a Windows computer, you might find a folder called
``MyDocuments``, within which there might be a ``data`` folder, and within that
some more folders, etc. Specifying a file or folder is simply a matter of
providing enough information to uniquely identify it.

The easiest way to visualise the directory structure is to think of it as a
*tree*. If you listed the contents of the root folder (the root of the tree),
you would find a number of other folders (the main branches). For example, one
of these folders would be the ``home`` folder, where user accounts are kept.
These folders might contain more folders (smaller branches) and/or files
(leaves), as illustrated below::

  /
  ├── bin/
  ├── boot/
  ├── home/
  │   └── donald/
  │       ├── data/
  │       │   ├── pilot/
  │       │   │   ├── subject1/
  │       │   │   │   └── dwi.mif
  │       │   │   ├── subject2/
  │       │   │   │   └── dwi.mif
  │       │   │   └── subject3/
  │       │   │       └── dwi.mif
  │       │   └── project/
  │       │       ├── analysis_script.sh
  │       │       ├── control1/
  │       │       │   ├── anat.nii
  │       │       │   └── dwi.mif
  │       │       ├── control2/
  │       │       │   ├── anat.nii
  │       │       │   └── dwi.mif
  │       │       ├── control3/
  │       │       │   ├── anat.nii
  │       │       │   └── dwi.mif
  │       │       ├── patient1/
  │       │       │   ├── anat.nii
  │       │       │   └── dwi.mif
  │       │       └── patient2/
  │       │           ├── anat.nii
  │       │           └── dwi.mif
  │       ├── Desktop/
  │       └── Documents/
  ├── usr/
  └── var/


Here, the folder called ``project`` can be uniquely identified by starting from
the root folder, going into ``home``, then ``donald``, ``data``, and finally
``project``.  This process can be thought of as specifying the *path* to the
file or folder of interest. In fact, this is the exact term used in Unix
jargon, essentially meaning 'an unambiguous file name'. Thus, specifying a
filename boils down to providing a unique, unambiguous *path* to the file.


.. NOTE::

  In this context, *directory* and *folder* are synonymous.

.. _abspath:

Absolute paths
--------------

On Unix, the root of the tree is always referred to using a simple forward
slash. Folders are referred to using their names, and are delimited using a
forward slash. For example, the full, absolute path to the ``project`` folder
in the figure above is::

  /home/donald/data/project/


This simply means: starting from the root folder (``/``), go into folder
``home``, then ``donald``, then ``data``, to find ``project``. This is an
example of an *absolute path*, because the start point of the path (the root
folder) has been specified within the filename. Thus, an absolute path must
start with a forward slash -- if it does not, it becomes a relative path,
explained below.

.. _wd:

The working directory
---------------------

When using the command line, you will often find that many of the files you are
manipulating reside in the same folder. It therefore makes sense to specify
this folder as your *working directory*, and then simply refer to each file by
its name, rather than its absolute path. This is exactly what the working
directory is, and it can save you a lot of unnecessary typing.

You can also think of it as your current location on the directory tree. For
example, if your current working directory is ``/home/donald/data/project``,
you can imagine that you have climbed up branch ``home``, then up branch
``donald``, then up branch ``data``, then up branch ``project``, and since
you're sitting there, you have direct access to all the files and folders that
spring from that branch.

Your working directory can be specified with the command :ref:`cd <cd>`, and
queried using the command :ref:`pwd <pwd>` (both described in :ref:`commands`).

.. _relpath:

Relative paths
--------------

Relative paths are so called because their starting point is the current
working directory, rather than the root folder. Thus, they are relative to the
current working directory, and only make sense if the working directory is also
known.

For example, the working directory might currently be
``/home/donald/data/project/``. In this folder there may be a number of other
files and folders. Since the file ``analysis_script.sh``  is in the current
working directory, it can be referred to unambiguously using the relative path
``analysis_script.sh``, rather than its full absolute path
``/home/donald/data/project/analysis_script.sh`` -- that's a lot less typing.

When you specify a relative path, it will actually be converted to an absolute
path, simply by taking the current working directory (an absolute path),
appending a forward slash, and appending the relative path you supplied after
that. For example, if you supply the relative path ``analysis_script.sh``, the
system will (internally) add up the current working directory
``/home/donald/data/project`` + ``/`` + ``analysis_script.sh`` to give the
absolute path.

Since the system simply adds the relative path to the working directory, you
can see that files and folders further along the directory tree can also be
accessed easily. For example, the ``project`` folder contains other folders,
``patient1``, ``patient2``, etc.  The file ``anat.nii`` within one of these
folders can be specified using the relative path ``patient1/anat.nii``
(assuming your current working directory is ``/home/donald/data/project``).

Of course, if you changed your current working directory, the relative path
would need to change accordingly. Using the same example as previously, if
``/home/donald/data/project/patient1`` was now your current working directory,
you could use the simpler relative path ``anat.nii`` to refer to the same file.

Special filenames
-----------------

A few shortcuts have special significance, and you should learn to use them, or
at least know of them. These are:

- ``~`` (tilda):

  shorthand for your home folder. For example, I could refer to the ``project``
  folder as ``~/data/project``, since my home folder is ``/home/donald``.

- ``.`` (single full stop): 

  the current directory. For example, if my current working directory is
  ``/home/donald``, I can refer to the ``project`` folder by specifying
  ``./data/project``, or even ``data/./project``. Although this may not look
  very useful, there are occasions when it becomes important (see examples
  below).

- ``..`` (double full stop): 

  the parent folder of the current directory. For example, if my current
  working directory is ``/home/donald/Desktop``, I can still refer to the
  ``data`` folder using the relative path ``../data``. This shortcut
  essentially means "drop the previous folder name from the path", or "go back
  down to the previous branch". Here are some alternative, less useful ways of
  referring to that same ``data`` folder, just to illustrate the idea::

    ../../donald/data
    ../Documents/../data
    ~/Desktop/../data

Using wildcards
---------------

There are a number of characters that have special meaning to the shell. Some
of these characters are referred to as *wildcards*, and their purpose is to ask
the shell to find all filenames that match the wildcard, and expand them on the
command line. Although there are a number of wildcards, the only one that will
be detailed here is the ``*`` character.

The ``*`` character essentially means 'any number or any characters'. When the
shell encounters this character in an argument, it will look for any files that
match that pattern, and append them one after the other where the original
pattern used to be. This can be better understood using some examples.

Imagine that within the current working directory, we have the files
``file1.txt``, ``file2.txt``, ``file3.txt``, ``info.txt``, ``image1.dat``, and
``image2.dat``. If we simply list the files (using the :ref:`ls <ls>` command),
we would see:

.. code-block:: console

  $ ls
  file1.txt   file2.txt   file3.txt
  image1.dat  image2.dat  info.txt

If we only wanted to list the text files, we could use a wildcard, and specify
that we are only interested in files that end with ``.txt``:

.. code-block:: console

  $ ls *.txt
  file1.txt   file2.txt   file3.txt   info.txt

We might only be interested in those text files that start with ``file``. In
this case, we could type:

.. code-block:: console

  $ ls file*.txt
  file1.txt   file2.txt   file3.txt

This use of wildcards becomes very useful when dealing with folders containing
large numbers of similar files, and only a subgroup of them is of interest. 

.. NOTE::

  It will be important later on to understand exactly what is going on here.
  Typing a command such as:

  .. code-block:: console

    $ ls *.txt

  does *not* instruct the :ref:`ls <ls>` command to find all files that match
  the wildcard. The wildcard matching is actually performed by the *shell*,
  before the :ref:`ls <ls>` command is itself invoked. What this means is that
  the *shell* takes the command you typed, modifies it by *expanding* the
  arguments, and invokes the corresponding command on your behalf. In the case
  above, this means that the command actually invoked will be:

  .. code-block:: console

    $ ls file1.txt file2.txt file3.txt info.txt

  In other words, your *single* argument containing a wildcard is expanded into
  multiple matching arguments by the *shell*.

  As another example, a command like:

  .. code-block:: console

    $ cp *.dat

  will be expanded to:

  .. code-block:: console

    $ cp image1.dat image2.dat

  which will cause ``image2.dat`` to be *overwritten* with the contents of
  ``image1.txt`` -- presumably causing irretrievable loss of data. In other
  words: think carefully about what you're typing...
