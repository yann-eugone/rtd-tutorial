Using ``yokai/batch`` in a Symfony Project
==========================================

Frameworks offers various ways to simplify developer's daily jobs.

Installation
------------

Please refer to the ::doc::`dedicated documentation </frameworks/symfony/install>`.

Job as a service
----------------

| We need to provide the library with all the implemented jobs we have.
| We will be using Symfony's dependency injection system for that.

| As Symfony supports registering all classes in ``src/`` as a service, we
  can leverage this behaviour to register all jobs in ``src/``.
| We will add a tag to every found class in ``src/`` that implements ``Yokai\Batch\Job\JobInterface``:

.. code:: yaml
   # config/services.yaml
   services:
     _defaults:
       _instanceof:
         Yokai\Batch\Job\JobInterface:
           tags: ['yokai_batch.job']

Your first job
--------------

| In a Symfony project, we will prefer using one class per job, because service discovery is so easy to use.
| But also because it will be very far easier to configure your job using PHP than any other format.
| For instance, there is components that uses ``Closure``, has static constructors, ...
| But keep in mind you can register your jobs with any other format of your choice.

.. code:: php
   <?php

   namespace App\NamespaceOfYourChoice;

   use Yokai\Batch\Bridge\Symfony\Framework\JobWithStaticNameInterface;
   use Yokai\Batch\Job\JobInterface;

   final class NameOfYourJob implements JobInterface, JobWithStaticNameInterface
   {
       public static function getJobName(): string
       {
           return 'job.name';
       }

       public function execute(JobExecution $jobExecution): void
       {
           // your logic here
       }
   }

.. hint::
   | When registering jobs with dedicated class, you can use the
     `JobWithStaticNameInterface <https://github.com/yokai-php/batch-symfony-framework/blob/0.x/src/src/JobWithStaticNameInterface.php>`__
     interface to be able to specify the job name of your service.
   | Otherwise, the service id will be used, and in that case, the service id is the FQCN.

.. seealso::
   | :doc:`What is a job? </domain/job>`

Triggering the job
------------------

Then the job will be triggered with its name (or service id when notc specified):

.. code:: php
   <?php

   namespace App\MyNamespace;

   use Yokai\Batch\Storage\JobExecutionStorageInterface;

   final class MyClass
   {
       public function __construct(
           private JobLauncherInterface $jobLauncher,
       ) {
       }

       public function method(): void
       {
           $this->jobLauncher->launch('job.name');
       }
   }

The job launcher that will be injected depends on the packages you have installed, order matter:

* if ``yokai/batch-symfony-messenger`` is installed, you will receive a
  ``Yokai\Batch\Bridge\Symfony\Messenger\DispatchMessageJobLauncher``
* if ``yokai/batch-symfony-console`` is installed, you will receive a
  ``Yokai\Batch\Bridge\Symfony\Console\RunCommandJobLauncher``
* otherwise you will receive a ``Yokai\Batch\Launcher\SimpleJobLauncher``

