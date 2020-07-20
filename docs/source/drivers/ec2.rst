.. meta::
   :description lang=en: Configuring Bcome's EC2 driver


**********
EC2 Driver
**********

This guide demonstrates a basic EC2 driver setup: a single inventory namespace is configured to populate itself with all running instances within the eu-west-1 region.


Directory structure
===================

.. code-block:: bash

   .
   ├── .aws
   │   └── keys
   └── bcome
       └── networks.yml


networks.yml file
=================

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

Note how connections to all servers are proxied via the server named 'bastion'.


Ascii Cast
==========

TODO: ascii_casts/drivers-ec2

