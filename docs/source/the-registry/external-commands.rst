.. include:: ../urls.rst

.. meta::
   :description lang=en: The registry - configuring external commands

*************
External Hook
*************

An external-hook registry method allows for the invoking of an external script, which itself makes use of Bcome's runtime.

This guide demonstrates the configuration of a simple external hook.

Here's our script -

.. code-block:: bash

   require 'bcome'
   require 'pry'

   # Define an orchestrator
   orchestrator = ::Bcome::Orchestrator.instance

   # Load in the namespace
   @node = orchestrator.get(ENV["bcome_context"])

   # Work with the namespace

   # run a command
   command = "echo \"hello world I am `hostname -A`\""
   @node.run command

   exit 0


.. hint::

   There is a lot you can do with a ``@node`` object. See |INTERACTING_WITH_NODE|_.

Let's save our script as 'say_hello_server.rb' at the following location:

.. code-block:: bash

   .
   └── bcome
       └── scripts
           └── say_hello_server.rb


Now we'll add a registry.yml association for our script, associating it with the 'production:wbzsite' namespace in our namespace tree:

.. code-block:: yaml

   ---
   "production:wbzsite(.+)?":
     - type: external
       description: "Say hello, server"
       console_command: say_hello
       group: salutations
       local_command: ruby scripts/say_hello_server.rb

Now let's try out our external registry hook:

.. raw:: html

   <a href="https://asciinema.org/a/LW2fZbitYWKyaFnCuWtgyJOIP" target="_blank"><img src="https://asciinema.org/a/LW2fZbitYWKyaFnCuWtgyJOIP.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/LW2fZbitYWKyaFnCuWtgyJOIP``


.. note::

   See our documentation site for more details on configuring |EXTERNAL_DOCS|_.
