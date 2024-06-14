Job execution storage
=====================

What is a job execution storage ?
---------------------------------

Whenever a job is launched, whether is starts immediately or not, an
execution is stored for it.

The execution are stored to allow you to keep an eye on what is
happening.

This persistence is on the responsibility of the job execution storage.

How do I store my Job Execution ?
---------------------------------

You should never be forced to store ``JobExecution`` yourself.

This is `JobLauncher <job-launcher>`__\ ’s job to store it whenever
it is required (usually at the beginning and the end of the job
execution).

How can I retrieve a Job Execution afterwards ?
-----------------------------------------------

Every storage implements
`JobExecutionStorageInterface <https://github.com/yokai-php/batch/tree/0.x/src/Storage/JobExecutionStorageInterface.php>:doc:`__
that has a method called ``retrieve``. Use this method to retrieve one
execution using job name and execution id.

Depending on which storage you decided to use, you may also be able to:

* list of all executions for particular job, if your storage implements
  `ListableJobExecutionStorageInterface <https://github.com/yokai-php/batch/tree/0.x/src/Storage/ListableJobExecutionStorageInterface.php>`
* filter list of executions matching criteria you provided, if your storage implements
  :doc:`QueryableJobExecutionStorageInterface <https://github.com/yokai-php/batch/tree/0.x/src/Storage/QueryableJobExecutionStorageInterface.php>`

.. note::
   Sometimes the storage may implement these interfaces, but
   due to the way executions are stored, it might not be recommended
   heavily rely on these extra methods.

What types of storages exists ?
-------------------------------

**Built-in storages:**

* :doc:`NullJobExecutionStorage <https://github.com/yokai-php/batch/tree/0.x/src/Storage/NullJobExecutionStorage.php>`
  do not persist any job execution.
* :doc:`FilesystemJobExecutionStorage <https://github.com/yokai-php/batch/tree/0.x/src/Storage/FilesystemJobExecutionStorage.php>`
  store job executions to a file on local filesystem.

**Storages from bridges:**

* From :doc:``doctrine/dbal`` bridge:
  * `DoctrineDBALJobExecutionStorage <https://github.com/yokai-php/batch-doctrine-dbal/blob/0.x/src/DoctrineDBALJobExecutionStorage.php>`
    store job executions to a relational database.

**Storages for testing purpose:**

* :doc:`InMemoryJobExecutionStorage <https://github.com/yokai-php/batch/tree/0.x/src/Test/Storage/InMemoryJobExecutionStorage.php>`
  store executions in a private var that can be accessed afterwards in your tests.
