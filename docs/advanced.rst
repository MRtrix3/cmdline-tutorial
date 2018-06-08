.. _advanced:

Advanced usage
==============

So far, we have only covered the simplest aspects of what the shell can do. But
it can be used for far more than this, and can even be used as a full-blown
scripting language capable of running complex applications. While this level of
mastery is probably unnecessary for most users, there are a few advanced topics
that are very useful and worth covering in more detail, even in an introductory
document such as this. The interested reader is referred to more complete
guides, such as `this one <https://www.tldp.org/LDP/abs/html/index.html>`_.


Redirection
-----------

The *standard output* of commands, normally intended to be displayed on the
terminal, can be *redirected* to a file if needed, using the ``>`` symbol. 

For example, assuming that:

.. code-block:: console

  $ ls
  docs  html  LICENSE  README.md

The file listing can be *redictered* to a file, called ``listing.txt`` in the
example below:

.. code-block:: console

  $ ls > listing.txt

This creates the file specified, and the output normally shown by ``ls`` is not
visible on the terminal. It has however been stored in the ``listing.txt``
file, as we can verify with ``cat``:

.. code-block:: console
  
  $ cat listing.txt
  docs
  html
  LICENSE 
  README.md


This can also work in *append* mode, where the output of the command is
appended to the file, rather than overwriting its entire contents. 

For example:

.. code-block:: console
  
   $ app1 input output -options > log.txt
   $ app2 arg1 arg2 >> log.txt

will create the ``log.txt`` file in the first line, and record any output from
the ``app1`` command. The second line will then *append* its output to the log
file. 


Likewise, we can redirect the *standard input* to feed in the contents of a
file as input, rather than typing it in, using the ``<`` symbol. 

For example:

.. code-block:: console
  
   $ sort < myfile.txt

will feed the contents of ``myfile.txt`` to the ``sort`` command's *standard
input*.



Pipes
-----

This is a special type of redirection, where the *standard output* of one
command can be fed directly into the *standard input* of another, using the
``|`` symbol. Both commands run concurrently, with the second command able to
process the output of the first as soon as it is provided. This can be
incredibly useful to build compound commands. 

For example:

.. code-block:: console
  
   $ grep ERROR log.txt | sort | uniq 
   ERROR: error type one
   ERROR: input file not found
   ERROR: something bad happened

uses the ``grep`` command to find all lines in ``log.txt`` that contain the
character string ``ERROR``, then feeds those lines (which would normally be
displayed on the terminal) via the pipe as input for the ``sort`` command. This
sorts the lines in alphabetical order, and feed its output to the ``uniq``
command, which remove duplicates. The outcome of the full pipeline is a list of
all unique error messages logged in the ``log.txt`` file. 

Another particularly useful example is to capture the output from a command
expected to produce a lot of output, and browse through it at a more suitable
pace rather than seeing it fly past on the terminal. This can be done using the
command ``less`` (a paginator):

.. code-block:: console
  
   $ complex_process -verbose | less

This ability to quickly implememt otherwise non-trivial functionality is one of
the great strengths of the command-line. Unix is full of little tools like
``grep``, ``sort`` and ``uniq`` that are designed to operate on text and to be
daisy-chained in this manner.


Conditional execution
---------------------

While BASH provide its own ``if`` statement for more complex situations, it
also offers a simple construct to allow execution of one command based on the
success or failure of another, using the ``&&`` and ``||`` operators
respectively. 

For example:

.. code-block:: console
  
   $ myapp args -options || echo "myapp failed to run!" >> log.txt

will record the fact that the ``myapp`` command has failed to the ``log.txt``
file. 

On the other hand:

.. code-block:: console

    $ stage1 -options inputdata/ tmpdata/ && stage2 tmpdata/ outputdata/

will only run the ``stage2`` command if the ``stage1`` executable has completed
successfully (useful if the data produced by the first command is to be
processed by the next one). 


Variables
---------

It is often useful to store information in variables. For instance, you might
want to use a long and complicated filename often, and rather than typing it in
every time you need it, you could use a variable. Variables are assigned using
the ``=`` symbol (beware: no spaces around it), and retrieved (dereferenced)
using the ``$`` symbol.

For example:

.. code-block:: console

    $ logfile=/some/complicated/location/myapp/logs/run1.txt
    $ myapp input intermediate > $logfile
    $ otherapp intermediate output >> $logfile
    ...

The variable ``logfile`` is set to the filename of the logfile, and the output
of all subsequent commands is then redirected to that file (see above). 


Iterating with for loops
------------------------

It is often required to perform the same command for a number of files. This
can be achieved simply and effectively with a ``for`` loop, like this:

.. code-block:: console

    $ for item in logs/run*.txt; do grep OUTPUT $item; done

This will find all lines that contain the token ``OUTPUT`` in the logfiles
stored in the ``logs/`` folder that match the filename ``run*.txt``, and print
them on the terminal. 

What actually happens here is that a variable ``item`` is used to store each
token listed after the ``in`` keywords (until the end of line or ``;`` symbol),
and the command(s) between the ``do`` and ``done`` keywords are then executed
for each token. The current value of the token can then be retrieved within the
loop by dereferencing it like any other variable, using the ``$`` symbol.

Note that the above does not need to be all on the same line. In practice, lines
can be broken wherever the ``;`` was used in the example above:

.. code-block:: console

    $ for item in logs/run*.txt
    > do
    >   grep OUTPUT $item 
    > done


Parameter substitution
----------------------

There are certain operations that can be performed on variables at the point
where they are being dereferenced. Of these, the most useful are probably the
ability to strip a suffix or prefix. This is done using a syntax like
``${var#prefix}`` or ``${var%suffix}``. This is most useful in scripts and when
combined with ``for`` loops.

For example:

.. code-block:: console

    $ for data in *.dat
    > do 
    >   process $data ${data%.dat}.out > ${data%.dat}.log
    > done

will run the ``process`` command on all files in the current folder that end
with the ``.dat`` suffix, and pass as second argument the same filename with
the ``.dat`` suffix stripped and replaced with the ``.out`` suffix. The output
of each command will individually be stored in log files, each with the
``.log`` suffix. If the current folder contained the files:

.. code-block:: console

    $ ls
    backup/ final.dat original.dat parameters.txt trial2.dat 

Then the commands actually run will be:

.. code-block:: console

    $ process final.dat final.out > final.log
    $ process original.dat original.out > original.log
    $ process trial2.dat trial2.out > trial2.log


There are many other types of parameter substitutions possible, see the
`relevant documentation
<https://www.tldp.org/LDP/abs/html/parameter-substitution.html>`_ for details. 
