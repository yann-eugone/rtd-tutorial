Tips with ``yokai/batch`` in a Symfony Project
==============================================

Use the batch logger
-------------------

| The batch logger will log inside the JobExecution.
| In a Symfony project, you can use that with the symfony autowiring
  by naming your variable as ``$yokaiBatchLogger``

.. code:: php

   <?php

   namespace App;

   use Psr\Log\LoggerInterface;

   final readonly class YourService
   {
       public function __construct(
           private LoggerInterface $yokaiBatchLogger,
       ) {
       }

       public function method()
       {
           $this->yokaiBatchLogger->error(...);
       }
   }

.. seealso::

   :doc:`What is the job execution? <domain/job-execution>`
