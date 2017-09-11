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

An important point to note is that Unix is case-sensitive: a file called Data
is not the same as a file called data.

The command line prompt can vary from system to system, but usually consists of
some text followed by a character such as ``$``, ``#`` or ``>``. In many
systems, it might look like this: 

.. code-block:: console

  username@machine:path $

where username corresponds to your own, machine corresponds to the name of the
system you are currently logged onto, and path indicates your current working
directory. Note that this can be configured differently, do not be surprised or
alarmed if it looks different on your system.

If the prompt ends with a ``#``, this most likely means that you are logged in as
root (the super-user). If this is the case, please log out before anything bad
happens - unless you know what you are doing.

.. NOTE::

  A session will typically involve two separate programs:

  - the shell: the program that actually interprets the commands that are typed
    in, and takes the appropriate action. There are a number of different
    shells available, each with their own syntax rules. The default shell in
    MR2 is the Bourne Again Shell (bash).

  - the terminal: the program responsible for displaying the output of the
    shell, and what the user types in. There are a number of different
    terminals available, each with different levels of sophistication. In MR2,
    you will probably be using the konsole terminal program.

  This leads to a bewildering array of combinations between the different
  shells and terminal programs available. Thankfully, in MR2, you will almost
  always be using konsole as the terminal and bash as the shell, and this is
  what you should learn how to use.


How to access the command line
------------------------------

There are many ways to start a command line session. In MR2, the easiest way to
start a session is by launching a program called konsole (the default KDE
terminal). This can be done in a number of ways:

Click on the shell icon, which should be located either on the toolbar at the
bottom of the screen, or on your desktop. It should look something like that
shown on the right. Bear in mind that the icons on your desktop will depend on
your current theme, etc. and might therefore differ substantially.  Find the
'konsole', 'shell', or 'terminal' entry in the applications menu, which you
should find located on the extreme left of the toolbar at the bottom of your
screen. An example of what this might look like is shown on the right. Note
that your menu structure might look significantly different from this, and you
might need to look in the other subfolders to find it.

Click on the "MR2 applications" icon. This should be on your desktop, and look
like that shown here. This will open a window with a number of other icons, one
of which should be the terminal icon.

Launch the konsole program directly using the "Run Command" utility. This can
be accessed either via the applications menu (see above), or directly by
pressing the Alt+F2 key combination. This should open a small window into which
commands can be typed directly (this is essentially a mini command line). The
terminal program can then be launched by entering konsole in the box provided.



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
name, and the name of the file to be created. The program to do this, cp,
expects the first argument to be the name of the file to be copied, and the
second argument to be the name of the duplicate file to be created. If it does
not find two arguments, it will produce an error message.

Note that some programs may accept a number of arguments, but use default
values if they are omitted (example of these are cd and ls). Other programs may
accept variable numbers of arguments, and process each argument in turn.


Command line options
....................

There is a special type of argument that you might encounter, often referred to
as a command line option or switch. The purpose of these optional arguments is
to modify the behaviour of the program in some way. Command line options always
start with a minus symbol to distinguish them from normal arguments. For
example, passing the appropriate option (-l) to the ls command when listing the
files in the current folder will produce a longer listing, including
information such as file size and modification time as well as the file names
normally output.

Command line options can also require additional arguments. In this case, these
additional arguments should be entered immediately after the option itself.

Below are some typical command examples.  (the ``$`` symbol indicates the
prompt):

.. code-block:: console

  $ command name argument -option additional argument required by a preceeding option


- To list the contents of the current working directory:

  .. code-block:: console
  
    $ ls
  
- To list the contents of the current working directory, along with the file
  permissions, owner, size and modification date:
  
  .. code-block:: console
  
    $ ls -l
  
- To copy the file source, creating the file dest:
  
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

As previsouly mentioned, the command actually typed in will first be split
using spaces as boundaries. In certain cases, it may be necessary to provide
arguments that contain spaces within them. A common example of this is when
file names contain spaces (note that this should be avoided, especially since
SPM is often not able to deal with these). This is obviously a problem, since
an argument with a space in it will be interpreted as two separate arguments.
To supply an argument with a space in it, use the following syntax.

As an example, if we need to supply the argument "argument with spaces" to some
command, we can use any of the following:

-  ``'argument with spaces'``
- ``"argument with spaces"``
- ``argument\ with\ spaces``

In the last example, the backslash character tells the shell to ignore the
subsequent space character and treat it as a normal character.

