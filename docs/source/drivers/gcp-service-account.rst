.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring Bcome's GCP driver for Service Account authorisation

*******************
GCP Service Account
*******************

This guide demonstrates a basic GCP driver setup: a single inventory namespace is populated with servers having been authorised via a GCP service account.

For further configuration details, please refer to the |DOCS|_.

Directory structure
===================

.. code-block:: bash

   .
   ├── .gauth
   │   └── service-account.json
   └── bcome
       └── networks.yml

The networks.yml file contains your network configuration, whilst 'service-account.json' contains your GCP service account credentials.

.. note::

   For further information on linking GCP accounts, see |GCP_AUTH_DOCS|_.

Network Configuration
=====================

The networks.yml configuration is simple:

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

.. note::

   For a full list of namespace attributes see |NAMESPACE_ATTRIBUTES_DOCS|_.


Ascii Cast
==========

The following Ascii Cast illustrates the above configuration:

.. raw:: html

   <a href="https://asciinema.org/a/YCWMpROQy70UIpUaiUlOemF39" target="_blank"><img src="https://asciinema.org/a/YCWMpROQy70UIpUaiUlOemF39.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/YCWMpROQy70UIpUaiUlOemF39``
