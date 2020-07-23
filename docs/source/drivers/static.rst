.. include:: ../urls.rst

.. meta::
   :description lang=en: Populating an inventory from a static manifest

******
Static
******

Where a Static Manifest has been set against a given namespace, Bcome will populate that namespace with servers from the manifest.

.. note::

   See |STATIC_MANIFESTS_DOCS|_ for full documentation.

A Static Manifest allows for the declaration of servers local to your client, i.e. on-premise, or remote machines for which you may not have a configured Bcome cloud driver.

In this guide we'll add a single static server into the top-level inventory namespace.


Directory structure
===================

You should have a directory structure as follows:

.. code-block:: bash

   .
   └── bcome
       └── networks.yml
       └── static-cache.yml


Static Manifest
===============

My static-cache.yml file looks as follows:

.. code-block:: yaml

   ---
   wbz:
   - identifier: fserver_a
     internal_ip_address: 192.168.1.50
     local_network: yes
     description: Central store
     cloud_tags:
       data:
         environment: office
         function: filestore
         group: administrative

It declares a single server on my local network.


Network Configuration
=====================

My networks.yml configuration is extremely simple: it declares a top-level Inventory namesapace, for which no cloud driver has been declared.

.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate
     network: {}


Ascii Cast
==========

.. raw:: html

   <a href="https://asciinema.org/a/THHfySR7m6V50VkWKBjw7OSzG" target="_blank"><img src="https://asciinema.org/a/THHfySR7m6V50VkWKBjw7OSzG.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/THHfySR7m6V50VkWKBjw7OSzG``

