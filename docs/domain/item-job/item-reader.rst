What is an item reader ?
========================

The item reader is used by the item job to extract item from a source.

It can be any class implementing
`ItemReaderInterface <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/ItemReaderInterface.php>:doc:`__.

What types of item readers exists ?
-----------------------------------

**Built-in item readers:**

* `FixedColumnSizeFileReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/Filesystem/FixedColumnSizeFileReader.php>`
  read a file line by line, and decode each line with fixed columns size to an array.
* :doc:`JsonLinesReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/Filesystem/JsonLinesReader.php>`
  read a file line by line, and decode each line as JSON.
* :doc:`AddMetadataReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/AddMetadataReader.php>`
  decorates another reader by adding static information to each read item.

* :doc:`IndexWithReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/IndexWithReader.php>`
  decorates another reader by changing index of each item.
* :doc:`ParameterAccessorReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/ParameterAccessorReader.php>`
  read from an inmemory value located at some configurable place.
* :doc:`SequenceReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/SequenceReader.php>`
  read from multiple item reader, one after the other.
* :doc:`StaticIterableReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/StaticIterableReader.php>`
  read from an iterable you provide during construction.
* :doc:`CallbackReader <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Reader/CallbackReader.php>`
  read from a :doc:``Closure`` you provide during construction.

**Item readers from bridges:**

* From ``openspout/openspout`` bridge:
  * `FlatFileReader <https://github.com/yokai-php/batch-openspout/blob/0.x/src/src/Reader/FlatFileReader.php>`
    read from any CSV/ODS/XLSX file.
* From :doc:``doctrine/dbal`` bridge:
  * `DoctrineDBALQueryOffsetReader <https://github.com/yokai-php/batch-doctrine-dbal/blob/0.x/src/src/DoctrineDBALQueryOffsetReader.php>`
  read execute an SQL query and iterate over results, using a limit + offset pagination strategy.
  * :doc:`DoctrineDBALQueryCursorReader <https://github.com/yokai-php/batch-doctrine-dbal/blob/0.x/src/src/DoctrineDBALQueryCursorReader.php>`
  read execute an SQL query and iterate over results, using a column based cursor strategy.
* From :doc:``doctrine/orm`` bridge:
  * `EntityReader <https://github.com/yokai-php/batch-doctrine-orm/blob/0.x/src/src/EntityReader.php>`
  read from any Doctrine ORM entity.

**Item readers for testing purpose:**

* :doc:`TestDebugReader <https://github.com/yokai-php/batch/blob/0.x/src/Test/Job/Item/Reader/TestDebugReader.php>`
  dummy item reader that you can use in your unit tests.

.. seealso::

   :doc:`What is an item job? </domain/item-job>`
