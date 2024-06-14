Job parameter accessor
======================

When a job (or a component within a job) can be working with a parameterized value, it can rely on a
`JobParameterAccessorInterface <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/JobParameterAccessorInterface.php>`__
instance to retrieve that value.

.. code:: php

   <?php

   declare(strict_types=1);

   use Yokai\Batch\Job\JobInterface;
   use Yokai\Batch\Job\Parameters\JobParameterAccessorInterface;
   use Yokai\Batch\JobExecution;

   class FooJob implements JobInterface
   {
       public function __construct(
           private JobParameterAccessorInterface $path,
       ) {
       }

       public function execute(JobExecution $jobExecution): void
       {
           /** @var string $path */
           $path = $this->path->get($jobExecution);
           // do something with $path
       }
   }

What types of parameter accessors exists ?
------------------------------------------

**Built-in parameter accessors:**
* :doc:`ChainParameterAccessor.php <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/ChainParameterAccessor.php>`
  try multiple parameter accessors, the first that is not failing is used.
* :doc:`ClosestJobExecutionAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/ClosestJobExecutionAccessor.php>`
  try another parameter accessor on each job execution in hierarchy, until not failed.
* :doc:`DefaultParameterAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/DefaultParameterAccessor.php>`
  try accessing parameter using another parameter accessor, use default value if failed.
* :doc:`JobExecutionParameterAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/JobExecutionParameterAccessor.php>`
  extract value from job execution’s
`parameters <https://github.com/yokai-php/batch/blob/0.x/src/src/JobParameters.php>:doc:`__.
* `JobExecutionSummaryAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/JobExecutionSummaryAccessor.php>`
  extract value from job execution’s `summary <https://github.com/yokai-php/batch/blob/0.x/src/src/Summary.php>:doc:`__.
* `ParentJobExecutionAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/ParentJobExecutionAccessor.php>`
  use another parameter accessor on job execution’s parent execution.
* :doc:`ReplaceWithVariablesParameterAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/ReplaceWithVariablesParameterAccessor.php>`
  use another parameter accessor to get string value, and replace variables before returning.
* :doc:`RootJobExecutionAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/RootJobExecutionAccessor.php>`
  use another parameter accessor on job execution’s root execution.
* :doc:`SiblingJobExecutionAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/SiblingJobExecutionAccessor.php>`
  use another parameter accessor on job execution’s sibling execution.
* :doc:`StaticValueParameterAccessor <https://github.com/yokai-php/batch/blob/0.x/src/src/Job/Parameters/StaticValueParameterAccessor.php>`
  use static value provided at construction.

**Parameter accessors from bridges:**

* From :doc:``symfony/framework-bundle`` bridge:
  * `ContainerParameterAccessor <https://github.com/yokai-php/batch-symfony-framework/blob/0.x/src/src/ContainerParameterAccessor.php>`
    use a parameter from Symfony’s container.

.. seealso::

   :doc:`What is a job? </domain/job>`

   :doc:`When does a job execution hierarchy is created? </domain/job-with-children>`

   :doc:`What is a job execution? </domain/job-execution>`
