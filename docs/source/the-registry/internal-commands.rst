.. include:: ../urls.rst

.. meta::
   :description lang=en: The registry - configuring internal commands

*************
Internal hook
*************

Internal registry hooks associate |INTERNAL_DOCS|_ with your Bcome namespaces - they allow for the execution of your internal scripts in the context of the Bcome namespace at which you call them.

This guide demonstrates the configuration of a simple Internal registry hook.

Here's our Internal script class:

.. code-block:: bash

   module ::Bcome::Orchestration
     class MyInternalScript < Bcome::Orchestration::Base
   
       def execute
         do_something
       end
   
       def do_something
         # @node is made available to all internal scripts. It is an object representing the namespace at which the script was called.
         @node.run command
       end
   
       def command
         "echo \"Hello, I am `hostname -A`\"
       end
    end
   end

.. hint::

   There is a lot you can do with a ``@node`` object. See |INTERACTING_WITH_NODE|_.


Now let's save our class within our orchestration directory so that it can be loaded into our installation at runtime:

.. code-block:: bash

   .
   └── bcome
       └── orchestration
           └── my_internal_script.rb


Next we'll add a registry.yml association for our script, associating it with the 'production:wbzsite' namespace in our namespace tree:

.. code-block:: yaml

   ---
   "production:wbzsite(.+)?":
     - type: internal
       description: Say hello, server
       console_command: say_hello
       group: salutations
       orch_klass: MyInternalScript

Now let's try out our internal registry hook:

.. raw:: html

   <a href="https://asciinema.org/a/5WjMvBMYHJ9VMUjkxlvxZX9EG" target="_blank"><img src="https://asciinema.org/a/5WjMvBMYHJ9VMUjkxlvxZX9EG.svg" /></a>

.. note::

   To replay this Asciicast in your own terminal, install the ``asciinema`` package from https://asciinema.org/, and then enter the following in your terminal:

   ``asciinema play https://asciinema.org/a/5WjMvBMYHJ9VMUjkxlvxZX9EG``


.. note::

   See our documentation site for more details on configuring |INTERNAL_DOCS|_.

