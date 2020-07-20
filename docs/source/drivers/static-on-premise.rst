.. meta::
   :description lang=en: Configuring an on-premise inventory

**********
On-premise
**********

The absence of a configured cloud driver will result in Bcome looking to its static-cache.yml configuration file in order to populate its inventories. 

Servers are keyed on their target inventory namespaces.

This will allow you to add in servers that are local to your client - i.e. on premise - into your Bcome installation.

In this simple example, we add a single server into the top-level inventory namespace.


Directory structure
===================

.. code-block:: bash

   .
   └── bcome
       └── networks.yml
       └── static-cache.yml

static-cache.yml file
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

networks.yml file
=================

.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate
     network: {}

Ascii Cast
==========

TODO: ascii_casts/simple-static




