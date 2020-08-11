.. include:: ../urls.rst

.. meta::
   :description lang=en: Bcome's SSH configuration - basic single hop proxying


**************
Basic Proxying
**************

If you connect to your machines via an intermediary, then you will need to include a Proxy host in your SSH configuration.

.. note::

   In all cases - whether SSH is invoked programmatically or otherwise - Bcome will defer to your local ssh-agent for your SSH keys.  

   Make sure that your ssh-agent is running.

Example configuration
---------------------

The networks.yml configuration below defines two inventories: one containing a proxy server, and the other containing servers that may only be connected via the proxy.

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

   wbz:proxies:
     type: inventory
     description: ssh proxies
     override_identifier: "prod_net_(.+)"
     network:
       filters: status:running AND labels.function=bastion AND labels.environment=prod-net

   wbz:servers:
     type: inventory
     description: Servers
     network:
       filters: status:running AND labels.environment=prod-net AND NOT labels.function=bastion
     override_identifier: "prod_net_(.+)"
     ssh_settings:
       proxy:
         - host_lookup: by_bcome_namespace
           namespace: proxies:bastion

The 'proxies' inventory contains a single server named 'bastion' that the 'servers' inventory machines are configured above to use as their proxy.

My local user is ``guillaume``, and I have ssh keys added to my agent.  

The AsciiCast below illustrates how I may interact with my servers configured in this way:

.. raw:: html

   <a href="https://asciinema.org/a/Z8wHFA8DwYYHiKaG1oh7ZYenS" target="_blank"><img src="https://asciinema.org/a/Z8wHFA8DwYYHiKaG1oh7ZYenS.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/Z8wHFA8DwYYHiKaG1oh7ZYenS``

See the Bcome documentation for more detailed & alternative proxy configuration options: |SSH_PROXY_DOCS|.
