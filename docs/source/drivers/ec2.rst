.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring Bcome's EC2 driver


**********
EC2 Driver
**********

This guide demonstrates a basic EC2 driver setup: a single inventory namespace is configured to populate itself with some arbitrary servers.

For further configuration details, please refer to the |DOCS|_.

Directory structure
===================

You should have a directory structure as follows:

.. code-block:: bash

   .
   ├── .aws
   │   └── keys
   └── bcome
       └── networks.yml


The networks.yml file contains your network configuration, whilst 'keys' contains your AWS access keys.

.. note::

   For further information on linking AWS accounts, see |AWS_AUTH_DOCS|_.

Network Configuration
=====================

Below can be seen a simple network configuration that populates a single Inventory with all running instances from the 'eu-west-1' region of the linked AWS account.

.. code-block:: yaml

   ---
   wbz:
     type: inventory
     description: Entire WBZ estate

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
         namespace: bastion

.. note::

   For a full list of namespace attributes see |NAMESPACE_ATTRIBUTES_DOCS|_.

Ascii Cast
==========

The following Ascii Cast illustrates the above configuration:

TODO: ascii_casts/drivers-ec2

