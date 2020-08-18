.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring Bcome's GCP driver for OAuth authorisation

*********
GCP Oauth
*********

This guide demonstrates a basic GCP driver setup: a single inventory namespace is populated with servers having been authorised via GCP Oauth (|GCP_OAUTH|_).

For further configuration details, please refer to the |DOCS|_.


Directory structure
===================

.. code-block:: bash

   .
   ├── .gauth
   │   └── your-secrets-file.json
   └── bcome
       └── networks.yml

The networks.yml file contains your network configuration, whilst 'your-secrets-file.json' contains your OAuth 2.0 application secrets.

.. note::

   Any user requiring use of your OAuth application will need the OAuth application secrets.  

   Bcome will trigger an OAuth authentication process with first usage (or should the access tokens returned from the OAuth process have expired or been invalidated). 

.. warning::

   Access tokens are saved to the .gauth direct, the contents of which *should not* be added to source control.


Network Configuration
=====================

The networks.yml configuration is simple:

.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate

     network:
       type: gcp
       project: wbznet
       zone: europe-west1-b
       authentication_scheme: oauth
       secrets_filename: your-secrets-file.json
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

   <a href="https://asciinema.org/a/iskFuzue4LzAx6LIV9l44JGuy" target="_blank"><img src="https://asciinema.org/a/iskFuzue4LzAx6LIV9l44JGuy.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/iskFuzue4LzAx6LIV9l44JGuy``

