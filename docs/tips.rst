.. _tips:

Useful tips
===========

The commands that you might type in can get quite long, and using the command
line can quickly become laborious. You are strongly encouraged to read the tips
below, which can not only save you a lot of unnecessary typing, but also help
you avoid making mistakes and typos. Some of these features may not be
immediately obvious, so you are advised to spend a little time familiarising
yourself with them.

Previous command history - the up arrow key
-------------------------------------------

You will often find yourself typing a whole series of similar commands, that
differ only by a few characters each time. It would be tedious if you had to
type each one of them in individually. Similarly, you may find that the long
command you just typed in has failed because of a single little typo. To save
yourself the time and the hassle, use of the command history.

The shell remembers all the commands that you recently typed in. At any time,
you can set the text of your command to any of these previous commands, as if
you had just typed it in. It is then trivial to go back and edit those sections
that need to be amended, using the left and right arrow keys, etc.

To access previous commands, simply press the up arrow. This will give you the
last command you typed in. Pressing it again will produce the command before
that, and so on. You can also press the down arrow key to find more recent
commands if you have gone too far back in the command history.


Command completion - the tab key
--------------------------------

You will often need to supply filenames as arguments to commands. Some of these
may be long and hard to remember, for example, if they contain patient IDs
and dates. However, the computer knows everything about which files are in
which folder, and can therefore help the user to type in the right filename.
You will find that often, it is only necessary to type in the first few letters
of the file and the shell will fill in the rest for you.

You can ask the computer to attempt to complete the filename by pressing the
``TAB`` key. At this point, the shell will look at the fragment that you have
already typed in, and compare it to the list of files in the corresponding
folder. For example, if you type:

.. code-block:: console

  $ ls /data/3T_scanner/b 
  
and press the ``TAB`` key, the shell will look at the contents of the
``/data/3T_scanner`` folder, and see if any of these files or folders begin
with a ``b``. At this point, one of three things can happen:

1. there is only one file that begins with the fragment you supplied. In this
   case, the shell will complete the filename for you. Using the example above,
   if the only file or folder in ``/data/3T_scanner`` that started with a ``b``
   was ``bloggsj_010203_123``, then the command would be changed to: 
   
   .. code-block:: console 
   
     $ ls /data/3T_scanner/bloggsj_010203_123/

2. there are more than one file that begin with the fragment you supplied. In
   this case, the shell will emit a beep or some other visual feedback (unless
   configured not to) to notify the user. 
   
   Pressing the ``TAB`` key a second time will cause the shell to output a list
   of all the potential candidates, so that the user can select the right one.
   You should then just type in more of the filename and press the ``TAB`` key
   again once you think that fragment is unambiguous.

   Using the example above, there might have been another folder in
   ``/data/3T_scanner`` that started with a ``b``, say ``brownj_030201_789``. In this
   case, the first ``TAB`` key press would have produced a beep, and the second
   would have produced a list like the following:

   .. code-block:: console

     $ ls /data/3T_scanner/b 
     bloggsj_010203_123        brownj_030201_789

   You then simply need to type in an extra ``l`` and press ``TAB`` again for
   the shell to change the command as above.

3. there are no files that start with the fragment you supplied. In this case,
   the shell will also emit a beep to notify the user. This time, pressing the
   ``TAB`` key again will only cause repeated beeping. You should amend what you
   have typed in, and make sure that it is correct up to that point.

