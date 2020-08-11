.. include:: ../urls.rst

.. meta::
   :description lang=en: Bcome's SSH configuration - Overriding configuration with ME= or me.yml

****************************
Overriding SSH Configuration
****************************

.. note::

   For detailed documentation on configuration overrides, please refer to the documentation:

   |SSH_ME_CONF_DOCS|_

   |SSH_ME_FILE_DOCS|_

The following network configuration pulls down some servers from AWS EC2 into a single inventory:

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

My local terminal user (and so default ssh username) is ``guillaume``, yet my EC2 machines have not yet been bootstrapped and expect a username of ``ubuntu``.

Rather than hardcoding the required username in my networks.yml configuration (see: |SSH_ATTR_DOCS|_) I can create an override file and reference it when I make calls to ``bcome``.  I can also save this override file to my configuration directory as ``me.yml`` so that is loaded automatically.

See the following AsciiCast for a demonstration:

.. raw:: html

   <a href="https://asciinema.org/a/ydMEvJaozNGl9NtoqZImtbwxE" target="_blank"><img src="https://asciinema.org/a/ydMEvJaozNGl9NtoqZImtbwxE.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/ydMEvJaozNGl9NtoqZImtbwxE``

