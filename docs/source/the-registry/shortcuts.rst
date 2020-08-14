.. include:: ../urls.rst

.. meta::
   :description lang=en: The registry - configuring shortcuts

TODO:
  Spin up another webserver, so can demonstrate running the same shortcut against each individually, then both at the same time
  something around service uptime would be cool.


*********
Shortcuts
*********

A ``shortcut`` is a command saved in Bcome's registry, that you can invoke against any registered namespace using an alternative, or "shortcut" reference.

These are useful either as a shorthand for oft used commands, or to highlight functionality as part of your Bcome installation.

.. note::

   For full details on configuring shortcuts, please refer to our documentation site: |SHORTCUTS_DOCS|_.

The following diagram illustrates the Tree structure of the namespaces in use for this guide:

.. code-block:: bash


   ▐▆   Namespace tree wbz
    │
    ├───╸ inventory-subselect management
    │         ├───╸ server bastion
    │         └───╸ server puppet
    │
    └───╸ inventory-subselect wbzsite
              └───╸ server app_sq6v


My ``registry.yml`` configuration file (see: |SHORTCUTS_DOCS|_) looks as follows:

.. code-block:: yaml

   ---

   ## TODO - come up with
     # Couple shortcuts that can be applied to all namespaces
     # Just 2 X elastic
      

Then KIT: just demo them. Simple.

DEMO:
  bcome foo:bar
  registry
  [invoke shortcuts]
  quit
  bcome foo:woo
  registry
  [invoke shortcuts]


Ascii Cast
==========

.. raw:: html

   <a href="https://asciinema.org/a/MTctn1cAnAWdGt8nG0r1N7mjp" target="_blank"><img src="https://asciinema.org/a/MTctn1cAnAWdGt8nG0r1N7mjp.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/MTctn1cAnAWdGt8nG0r1N7mjp``
