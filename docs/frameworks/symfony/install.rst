Install and configure in a Symfony project
==========================================

.. code:: console

   composer require yokai/batch-symfony-framework

.. code:: php

   // config/bundles.php
   return [
       // ...
       Yokai\Batch\Bridge\Symfony\Framework\YokaiBatchBundle::class => ['all' => true],
   ];

Job launcher
------------

You can use many different job launcher in your application, you will be
able to register these using configuration:

.. code:: yaml

   # config/packages/yokai_batch.yaml
   yokai_batch:
       launcher:
           default: simple
           launchers:
             simple: ...
             async: ...

..

.. note::
   If you do not configure anything here, you will be using the
   `SimpleJobLauncher <https://github.com/yokai-php/batch/blob/0.x/src/src/Launcher/SimpleJobLauncher.php>`__.

The ``default`` job launcher, must reference a launcher name, defined in
the ``launchers`` list. The ``default`` job launcher will be the
autowired instance of job launcher when you ask for one. All
``launchers`` will be registered as a service, and an autowire named
alias will be configured for it. For instance, in the example below, you
will be able to register all these launchers like this:

.. code:: php

   <?php

   use Yokai\Batch\Launcher\JobLauncherInterface;

   final class YourAppCode
   {
       public function __construct(
           private JobLauncherInterface $jobLauncher, // will inject the default job launcher
           private JobLauncherInterface $simpleJobLauncher, // will inject the "simple" job launcher
           private JobLauncherInterface $messengerJobLauncher, // will inject the "messenger" job launcher
       ) {
       }
   }

All ``launchers`` are configured using a DSN, every scheme has it’s own associated factory:

* ``simple://simple``: a `SimpleJobLauncher <https://github.com/yokai-php/batch/blob/0.x/src/src/Launcher/SimpleJobLauncher.php>`__, no configuration allowed
* ``messenger://messenger``: a `DispatchMessageJobLauncher <https://github.com/yokai-php/batch-symfony-messenger/blob/0.x/src/src/DispatchMessageJobLauncher.php>`__, no configuration allowed
* ``console://console``: a `RunCommandJobLauncher <https://github.com/yokai-php/batch-symfony-console/blob/0.x/src/src/RunCommandJobLauncher.php>`__, configurable options:
  * ``log``: the filename where command output will be redirected (defaults to ``batch_execute.log``)
* ``service://service``: pointing to a service of your choice, configurable options:
  * ``service``: the id of the service to use (required, an exception will be thrown otherwise)

.. seealso::

   :doc:`What is a job launcher? <domain/job-launcher>`

JobExecution storage
~~~~~~~~~~~~~~~~~~~~

You can have only one storage for your ``JobExecution``, and you have
several options:

* ``filesystem`` will create a file for each ``JobExecution`` in
  ``%kernel.project_dir%/var/batch/{execution.jobName}/{execution.id}.json``
* ``dbal`` will create a row in a table for each ``JobExecution``
* ``service`` will use a service you have defined in your application

.. code:: yaml

   # config/packages/yokai_batch.yaml
   yokai_batch:
       storage:
           filesystem: ~
           # Or with yokai/batch-doctrine-dbal (& doctrine/dbal)
           # dbal: ~
           # Or with a service of yours
           # service: ~

.. note::
   The default storage is ``filesystem``, because it only
   requires a writeable filesystem. But if you already have
   ``doctrine/dbal`` in your project, it is highly recommended to use it
   instead. Because querying ``JobExecution`` in a filesystem might be
   slow, specially if you are planing to add UIs on top.

.. seealso::

   :doc:`What is a job execution storage? <domain/job-execution-storage>`
