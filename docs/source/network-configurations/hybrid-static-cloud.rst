.. meta::
   :description lang=en: Configuring a hybrid static cloud Bcome installation.

*******************
Hybrid Static Cloud
*******************

Perhaps you have on-premise & remote servers that you wish to use within the same installation. 

In this example, we'll populate one namespace with an on-premise fileserver, and another with a few servers from GCP.  As a final step, a merged inventory is created demonstrating how to interact with all the servers at once.


Directory structure
===================

.. code-block:: bash

   .
   └── bcome
       └── networks.yml
       └── static-cache.yml


Static Cache Manifest
=====================

Here we define a single local server:

.. code-block:: yaml

   ---
   wbz:on_premise:
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

The network.yml configuration specifies three inventories: One populated from the cloud, a second populated from a static cache, and a third merging cloud & static.

.. code-block:: yaml

   ---
   wbz:
     type: collection
     description: Entire WBZ estate

   wbz:on_premise:
     type: inventory
     description: on-premise infrastructure

   wbz:gcp:
     type: inventory
     description: GCP machines
     network:
       type: gcp
       project: wbznet
       zone: europe-west1-b
       authentication_scheme: oauth
       secrets_filename: wbz-net-oauth-secrets.json
       service_scopes:
       - https://www.googleapis.com/auth/compute.readonly
       - https://www.googleapis.com/auth/cloud-platform
       filters: status:running

     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: gcp:bastion

     override_identifier: "[a-z]*_[a-z]*_(.+)"

   wbz:hybrid:
     type: inventory-merge
     description: GCP & on-premise infrastructure
     contributors:
     - gcp
     - on_premise


Tree Hierarchy
==============

Illustrated below is the installation's tree structure.  The "gcp" namespace contains servers populated from Google Cloud Platform.  The "on_premise" is a local fileserver, whilst the "hybrid" namespace merges both allowing orchestration of all at the same time.

.. code-block:: bash

      ▐▆   Namespace tree wbz
      │
      ├───╸ inventory gcp
      │         ├───╸ server bastion
      │         ├───╸ server puppet
      │         └───╸ server wbzsite_app_sq6v
      │
      ├───╸ inventory-merge hybrid
      │         ├───╸ server wbz_gcp_bastion
      │         ├───╸ server wbz_gcp_puppet
      │         ├───╸ server wbz_gcp_wbzsite_app_sq6v
      │         └───╸ server wbz_on_premise_fserver_a
      │
      └───╸ inventory on_premise
                └───╸ server fserver_a

.. note::

  Note how the merged inventory retains the full server identifiers. This prevents name conflicts when similar inventories are used as contributors to a merge.


SSH Routing Tree
================

The following routing tree (generated using Bcome's ``routes`` command) illustrates how the system will connect to the servers within it.

.. code-block:: bash

      ▐▆   Ssh connection routes wbz
      │
      ├───╸ server
      │     namespace: wbz:on_premise:fserver_a
      │     ip address 192.168.1.50
      │     user guillaume
      │
      └───╸ proxy [1]
            bcome node wbz:gcp:bastion
            host 104.155.101.98
            user guillaume

                ├───╸ server
                │     namespace: wbz:gcp:bastion
                │     ip address 10.2.0.2
                │     user guillaume
                │
                ├───╸ server
                │     namespace: wbz:gcp:puppet
                │     ip address 10.0.0.10
                │     user guillaume
                │
                └───╸ server
                      namespace: wbz:gcp:wbzsite_app_sq6v
                      ip address 10.0.0.2
                      user guillaume


Ascii Cast
==========

.. raw:: html

   <a href="https://asciinema.org/a/HJWt7HSZCLnth823FhyVcje85" target="_blank"><img src="https://asciinema.org/a/HJWt7HSZCLnth823FhyVcje85.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/HJWt7HSZCLnth823FhyVcje85``

