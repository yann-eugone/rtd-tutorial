What is an item writer ?
========================

The item writer is used by the item job to load every processed item.

It can be any class implementing
`ItemWriterInterface <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/ItemWriterInterface.php>:doc:`__.

What types of item writers exists ?
-----------------------------------

**Built-in item writers:**

* `JsonLinesWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/Filesystem/JsonLinesWriter.php>`
  write items as a json string each on a line of a file.
* :doc:`ChainWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/ChainWriter.php>`
  write items on multiple item writers.
* :doc:`ConditionalWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/ConditionalWriter.php>`
  will only write items that are matching your conditions.
* :doc:`DispatchEventsWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/DispatchEventsWriter.php>`
  will dispatch events before and after writing.
* :doc:`LaunchJobForEachItemWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/LaunchJobForEachItemWriter.php>`
  launch another job for each items.
* :doc:`LaunchJobForItemsBatchWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/LaunchJobForItemsBatchWriter.php>`
  launch another job for each item batches.
* :doc:`NullWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/NullWriter.php>`
  do not write items.
* :doc:`RoutingWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/RoutingWriter.php>`
  route writing to different writer based on your logic.
* :doc:`SummaryWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/SummaryWriter.php>`
  write items to a job summary value.
* :doc:`TransformingWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/TransformingWriter.php>`
  perform items transformation before delegating to another writer.
* :doc:`CallbackWriter <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Writer/CallbackWriter.php>`
  delegate items write operations to a closure passed at construction.

**Item writers from bridges:**

* From :doc:``symfony/messenger`` bridge:
  * `DispatchEachItemAsMessageWriter <https://github.com/yokai-php/batch-symfony-messenger/blob/0.x/src/src/Writer/DispatchEachItemAsMessageWriter.php>`
    dispatch each item as a message in a bus.
* From :doc:``doctrine/dbal`` bridge:
  * `DoctrineDBALInsertWriter <https://github.com/yokai-php/batch-doctrine-dbal/blob/0.x/src/src/DoctrineDBALInsertWriter.php>`
    write items by inserting in a table via a Doctrine :doc:``Connection``.
  * `DoctrineDBALUpsertWriter <https://github.com/yokai-php/batch-doctrine-dbal/blob/0.x/src/src/DoctrineDBALUpsertWriter.php>`
    write items by inserting/updating in a table via a Doctrine :doc:``Connection``.
* From ``doctrine/persistence`` bridge:
  * `ObjectWriter <https://github.com/yokai-php/batch-doctrine-persistence/blob/0.x/src/src/ObjectWriter.php>`
    write items to any Doctrine :doc:``ObjectManager``.
* From ``openspout/openspout`` bridge:
  * `FlatFileWriter <https://github.com/yokai-php/batch-openspout/blob/0.x/src/src/Writer/FlatFileWriter.php>`
    write items to any CSV/ODS/XLSX file.

**Item writers for testing purpose:**

* :doc:`InMemoryWriter <https://github.com/yokai-php/batch/blob/0.x/src/Test/Job/Item/Writer/InMemoryWriter.php>`
  write in a private var which can be accessed afterward in your tests.
* :doc:`TestDebugWriter <https://github.com/yokai-php/batch/blob/0.x/src/Test/Job/Item/Writer/TestDebugWriter.php>`
  dummy item writer that you can use in your unit tests.

.. seealso::

   :doc:`What is an item job? </domain/item-job>`
