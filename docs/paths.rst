.. _paths:

Specifying filenames
====================

Filenames are often supplied to programs as arguments. For this reason, it is
essential to have a good understanding of how files are specified on the
command line.

Files and folders
-----------------

Files and folders are stored on computers using a folder or directory
structure. For example, on a Windows computer, you might find a folder called
'MyDocuments', within which there might be a 'data' folder, and within that
some more folders, etc. Specifying a file or folder is simply a matter of
providing enough information to uniquely identify it.

The easiest way to visualise the directory structure is to think of it as a
tree. If you listed the contents of the root folder (the root of the tree), you
would find a number of other folders (the main branches). For example, one of
these folders would be the home folder, where user accounts are kept. These
folders might contain more folders (smaller branches) and/or files (leaves).
This is illustrated in the right hand side figure.

Here, the folder called 'donald' can be uniquely identified by starting from
the root folder, going into home, then dali, then a, to find donald. This
process can be thought of as specifying the path to the file or folder of
interest. In fact, this is the exact term used in Unix jargon, essentially
meaning 'an unambiguous file name'. Thus, specifying a filename boils down to
providing a unique, unambiguous path to the file.

Absolute paths
--------------

On Unix, the root of the tree is always referred to using a simple forward
slash. Folders are referred to using their names, and are delimited using a
forward slash. For example, the full, absolute path to the folder 'donald' in
the figure above is::

  /home/donald


This simply means: starting from the root folder ('/'), go into folder home,
then dali, then a, to find donald. This is an example of an absolute path,
because the start point of the path (the root folder) has been specified within
the filename. Thus, an absolute path must start with a forward slash - if it
does not, it becomes a relative path, explained below.  

.. _wd:

The working directory
---------------------

When using the command line, you will often find that many of the files you are
manipulating reside in the same folder. It therefore makes sense to specify
this folder as your working directory, and then simply refer to each file by
its name, rather than its absolute path. This is exactly what the working
directory is, and it can save you a lot of unnecessary typing.

You can also think of it as your current location on the directory tree. For
example, if your current working directory is /home/dali/a/donald, you can
imagine that you have climbed up branch home, then up branch dali, then up
branch a, then up branch donald, and since you're sitting there, you have
direct access to all the files and folders that spring from that branch.

Your working directory can be specified with the command cd, and queried using
the command `pwd`_ (both described below).

Relative paths
--------------

Relative paths are so called because their starting point is the current
working directory, rather than the root folder. Thus, they are relative to the
current working directory, and only make sense if the working directory is also
known.



For example, the working directory might currently be /home/dali/a/donald. In
this folder there may be a number of other files and folders, as illustrated in
the right hand side figure. Since the folder data is in the current working
directory, it can be referred to unambiguously using the relative path data,
rather than its full absolute path /home/dali/a/donald/data. As you can see,
this requires considerably less typing.

When you specify a relative path, it will actually be converted to an absolute
path, simply by taking the current working directory (an absolute path),
appending a forward slash, and appending the relative path you supplied after
that. For example, if you supply the relative path data, the system will add up
'/home/dali/a/donald' + '/' + 'data' to give the absolute path.

Since the system simply adds the relative path to the working directory, you
can see that files and folders further along the directory tree can also be
accessed easily. For example, the Desktop folder contains another folder, abc.
This folder can be specified using the relative path Desktop/abc (assuming your
current working directory is /home/dali/a/donald).

Of course, if you changed your current working directory, the relative path
would need to change accordingly. Using the same example as previously, if
/home/dali/a/donald/Desktop was now your current working directory, you could
use the simpler relative path abc to refer to the same folder.

Special filenames
-----------------

A few shortcuts have special significance, and you should learn to use them, or
at least know of them. These are:

- ``~`` (tilda):
  shorthand for your home folder. For example, I could refer to the abc folder using ~/Desktop/abc, since my home folder is /home/dali/a/donald.
- `.` (single full stop): 
  the current directory. For example, if my current working directory is
  /home/dali/a/donald, I can refer to the abc folder by specifying
  ./Desktop/abc, or even Desktop/./abc. Although this may not look very useful,
  there are occasions when it is helpful (see here for example).

- .. (double full stop): 
  the parent folder of the current directory. For example, if my current
  working directory is /home/dali/a/donald/Desktop, I can still refer to the
  data folder using the relative path ../data. This shortcut essentially means
  "drop the previous folder name from the path", or "go back down to the
  previous branch". Here are some alternative, less useful ways of referring to
  the same folder, just to illustrate the idea::
    ../../donald/data
    abc/../../data
    ~/Desktop/../data

Using wildcards
--------------

There are a number of characters that have special meaning to the shell. Some
of these characters are referred to as wildcards, and their purpose is to ask
the shell to find all filenames that match the wildcard, and expand them on the
command line. Although there are a number of wildcards, the only one that will
be detailed here is the * character.

The ``*`` character essentially means 'any number or any characters'. When the
shell encounters this character in an argument, it will look for any files that
match that pattern, and append them one after the other where the original
pattern used to be. This can be better understood using some examples.

Imagine that within the current working directory, we have the files file1.txt,
file2.txt, file3.txt, info.txt, image1.dat, and image2.dat. If we simply list
the files (using the ls command), we would see:

.. code-block:: console

  $ ls
  file1.txt   file2.txt   file3.txt 
  image1.dat  image2.dat  info.txt 

If we only wanted to list the text files, we could use a wildcard, and specify
that we are only interested in files that end with .txt:

.. code-block:: console

  $ ls *.txt
  file1.txt   file2.txt   file3.txt   info.txt

We might only be interested in those text files that start with file. In this
case, we could type:

.. code-block:: console

  $ ls file*.txt
  file1.txt   file2.txt   file3.txt

This use of wildcards becomes very useful when dealing with folders containing
large numbers of similar files, and only a subgroup of them is of interest. See
the here for more relevant examples.

