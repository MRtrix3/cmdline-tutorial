.. _customising:

Customising the shell
=====================

As noted in previous sections, the shell can be configured in various way to
make it easier to use or more powerful. Many of the more useful modifications
can be made via the readline library, since this is the part of the shell that
allows commands to be edited. There are also various environment variables that
can be set, for instance to modify the prompt, or store more commands in the
history. 


Startup files
-------------

All of these modifications are typically implemented by adding the relevant
commands to the shell's various startup files. Part of the difficulty in
getting these modifications to work lie in figuring which file gets executed
under what circumstances, and what settings this file affects.

When the shell starts, it will typically be invoked as a *login* or *non-login*
shell, depending on whether you have just logged into the system directly into
this shell. Generally, when you log into a desktop session, the session will be
started from the shell, and this shell is the login shell. This means any
terminal sessions you start subsequently from this graphical session are
*non-login* shells (although not on macOs, by default). The reason this matters
is because different startup files are used depending on whether it's a login
shell or not. This means that settings placed in one startup file (e.g.
``~/.bashrc``) may have no effect when logging in via a remote SSH connection,
for instance, since this session would start a *login* shell. On top of that,
different distributions have come up with subtly different conventions as to
what files are used, making it very difficult to make recommendations
guaranteed to work in all cases. 

The information below relates to the Bourne Again shell (BASH), and will
hopefully be representative of most systems (see the `official BASH
documentation
<https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html>`_
for full details). The relevant files would be different for different shells:

- *login* shells will typically read the system-wide ``/etc/profile`` if it
  exists, then the user-specific ``~/.profile`` or ``~/.bash_profile`` if it
  exists. 

- *non-login* shells will typically read the ``~/.bashrc`` file if it exists. 

- on many distributions, the system-wide ``/etc/profile`` will contain
  instructions to read the system-wde ``/etc/bash.bashrc`` file if it exists.

- likewise, on many distributions, the user ``~/.profile`` will contain
  instructions to run the user ``~/.bashrc`` file if found.

- Settings that affect the readline library will normally go in the
  ``~/.inputrc`` file (see below).

As you can appreciate, things can rapidly become complicated. In most cases,
you should be able to add your settings to the ``~/.bashrc`` file, and on a
properly configured system, that should be sufficient.


Useful ~/.bashrc customisations
-------------------------------

These suggestions are things that I've found useful to add to ``~/.bashrc``,
you may find some of them to your taste:

- Append to the history, do not overwrite it::

      shopt -s histappend

- Keep a lot more commands in the history than the default::

      export HISTSIZE=10000 HISTFILESIZE=100000

- Don't keep duplicate entries in the command history::

      export HISTCONTROL=ignoreboth

- On colour-capable terminals, use colours in file listings::

      alias ls='ls --color=auto'

- Make common operations prompt you if they are about to overwrite files::

      alias rm='rm -i'
      alias cp='cp -i'
      alias mv='mv -i'
  



Useful readline customisations
------------------------------

Most distributions will come with the following already set, but just in case,
the following is useful to put into your ``~/.inputrc`` if your Home, End, or
Del keys don't work, and if you want to be able to skip over words with
Ctrl+Left/Right::

  # mappings for Home/End:
  "\e[1~": beginning-of-line
  "\e[4~": end-of-line
  "\e[7~": beginning-of-line
  
  # Del key:
  "\e[3~": delete-char
  
  # Ctrl+arrows to skip words:
  "\e[5C": forward-word
  "\e[5D": backward-word
  "\e\e[C": forward-word
  "\e\e[D": backward-word
  "\e[1;5C": forward-word
  "\e[1;5D": backward-word


Generally, most distribution set up the Up & Down arrows to go through the
history, with PgUp & PgDn going to the oldest and most recent entry
respectively. I find a more useful use for the Up & Down arrows is to perform
a *search* through the history. If nothing has been typed yet, this just goes
through the history as is normally the case. But as soon as a few characters
have been entered, only those commands in the history that start with the same
fragment will come up when you press Up. This allows you to quickly retrieve a
command you might have typed quite some time ago, as long as you know how it
started::

  # alternate mappings for "up" and "down" to search the history
  "\e[A": history-search-backward
  "\e[B": history-search-forward

For example, if you set a complicated environment variable at the beginning of
your session, but now need to modify its value slightly, you could just type
``exp`` followed by the Up arrow key, and the chances are the first match will
be the ``export COMPLICATED_VARIABLE=some_other_complicated_value`` line that
you wanted to edit -- no need to type it all in again...

