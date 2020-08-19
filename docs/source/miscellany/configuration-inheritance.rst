.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuration Inheritance & Overrides

*************************
Configuration Inheritance
*************************

SSH and networking configuration is inherited by child namespaces at which point it may be overridden (see: |INHERITANCE_DOCS|_).

Here we show a simple two-network GCP setup, where both inherit part of their networking configuration from their parent namespace.

Network Configuration
---------------------

Below can be seen an example network.yml configuration.  The "gcp:prod: and "gcp:dev" namespaces both inherit elements of their network configuration from the parent "gcp" collection, then override that configuration with their own authentication scheme (a service account for one, OAuth 2.0 for the other) and their own network filters.

.. code-block:: yaml

   ---
   wbz:
     type: collection
     description: Entire WBZ estate

   wbz:gcp:
     type: collection
     description: WBZ gcp estate
     network:
       type: gcp
       project: wbznet
       service_scopes:
       - https://www.googleapis.com/auth/compute.readonly
       - https://www.googleapis.com/auth/cloud-platform

   wbz:gcp:prod:
     type: inventory
     description: GCP Production
     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: gcp:prod:bastion
     network:
       filters: status:running AND labels.environment=prod-net
       authentication_scheme: oauth
       secrets_filename: wbz-net-oauth-secrets.json
       zone: europe-west1-b

   wbz:gcp:dev:
     type: inventory
     description: GCP Production
     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: gcp:dev:bastion
     network:
       filters: status:running AND labels.environment=dev-net
       authentication_scheme: service_account
       service_account_credentials: service-account.json
       zone: europe-west1-c


Any SSH or network configuration may be defined in this way.

Ascii Cast
==========

The following Ascii Cast illustrates the above configuration:

.. raw:: html

   <a href="https://asciinema.org/a/C2m3rAOEHTp72RrNSVetkGkYa" target="_blank"><img src="https://asciinema.org/a/C2m3rAOEHTp72RrNSVetkGkYa.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/C2m3rAOEHTp72RrNSVetkGkYa``

