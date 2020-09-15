.. include:: ../urls.rst

.. meta::
   :description lang=en: Configuring multi-hop SSH proxying within Bcome.

**************
Multi-proxying
**************

If you connect to your machines via an intermediary, then you will need to include a Proxy host in your SSH configuration. 

This guide expands on the :doc:`../ssh-configuration/basic-proxying` guide to demonstrate how multiple proxies - i.e. a chain of proxies -  may be configured.


Example configuration
---------------------

The networks.yml configuration below defines three inventories: one containing a public-facing proxy server, the second a proxy server installed intra-network (and accessible only from the first), whilst the third inventory defines servers reachable by proxying via both proxy servers. 

.. code-block:: yaml

   ---
   wbz:
     type: collection
     description: WBZ gcp estate
     network:
       type: gcp
       project: wbznet
       zone: europe-west1-b
       :authentication_scheme: service_account
       service_account_credentials: service-account.json
       service_scopes:
       - https://www.googleapis.com/auth/compute.readonly
       - https://www.googleapis.com/auth/cloud-platform

   wbz:public_proxies:
     type: inventory
     description: public ssh proxies
     override_identifier: "prod_net_(.+)"
     network:
       filters: status:running AND labels.function=bastion AND labels.environment=prod-net

   wbz:private_proxies:
     type: inventory
     description: private ssh proxies
     override_identifier: "prod_net_(.+)"
     network:
       filters: status:running AND labels.function=internal-bastion AND labels.environment=prod-net
     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: public_proxies:bastion

   wbz:servers:
     type: inventory
     description: Servers accessible via two proxy hops 
     network:
       filters: status:running AND labels.environment=prod-net AND NOT (labels.function=bastion OR labels.function=internal-bastion)
     override_identifier: "prod_net_(.+)"
     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: public_proxies:bastion
         - host_lookup: by_bcome_namespace
           namespace: private_proxies:internal_jump

My local user is ``guillaume``, and I have ssh keys added to my agent.  

.. note::

   The ``proxy`` block in your ``ssh_settings`` is an array of proxies: you may define as many as you like.

Routes
------

Bcome's ``routes`` command will result in the following for the above configuration:

.. code-block:: bash

         ▐▆   Ssh connection routes wbz
         │
         ├───╸ server
         │     namespace: wbz:public_proxies:bastion
         │     ip address 104.155.101.98
         │     user guillaume
         │
         └───╸ proxy [1]
               bcome node wbz:public_proxies:bastion
               host 104.155.101.98
               user guillaume

                   └───╸ proxy [2]
                         bcome node wbz:private_proxies:internal_jump
                         host 10.0.33.2
                         user guillaume
   
                             ├───╸ server
                             │     namespace: wbz:servers:puppet
                             │     ip address 10.0.0.10
                             │     user guillaume
                             │
                             └───╸ server
                                   namespace: wbz:servers:wbzsite_app_sq6v
                                   ip address 10.0.0.2
                                   user guillaume

The AsciiCast below demonstrates my configuration:

.. raw:: html

   <a href="https://asciinema.org/a/nPKMiZ6fyum56kHAWswg6ywXO" target="_blank"><img src="https://asciinema.org/a/nPKMiZ6fyum56kHAWswg6ywXO.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/nPKMiZ6fyum56kHAWswg6ywXO``

See the Bcome documentation for more detailed & alternative proxy configuration options: |SSH_PROXY_DOCS|_.

