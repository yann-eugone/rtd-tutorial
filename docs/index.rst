Yokai Batch: Batch processing with PHP
======================================

**Yokai Batch** is batch processing job library written with PHP, with zero dependencies.

With this library, you will be able to:

* 📑 keep track of the execution of your jobs
* 🚀 have base classes to handle batch processing jobs
* ♻️ have decoupled reusable components to compose your jobs
* 🏭 have bridges with popular libraries and frameworks

What is Batch?
--------------

| The Batch library solves all your massive data processing problems.
| It can also be seen as an ETL.
| It can works both synchronously & asynchronously.
| It will store execution results, so you can access it afterwards.

How it works?
-------------

Batch will allow you to declare and execute jobs.

| A job is the class that is responsible for **what** your code is doing.
| This is the class you will have to create (or reuse), as it contains the
  business logic required for what you wish to achieve.

The only requirement is implementing `JobInterface <https://github.com/yokai-php/batch/tree/0.x/src/batch/src/Job/JobInterface.php>`__,

.. code-block:: php

    <?php

    use Yokai\Batch\Job\JobInterface;
    use Yokai\Batch\JobExecution;

    class ImportJob implements JobInterface
    {
        public function execute(JobExecution $jobExecution): void
        {
            // your import logic will be executed here
        }
    }

.. seealso::
   | :doc:`What is a job? </domain/job>`

| When you have your job ready, you will want to execute it.
| This is the responsibility of the job launcher.

| There is multiple implementation of a job launcher across bridges.
| Because batch can work both synchronously & asynchronously, the job launcher might just schedule job's execution.
| Thus, when you ask the job launcher to “launch” a job, you have to check
  the ``JobExecution`` status that it had returned to know if the job is already executed.

.. code-block:: php

    <?php

    use Yokai\Batch\Launcher\JobLauncherInterface;

    class ImportController
    {
         public function __construct(
             private JobLauncherInterface $launcher,
         ) {
         }

         public function trigger()
         {
             $jobExecution = $this->launcher->launch('import', ['path' => '/path/to/file/to/import']);
             // now you can look for information in JobExecution
             // or if execution is asynchronous, redirect user to a UI where he will watch it 🍿
         }
    }

.. seealso::
   | :doc:`What is a job launcher? </domain/job-launcher>`

| While the job is being executed, everything that might happen will be remembered in the job execution.
| All the components involved in the job's execution, whenever they access the object, will be able to update it.
| From time to time during the execution, the object will be stored, so you can access it afterwards.

.. code-block:: php

    <?php

    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    class JobController
    {
         public function __construct(
             private JobExecutionStorageInterface $jobExecutionStorage,
         ) {
         }

         public function show(string $jobName, string $id)
         {
             $jobExecution = $this->jobExecutionStorage->retrieve($jobName, $id);
             // here you are, build a fancy UI with live reload for the user to watch the execution 🍿
         }
    }

.. seealso::
   | :doc:`What is the job execution? </domain/job-execution>`
   | :doc:`What is a job execution storage? </domain/job-execution-storage>`

.. hint::
   | If you are running this library with Symfony, we got you covered.
   | The HTTP endpoints to manipulate job executions already exists.
   | :doc:`/frameworks/symfony/ui`

.. toctree::
   :hidden:
   :caption: Documentation

   Getting Started </getting-started>
   What is a Job? </domain/job>
   How to launch a Job? </domain/job-launcher>
   How to implement a batch processing Job? </domain/item-job>
   What is the JobExecution? </domain/job-execution>
   Where are stored my JobExecution? </domain/job-execution-storage>
   Can jobs run other jobs? </domain/job-with-children>
   How jobs can access parameters? </domain/job-parameter-accessor>
   Provide execution context to components</domain/aware-interfaces>

.. toctree::
   :hidden:
   :caption: Frameworks

   Symfony </frameworks/symfony>

.. toctree::
   :hidden:
   :caption: Bridges

   What are bridges? </bridges>
   Doctrine DBAL </bridges/doctrine-dbal>
   Doctrine ORM </bridges/doctrine-orm>
   Doctrine Persistence </bridges/doctrine-persistence>
   Flysystem </bridges/league-flysystem>
   OpenSpout </bridges/openspout>
   Symfony Console </bridges/symfony-console>
   Symfony Messenger </bridges/symfony-messenger>
   Symfony Serializer </bridges/symfony-serializer>
   Symfony Validator </bridges/symfony-validator>
