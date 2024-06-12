Bridge with ``symfony/console``
===================================

todo

Command
--------------

The
`RunJobCommand <../../src/batch-symfony-console/src/RunJobCommand.php>`__
can execute any job.

The command accepts 2 arguments : - the job name to execute - the job
parameters for the ``JobExecution`` (optional)

Examples :

.. code:: console

   bin/console import
   bin/console export '{"toFile":"/path/to/file.xml"}'


Job launcher
--------------

The
`RunCommandJobLauncher <../../src/batch-symfony-console/src/RunCommandJobLauncher.php>`__
execute jobs via an asynchronous symfony command.

The command called is ```yokai:batch:run`` <command.md>`__, and the
command will actually execute the job.

Additionally, the command will run with an output redirect (``>>``) to
``var/log/batch_execute.log``.

