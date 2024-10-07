.. _inner_workings:

What's going on under the hood?
===============================

While the description given so far is sufficient to get started, you will
rapidly encounter situations where things don't work as you might expect. In
these cases, it really helps to understand how what you type translates into
action, so that you can usefully reason about what's going on. The main thing
to get to grips with is what the various components are and what their specific
roles are.



The terminal
------------

The terminal's role is purely one of user *input and output*: it displays the
output produced by the shell or the command currently executing within it, and
receives input from the user via the keyboard (and potentially the mouse too)
to relay it to the shell or the command currently executing. In both cases,
the input and ouput consists merely of a stream of *bytes*. Some of these
characters correspond to printable characters, others to non-printable
characters (e.g. carriage return, newline, ...), and yet others to
*sequences* that might carry special meaning (e.g. the `VT100 Terminal Control
sequences <http://www.termsys.demon.co.uk/vtansi.htm>`_). You might see some of
these garbled sequences of characters when output intended for the terminal is
instead written to file.

The shell
---------

The role of the shell is to *interpret* the input provided by the user, and
provide output in response to be displayed by the terminal. The most obvious
output produced by the shell is the *prompt*, which typically looks like:

.. code-block:: console

  username@machine:path $

This is simply a string of characters produced by the shell, instructing the
terminal to display this to the user, in order to indicate to the user that it
is ready to receive input. The convention on modern systems to display the
username, machine and path is entirely optional, but you'll quickly find it
very useful as you navigate around the folders on your system, or if you start
using the terminal to remotely access other systems (e.g. a high performance
computing cluster). 

The input that you type at this point is then passed to the shell, which
decides what to do with it. In most cases, it will simply *echo* the characters
you've entered so you can see what you've typed on the terminal. But some of
your input might already start to be interpreted, for example ``Ctrl-A`` will
bring the cursor to the start of the line, ``Ctrl-E`` back to the end (along
with the Home & End keys, on a well-configured system). The Left & Right
arrow keys will allow you to move the cursor along to edit your command. The
Up & Down arrow keys will bring up previous commands that you've typed. These
are all examples of the shell *interpreting* your input and responding
accordingly. 

.. NOTE::

  The shell's handling of your input is typically managed by the
  `readline library
  <https://tiswww.case.edu/php/chet/readline/rltop.html>`_. It is highly
  configurable, and the actions tied to each of these keys can be modified in
  many ways. This tutorial describes the actions you'd
  expect on most Unix systems by default, but don't be surprised if things
  behave slightly differently on other systems. 

Once you've typed in your command, you will typically just hit the Return key
to execute it. In the simplest case, when the shell receives this instruction,
your input will simply be broken up into a list of distinct 'entries', each
separated by spaces (although it entirely possible to have entries with spaces,
as :ref:`detailed earlier <spaces>`). In other cases, it will also perform any
additional interpretation required (e.g. for wildcard expansion, variable
substitution, etc). These entries are typically called the command-line
*arguments*.

The first (and possibly only) entry in this list is expected to be the command
name, and the shell will try to locate the corresponding executable (unless
it's a special shell built-in command, such as ``cd``, ``pwd``, ``echo``,
``export``, ...). Executables are just regular files that happen to have a
special flag set to signal the fact that they can be executed (you can see this
using ``ls -l``
-- look for the ``x`` in the permissions field). Some of these executables might
be human-readable text, in which case they tend to be called *scripts*. Other
executables consist of machine code: the raw instructions executed by the CPU
-- these are often referred to as *binaries*. Regardless, the shell will need
to locate this file so that it can execute the code it contains. 

To locate the executable, the system will typically look through the list of
folders contained in the ``PATH`` environment variable. You can query this list
yourself by typing ``echo $PATH`` (it's a colon-separated list of folder
paths). This is why it is often necessary to modify the ``PATH`` when installing
non-standard software: this is how the shell 'knows' where to find these new
commands. It is also possible to provide the exact location of the command, by
invoking the command via the :ref:`absolute path <abspath>` to its executable
(e.g.  ``/usr/bin/whoami``, rather than just ``whoami``), or a :ref:`relative
path <relpath>` to it (e.g. ``./bin/myexecutable``); this can come in handy if
you just need to execute your script from a location not in your ``PATH``,
without having to modify the ``PATH`` explicitly.


The command
-----------

At this point, the shell is now ready to actually run your command. It will
place your list of command-line arguments in a special location, and launch the
executable itself. The executable now takes over the terminal: any input
you provide will be send to it, and any output it produces will be
displayed in the terminal. It also has access to your list of command-line
arguments, and will perform its actions based on what it finds there. 

It is important to note that how the command interprets the arguments it's been
given is *entirely* up to the command itself -- or rather, up to the command's
developer. This means that different commands will adopt slightly different
conventions as to how their arguments are expected to be provided. Some
commands expect all :ref:`command-line options <cmdopts>` to be provided before
any of the main arguments, and others don't. Some commands expect options to be
provided in short form (e.g. ``ls -l``), other in long form (e.g. ``git
--help``), others will accept a mixture of both (some short, some long), and
yet others may accept both forms for the *same* option (e.g. ``cp -f`` is
equivalent to ``cp --force``). There are accepted conventions, but this by no
means implies that all developers will abide by them. 

Another potential source of information that the executable will have access to
is via *environment variables* (the ``PATH`` is one such variable). This allows
the user to set a variable, which the command can query as it is executing.
This might be to specify the location of important configuration files, or the
number of threads that the application is expected to run, etc. In general,
this is reserved for information that is unlikely to change very often,
allowing the user to set this information once within the shell's startup file
(typically ``~/.bashrc``), and no longer have to worry about it. For example,
adding this line in ``~/.bashrc`` means that applications can query this
variable at runtime:

.. code-block:: bash

  export MYAPP_DIR=/usr/local/myapp/configfiles




Implications
------------

Once you appreciate the way these components fit together, various aspects of
the system may start to make more sense. For instance, there are many terminal
programs available, from raw VT100 terminals, to various graphical ones, all
with various levels of functionality (e.g. multiple tabs, split display,
transparent background, unlimited scrollback, etc.).
But within these various terminals, you will generally be running the same
*shell*, and it'll behave the same way no matter which terminal it is running
in. However, if you log into a different system, you may find subtle
differences in the way it behaves (different prompts, some keyboard shortcuts
that work differently, etc.). You may also find that the default shell you are
logging in with is different on different system: some HPC systems are
configured with the C shell as the default, and its syntax can be quite
different. But at heart, the concepts outlined here will be the same. 

It's also important to understand what the shell will do to your command as
it's interpreting it, and what arguments this will translate to once passed to
the executable itself. A useful trick here is to prefix your intended command
with ``echo``: the shell will perform all the variable substitute and wildcard
expansion that it normally would, but because ``echo`` is now the command, all
this will now simply be printed on the terminal. For example:

.. code-block:: console

  $ echo cp files*.txt destination/
  cp file.txt file_1.txt file_2.txt file_3.txt destination/

Note that the command itself is not executed; it is merely displayed as it
would have been interpreted by the shell. This might come in handy to see that
the ``files.txt`` file will also be copied, when that might not have been
intended.  This trick is particularly useful when performing more advanced
substitutions, as you'll see in the :ref:`advanced` page.


