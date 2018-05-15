.. _introduction:

Introduction
============

The command line is simply a way of passing instructions to the computer. It is
a text-only interface, and commands are supplied to the shell by typing them in
and pressing the return key. Any output produced by these commands is printed
out on the same text interface. Although daunting at first, the command line
provides a very rich interface to the available functionality, and it is worth
learning how to use it.

The command line interface will print out a prompt, after which commands can be
typed. After the command has completed, the prompt will be printed out again,
indicating that the shell is ready to accept new commands.

An important point to note is that Unix is case-sensitive: a file called
``Data`` is not the same as a file called ``data``.

The command line prompt can vary from system to system, but usually consists of
some text followed by the ``$`` character. In many systems, it might look like
this: 

.. code-block:: console

  username@machine:path $

where ``username`` corresponds to your own username, ``machine`` corresponds to
the name of the system you are currently logged onto, and ``path`` indicates your
current :ref:`working directory <wd>`. Note that this can be configured
differently, do not be surprised or alarmed if it looks different on your
system.

.. WARNING::

  If the prompt ends with a ``#``, this most likely means that you are logged
  in as root (the super-user or Administrator). If this is the case, please log
  out before doing anything irreversible - unless you know what you are doing.

.. NOTE::

  A session will typically involve two separate programs:

  - the **shell**: the program that actually interprets the commands that are typed
    in, and takes the appropriate action. There are a number of different
    shells available, each with their own syntax rules. The default shell in
    most Linux distributions is the Bourne Again Shell (bash). This is also the
    default on macOS and MSYS2 (used in Windows installations), and for this
    reason is the default assumed in this guide. 

  - the **terminal**: the program responsible for displaying the output of the
    shell, and what the user types in. There are a number of different
    terminals available, each with different levels of sophistication. 

  This leads to a bewildering array of combinations between the different
  shells and terminal programs available. Nonetheless, the general principles
  are fairly universal. 


How to access the command line
------------------------------

There are many ways to start a command line session, depending on your OS and
desktop environment. 

- **GNU/Linux**

  Different Linux distributions include different `desktop environments <de>`_.
  For instance `Ubuntu <https://www.ubuntu.com/>`_ installations will typically be
  running `Unity <https://unity.ubuntu.com/>`_, `Red Hat
  <https://www.redhat.com/>`_ and
  derivatives will typically be running `GNOME <https://www.gnome.org/>`_, while
  `SuSE <https://www.suse.com/>`_ would typically come with `KDE
  <https://www.kde.org/>`_. 
  
  Given the many different possible configurations, it is impossible to give
  specific instructions for each case. Nonetheless, in general you ought to be
  able to identify an application called 'Terminal' or similar in the desktop's
  main menu.

- **macOS**

  You will find the 'Terminal' application within the *Utilities* folder in the
  *Applications* folder in the Finder.

- **Windows (MSYS2)**

  You will need to use the MSYS2 *MinGW-w64 Win64* shell, either by
  double-clicking on the shortcut (on the Desktop or in the Start menu), or by
  searching for it (hit the Windows key and start typing 'MSYS2' -- it should
  show up in the list of applications displayed). 

Basic Structure of a command
----------------------------

In the simplest form, a command consists of a line of text, which is first
broken up by splitting it using spaces as boundaries. The first 'word' must be
the command name, corresponding to a real program to be executed. All other
'words' will eventually be passed to the program as arguments that should
provide it with enough information to perform what is intended.

Command line arguments
......................

To perform whatever action is required, the program that is being run may need
some additional information. This information can be supplied to the program
via the arguments that follow it in the command. Essentially, arguments are a
series of 'words' specified in the right order so that the program can
understand what is required of it.

For example, you need to supply two arguments to copy a file: the original file
name, and the name of the file to be created. The program to do this, :ref:`cp <cp>`,
expects the first argument to be the name of the file to be copied, and the
second argument to be the name of the duplicate file to be created. If it does
not find two arguments, it will produce an error message.

Note that some programs may accept a number of arguments, but use default
values if they are omitted (example of these are :ref:`cd <cd>` and :ref:`ls
<ls>`). Other programs may accept variable numbers of arguments, and process
each argument in turn.


Command line options
....................

There is a special type of argument that you might encounter, often referred to
as a command line option or switch. The purpose of these optional arguments is
to modify the behaviour of the program in some way. Command line options always
start with a minus symbol to distinguish them from normal arguments. For
example, passing the appropriate option (``-l``) to the :ref:`ls <ls>` command
when listing the files in the current folder will produce a longer listing,
including information such as file size and modification time as well as the
file names normally output.

Command line options can also require additional arguments. In this case, these
additional arguments should be entered immediately after the option itself --
see the examples below.

Examples
........

Below are some typical command examples.  (the ``$`` symbol indicates the
prompt):

- To list the contents of the current working directory:

  .. code-block:: console
  
    $ ls
  
- To list the contents of the current working directory, along with the file
  permissions, owner, size and modification date:
  
  .. code-block:: console
  
    $ ls -l
  
- To copy the file ``source``, creating the file ``dest``:
  
  .. code-block:: console
  
    $ cp source dest
  
- To convert image ``source.mif`` (*MRtrix* format) into image ``dest.nii`` (NIfTI format):
  
  .. code-block:: console
  
    $ mrconvert source.mif dest.nii

- To convert image ``source.mif`` into image ``dest.nii``, changing the voxel
  size to 1.25 x 1 x 1 mm and changing the datatype to 32-bit floating-point:
  
  .. code-block:: console
  
    $ mrconvert source.mif -vox 1.25,1,1 -datatype float32 dest.nii
  

Dealing with spaces in arguments
--------------------------------

As previously mentioned, the command actually typed in will first be split up
into *tokens* using spaces as delimiters. In certain cases, it may be necessary
to provide arguments that contain spaces within them. A common example of this
is when file names contain spaces (note that this should be avoided, especially
since other programs and scripts often have issues dealing with these).  This
is obviously a problem, since an argument with a space in it will be
interpreted as two separate arguments.  To supply an argument with a space in
it, use the following syntax.

As an example, if we need to supply the argument "argument with spaces" to some
command, we can use any of the following:

-  ``'argument with spaces'``
- ``"argument with spaces"``
- ``argument\ with\ spaces``

In the last example, the backslash character tells the shell to ignore the
subsequent space character and treat it as a normal character.


