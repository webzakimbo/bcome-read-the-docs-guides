.. meta::
   :description lang=en: Setting up a simple single namespace


****************
Single Namespace
****************

The most simple Bcome setup is where are your servers are loaded into a single namespace.

Let's see how this may work for some servers retrieved from Google Cloud Platform:

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

