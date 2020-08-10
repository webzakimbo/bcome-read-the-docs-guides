.. meta::
   :description lang=en: Configuring Bcome for multi-hybrid-cloud

******************
Multi-hybrid-cloud
******************

Bcome allows for interacting with servers from multiple clouds, and on-premise infrastructure at the same time.

This guide demonstrates a simple AWS, GCP and on-premise integration, where each source is used to populate an inventory, and then all three used as contributors to populate a merged (multi-hybrid-cloud) inventory.

.. note::

   A multi-hybrid-cloud inventory is no different to any other: it may be interacted with through the console, or programmatically from an orchestration script.

Directory structure
===================

.. code-block:: bash

   .
   ├── .aws
   │   └── keys
   ├── .gauth
   │   └── service-account.json
   └── bcome
       └── networks.yml
       └── static-cache.yml


Network Configuration
=====================

.. code-block:: yaml

   ---
   wbz:
     type: collection
     description: Entire WBZ estate

   wbz:on_premise:
     type: inventory
     description: on-premise infrastructure

   wbz:aws:
     type: inventory
     description: AWS machines
     network:
       type: ec2
       credentials_key: webzakimbo
       provisioning_region: eu-west-1
       filters:
         instance-state-name: running

     ssh_settings:
       timeout_in_seconds: 10
       proxy:
         host_lookup: by_bcome_namespace
         namespace: aws:bastion

     override_identifier: "[a-z]*_[a-z]*[a-z]*_(.+)"


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
     - aws
     - on_premise


Static Cache Manifest
=====================

.. code-block:: yaml

   ---
   wbz:on_premise:
   - identifier: fileserver_a
     internal_ip_address: 192.168.0.24
     local_network: yes
     description: Office filestore
     cloud_tags:
       data:
         environment: office
         function: filestore
         group: administrative



Tree Hierarchy
==============

Illustrated below is the installation's tree structure.  

The "gcp" namespace contains servers populated from Google Cloud Platform. The "aws" namespace contains servers populated from Amazon Web Services. The "on_premise" namespaces defines a local file server.  
The "hybrid" namespace merges all three.

.. code-block:: bash

      ▐▆   Namespace tree wbz
      │
      ├───╸ inventory aws
      │         ├───╸ server bastion
      │         ├───╸ server puppet
      │         ├───╸ server wbzsite_app1
      │         └───╸ server wbzsite_app2
      │
      ├───╸ inventory gcp
      │         ├───╸ server bastion
      │         ├───╸ server puppet
      │         └───╸ server wbzsite_app_sq6v
      │
      ├───╸ inventory-merge hybrid
      │         ├───╸ server wbz_aws_bastion
      │         ├───╸ server wbz_aws_puppet
      │         ├───╸ server wbz_aws_wbzsite_app1
      │         ├───╸ server wbz_aws_wbzsite_app2
      │         ├───╸ server wbz_gcp_bastion
      │         ├───╸ server wbz_gcp_puppet
      │         ├───╸ server wbz_gcp_wbzsite_app_sq6v
      │         └───╸ server wbz_on_premise_fileserver_a
      │
      └───╸ inventory on_premise
                └───╸ server fileserver_a

.. note::

  Note how the merged inventory retains the full server identifiers. This prevents name conflicts when similar inventories are used as contributors to a merge.


SSH Routing tree
================

.. code-block:: bash


      ▐▆   Ssh connection routes wbz
      │
      ├───╸ server
      │     namespace: wbz:on_premise:fileserver_a
      │     ip address 192.168.1.50
      │     user guillaume
      │
      ├───╸ proxy [1]
      │     bcome node wbz:aws:bastion
      │     host 3.250.83.109
      │     user ubuntu
      │
      │         ├───╸ server
      │         │     namespace: wbz:aws:wbzsite_app1
      │         │     ip address 10.0.9.73
      │         │     user ubuntu
      │         │
      │         ├───╸ server
      │         │     namespace: wbz:aws:wbzsite_app2
      │         │     ip address 10.0.4.13
      │         │     user ubuntu
      │         │
      │         ├───╸ server
      │         │     namespace: wbz:aws:puppet
      │         │     ip address 10.0.0.10
      │         │     user ubuntu
      │         │
      │         └───╸ server
      │               namespace: wbz:aws:bastion
      │               ip address 10.0.35.208
      │               user ubuntu
      │
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

The following Asciicast presents a quick run-through of navigating the namespace configuration.


.. raw:: html

   <a href="https://asciinema.org/a/0WfGGYxUpR5gm2heeWFK4SpvJ" target="_blank"><img src="https://asciinema.org/a/0WfGGYxUpR5gm2heeWFK4SpvJ.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/0WfGGYxUpR5gm2heeWFK4SpvJ``

