.. include:: ../urls.rst

.. meta::
   :description lang=en: Built in commands - tunnel

******
tunnel
******

The ``tunnel`` command forwards a remote port locally. 

The following AsciiCast demonstrates how to access a remote elastic search process running on port 9200, by forwarding the connection to local port 9200.  In the example, the remote elastic search server is accessible via two intermediate proxies, with Bcome handling all the SSH connectivity transparently.


.. raw:: html

   <a href="https://asciinema.org/a/0aB2LhU8gEA51FA75FnQ83DZ3" target="_blank"><img src="https://asciinema.org/a/0aB2LhU8gEA51FA75FnQ83DZ3.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/0aB2LhU8gEA51FA75FnQ83DZ3``


See the Bcome documentation for more information on |EXECUTING_COMMANDS_DOCS|_.

For a full command list, see |COMMAND_MENU_DOCS|_.
