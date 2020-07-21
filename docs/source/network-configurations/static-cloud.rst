.. meta::
   :description lang=en: Configuring a static cloud

************
Static-Cloud
************

Where a Static Manifest has been set against a given namespace, Bcome will populate that namespace with servers from the manifest (see: |STATIC_MANIFESTS_DOCS|_).

As well as using this pattern to configure on-premise infrastructure, you may add in remote infrastructure for which you may not necessarily have a Bcome driver installed.

In this example, we populate an inventory with three remote servers from a static manifest.


Directory structure
===================

.. code-block:: bash

   .
   └── bcome
       └── networks.yml
       └── static-cache.yml


Static Cache manifest 
=====================

The static-cache.yml configuration below defines three remote servers.

.. code-block:: yaml

   ---
   wbz:
   - identifier: bastion
     internal_ip_address: 10.2.0.2
     public_ip_address: 35.205.188.41
     description: GCP server - prod-net-bastion
     cloud_tags:
       data:
         environment: prod-net
         function: bastion
         group: operations
   - identifier: puppet
     internal_ip_address: 10.0.0.10
     description: GCP server - prod-net-puppet
     cloud_tags:
       data:
         environment: prod-net
         function: puppet
         group: operations
   - identifier: wbzsite_app_s27x
     internal_ip_address: 10.0.0.2
     description: GCP server - prod-net-wbzsite-app-s27x
     cloud_tags:
       data:
         group: application
         environment: prod-net
         function: frontend-wbzsite


Network Configuration
======================

The networks.yml configuration remains simple - there is no need to specify a cloud driver, the system will default to populating the inventory from the Static Cache.

.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate

     network: {}

     ssh_settings:
       timeout_in_seconds: 10
       proxy:
         host_lookup: by_bcome_namespace
         namespace: bastion


Ascii Cast
==========

TODO: ascii_casts/static-cloud
