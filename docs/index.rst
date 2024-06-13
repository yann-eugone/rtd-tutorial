Welcome to Yokai Batch documentation!
===================================

**Yokai Batch** is batch processing job library written with PHP, with zero dependencies.

With this library, you will be able to

* 📑 keep track of the execution of your jobs
* 🚀 have base classes to handle batch processing jobs
* ♻️ have decoupled reusable components to compose your jobs
* 🏭 have bridges with popular libraries and frameworks

.. toctree::
   :hidden:
   :caption: Documentation

   Getting Started <getting-started>
   What is a Job ?<domain/job>
   How to launch a Job ?<domain/job-launcher>
   How is batch processing done ?<domain/item-job>
   What is the JobExecution ?<domain/job-execution>
   Where are stored my JobExecution ?<domain/job-execution-storage>

.. toctree::
   :hidden:
   :caption: Frameworks

   Symfony <frameworks/symfony>

.. toctree::
   :hidden:
   :caption: Bridges

   Doctrine DBAL <bridges/doctrine-dbal>
   Doctrine ORM <bridges/doctrine-orm>
   Doctrine Persistence <bridges/doctrine-persistence>
   Flysystem <bridges/league-flysystem>
   OpenSpout <bridges/openspout>
   Symfony Console <bridges/symfony-console>
   Symfony Messenger <bridges/symfony-messenger>
   Symfony Serializer <bridges/symfony-serializer>
   Symfony Validator <bridges/symfony-validator>
