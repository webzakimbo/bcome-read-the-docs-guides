.. include:: ../urls.rst

.. meta::
   :description lang=en: Bcome's SSH configuration - simplest configuration


**************************
Simplest SSH configuration
**************************

The simplest SSH configuration is to define an empty ``ssh_settings`` block, or to leave it out entirely.

In this case, Bcome will fallback to default system SSH settings:

- Your local user (i.e. the system user running the Bcome process) will be used as the SSH user
- You will not be able to proxy SSH connections, all connections will be direct.

.. note::

   In all cases - whether SSH is invoked programmatically or otherwise - Bcome will defer to your local ssh-agent for your SSH keys.  

   Make sure that your ssh-agent is running.

Example configuration
---------------------

The networks.yml example below - that of single GCP inventory, returning a single server - demonstrates this configuration:

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
       secrets_filename: wbz-net-oauth-secrets.json
       service_scopes:
       - https://www.googleapis.com/auth/compute.readonly
       - https://www.googleapis.com/auth/cloud-platform
       filters: status:running AND labels.function:bastion

     ssh_settings: {}   

My local user is ``guillaume``, and I have ssh keys added to my agent.  See how I may interact with server in the Inventory in the AsciiCast below:

.. raw:: html

   <a href="https://asciinema.org/a/1KJiA2r4GoxQdtKuVSFsjqeMb" target="_blank"><img src="https://asciinema.org/a/1KJiA2r4GoxQdtKuVSFsjqeMb.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/1KJiA2r4GoxQdtKuVSFsjqeMb``

See the Bcome documentation for more detailed configuration options: |SSH_ATTR_DOCS|.

