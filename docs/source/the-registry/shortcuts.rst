.. include:: ../urls.rst

.. meta::
   :description lang=en: The registry - configuring shortcuts

*********
Shortcuts
*********

A ``shortcut`` is a command saved in Bcome's registry that can be invoked against any registered namespace using an alternative, or "shortcut" reference.

These are useful either as a shorthand for oft-used commands, or to highlight functionality as part of your Bcome installation.

This guide will use the following Bcome namespace tree:

.. code-block:: bash


   ▐▆   Namespace tree wbz
    │
    └───╸ collection production
              ├───╸ inventory-subselect wbzsite
              │         ├───╸ server app_16m7
              │         ├───╸ server app_sq6v
              │         └───╸ server app_x52z
              │
              └───╸ inventory-subselect xops
                        ├───╸ server bastion
                        └───╸ server puppet


In our registry.yml configuration file - 

.. code-block:: bash

   .
   └── bcome
       └── registry.yml

- we'll associate two commands with the 'production:wbzsite' namespace (an inventory), and one with the xops:puppet namespace (a server), as follows:

.. code-block:: yaml

   ---
   "production:wbzsite(.+)?":
     - type: shortcut
       description: Http status
       console_command: http_status
       shortcut_command: curl -fI http://webzakimbo.com/
       group: web
     - type: shortcut
       description: Thin webserver status
       console_command: thin_status
       shortcut_command: sudo supervisorctl status thin_public
       group: web

   "production:xops:puppet":
     - type: shortcut
       description: Run a 'top' interactively
       console_command: top
       shortcut_command: top
       run_as_pseudo_tty: true
       group: misc
   

Notice how a regular expression is used to associate the shortcuts. This allows the first two commands to be made available both at inventory & server-level within 'production:wbzsite', and specifically at server-level for our last shortcut.

Note also how our last shortcut is configured as a pseudo-tty. This feature allows shortcuts to access interactive sessions:

.. hint::

   Use a pseudo-tty interactive shortcut to enable shortcuts to remote command line interfaces, e.g. a MySQL prompt.

.. note::

   For full details on configuring shortcuts, please refer to our documentation site: |SHORTCUTS_DOCS|_.


The following Asciicast demonstrates the configuration within this guide:


Ascii Cast
==========

.. raw:: html

   <a href="https://asciinema.org/a/6SWOI6MMWoyeZya4ttJ17m6QM" target="_blank"><img src="https://asciinema.org/a/6SWOI6MMWoyeZya4ttJ17m6QM.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/6SWOI6MMWoyeZya4ttJ17m6QM``
