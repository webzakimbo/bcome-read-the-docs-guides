.. meta::
   :description lang=en: Setting up a simple single namespace


****************
Single Namespace
****************

The most simple Bcome setup is where your servers are loaded into a single namespace.

Let's see how this works for an inventory retrieved from Google Cloud Platform where I have two servers configured - a bastion server and an application server.

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

    ssh_settings:
      proxy:
        - host_lookup: by_bcome_namespace
          namespace: bastion

Tree Hierarchy
==============

.. code-block:: bash

      ▐▆   Namespace tree wbz
      │
      ├───╸ server bastion
      └───╸ server wbzsite_app_sq6v


Ascii Cast
==========







