.. include:: ../urls.rst

.. meta::
   :description lang=en: Setting up a simple single namespace


****************
Single Namespace
****************

The most simple Bcome setup is where your servers are loaded into a single namespace.

Let's see how this works for an inventory retrieved from Google Cloud Platform where I have two servers configured - a bastion server and an application server.

For further configuration details, please refer to the |DOCS|_.

Tree Hierarchy
==============

Here's the tree hierarchy for this guide - a single inventory containing two servers.

.. code-block:: bash

      ▐▆   Namespace tree wbz
      │
      ├───╸ server bastion
      └───╸ server wbzsite_app_sq6v


Network Configuration
=====================

The network configuration is simple:

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


Ascii Cast
==========

.. raw:: html

   <a href="https://asciinema.org/a/wSkGafvHxF6cDJgpAgN2x70cZ" target="_blank"><img src="https://asciinema.org/a/wSkGafvHxF6cDJgpAgN2x70cZ.svg" /></a>
.. note:: 

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/wSkGafvHxF6cDJgpAgN2x70cZ``

