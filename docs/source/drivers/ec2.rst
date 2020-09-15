.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring Bcome's EC2 driver for AWS authorisation.

***
EC2
***

This guide demonstrates a basic EC2 driver setup: a single inventory namespace is configured to populate itself with some arbitrary servers.

.. note::

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

The networks.yml configuration is simple:

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

.. raw:: html

   <a href="https://asciinema.org/a/0kwvSqjdkl9N3GYE39ZgHzzWP" target="_blank"><img src="https://asciinema.org/a/0kwvSqjdkl9N3GYE39ZgHzzWP.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/0kwvSqjdkl9N3GYE39ZgHzzWP``


