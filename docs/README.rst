unix_daemon
===========
| unix_daemon is a python module emulating BSD daemon(3).
| This module provides a function named daemon.
| If this function is called, the process become a daemon and start to run background.


Requirements
^^^^^^^^^^^^
* Python 2.7
* Unix or Linux platform.

Tested
^^^^^^
* Ubuntu 12.04 (x86_64)

Setup
^^^^^
::

  $ git clone git@github.com:wbcchsyn/unix_daemon.git
  $ cd unix_daemon
  $ sudo python setup.py install

Usage
^^^^^
unix_daemon.daemon(nochdir=False, noclose=False)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The process become a daemon and start to run background.

Arguments
---------
| If argument 1 'nochdir' is False, the process changes the calling process's current working directory to the root directory ("/");
| Otherwise, the current working directory is left unchanged.
| The default value of the nochdir is False.

| If argument 2 'noclose' is False, this function close file descriptor 0, 1 and 2, and then reopen /dev/null for file descriptor 0, 1 and 2.
| /dev/null is opened even if some of the file descriptors are closed before called this function.
| If the value is True, file descriptor 0, 1, and 2 are left.
| The default value is False.


Return Value
------------
daemon returns the pid of new process.

Note
----
| This function call fork internally to detach tty safely.
| Be careful to call this function when the more than two threads is run.
| (The best way is call this function before create a new thread.)

Example
-------
Call unix_daemon.daemon(), then the process run backgrond.

::

  import unix_daemon
  import os

  print('pid: %s\n' % os.getpid())

  pid = unix_daemon.daemon()
  with file('/tmp/foo', 'a') as f:
      f.write('new pid: %s\n' % pid)

You can see the process id changes.

Development
^^^^^^^^^^^
Before developing, copy pre-commit hook from repository.
::

  $ git clone git@github.com:wbcchsyn/unix_daemon.git
  $ cd unix_daemon
  $ cp -p utils/pre-commit .git/hook/