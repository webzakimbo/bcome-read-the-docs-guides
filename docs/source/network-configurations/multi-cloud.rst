.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring a multi-cloud Bcome installation.

***********
Multi-cloud
***********

Bcome allows for interacting with servers from multiple clouds at the same time.

This guide demonstrates a simple AWS & GCP integration, where each cloud is used to populate an inventory, and then both used as contributors to populate a merged (multi cloud) inventory.

.. note::

   A multi-cloud inventory is no different to any other: it may be interacted with through the console, or programmatically from an orchestration script.

For documentation on linking AWS accounts, see: |AWS_AUTH_DOCS|_.

For documentation on linking GCP accounts, see: |GCP_AUTH_DOCS|_


Directory structure
===================

You should have a directory structure as follows:


.. code-block:: bash

   .
   ├── .aws
   │   └── keys
   ├── .gauth
   │   └── service-account.json
   └── bcome
       └── networks.yml


Network Configuration
=====================


.. code-block:: yaml

   ---
   wbz:
     type: collection
     description: Entire WBZ estate

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
       authentication_scheme: service_account
       service_account_credentials: service-account.json
       service_scopes:
       - https://www.googleapis.com/auth/compute.readonly
       - https://www.googleapis.com/auth/cloud-platform
       filters: status:running

     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: gcp:bastion

     override_identifier: "[a-z]*_[a-z]*_(.+)"

   wbz:multicloud:
     type: inventory-merge
     description: GCP & AWS
     contributors:
     - gcp
     - aws



Tree Hierarchy
==============

Illustrated below is the installation's tree structure.  

The "gcp" namespace contains servers populated from Google Cloud Platform. The "aws" namespace contains servers populated from Amazon Web Services.  The "multicloud" namespace merges them both.


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
      └───╸ inventory-merge multicloud
                ├───╸ server wbz_aws_bastion
                ├───╸ server wbz_aws_puppet
                ├───╸ server wbz_aws_wbzsite_app1
                ├───╸ server wbz_aws_wbzsite_app2
                ├───╸ server wbz_gcp_bastion
                ├───╸ server wbz_gcp_puppet
                └───╸ server wbz_gcp_wbzsite_app_sq6v


.. note::

  Note how the merged inventory retains the full server identifiers. This prevents name conflicts when similar inventories are used as contributors to a merge.


SSH Routing tree
================

The routing below illustrates the two connection pathways that Bcome will use when interacting with the servers within the installation.

.. code-block:: bash


      ▐▆   Ssh connection routes wbz
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

.. raw:: html

   <a href="https://asciinema.org/a/6o3aRMAMZ10Kd7if3Bfr3rDqb" target="_blank"><img src="https://asciinema.org/a/6o3aRMAMZ10Kd7if3Bfr3rDqb.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/6o3aRMAMZ10Kd7if3Bfr3rDqb``
