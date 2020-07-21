.. meta::
   :description lang=en: Configuring an on-premise inventory

**********
On-premise
**********

Where a Static Manifest has been set against a given namespace, Bcome will populate that namespace with servers from the manifest.

In this simple example, we add a single server into the top-level inventory namespace.

.. note::

   See |STATIC_MANIFESTS_DOCS|_ for full documentation.


Directory structure
===================

.. code-block:: bash

   .
   └── bcome
       └── networks.yml
       └── static-cache.yml


Static Cache manifest 
=====================

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


Network Configuration
=====================

The networks.yml file is very simple - there is no need to specify a cloud driver as Bcome will default to loading in the declared Static Cache.


.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate
     network: {}


Ascii Cast
==========

TODO: ascii_casts/simple-static




