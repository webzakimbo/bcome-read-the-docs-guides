.. meta::
   :description lang=en: Setting up a simple single namespace


****************
Single Namespace
****************

The most simple Bcome setup is where your servers are loaded into a single namespace.

Let's see how this works for an inventory retrieved from Google Cloud Platform:

Network configuration
=====================

.. code-block:: yaml

  ---
  wbz:
    type: inventory
    description: All my servers in a single namespace

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

Tree Hierarchy
==============

.. code-block:: bash

      ▐▆   Namespace tree wbz
      │
      ├───╸ server bastion
      ├───╸ server elastic_master_node_0ls7
      ├───╸ server elastic_master_node_9s29
      ├───╸ server elastic_master_node_mlxk
      ├───╸ server puppet
      └───╸ server wbzsite_app_sq6v


Ascii Cast
==========





