What is an item processor ?
===========================

The item processor is used by the item job to transform every read item.

It can be any class implementing
`ItemProcessorInterface <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/ItemProcessorInterface.php>:doc:`__.

What types of item processors exists ?
--------------------------------------

**Built-in item processors:**

* `ArrayMapProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/ArrayMapProcessor.php>`
  apply a callback to each element of array items.
* :doc:`CallbackProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/CallbackProcessor.php>`
  use a callback to transform each items.
* :doc:`ChainProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/ChainProcessor.php>`
  chain transformation of multiple item processor, one after the other.
* :doc:`FilterUniqueProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/FilterUniqueProcessor.php>`
  assign an identifier to each item, and skip already encountered items.
* :doc:`NullProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/NullProcessor.php>`
  perform no transformation on items.
* :doc:`RoutingProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Job/Item/Processor/RoutingProcessor.php>`
  route processing to different processor based on your logic.

**Item processors from bridges:**

* From :doc:``symfony/validator`` bridge:
  * `SkipInvalidItemProcessor <https://github.com/yokai-php/batch-symfony-validator/blob/0.x/src/src/SkipInvalidItemProcessor.php>`
    validate item and throw exception if invalid that will cause item to be skipped.
* From :doc:``symfony/serializer`` bridge:
  * `DenormalizeItemProcessor <https://github.com/yokai-php/batch-symfony-serializer/blob/0.x/src/src/DenormalizeItemProcessor.php>`
    denormalize each item.
  * :doc:`NormalizeItemProcessor <https://github.com/yokai-php/batch-symfony-serializer/blob/0.x/src/src/NormalizeItemProcessor.php>`
    normalize each item.

**Item processors for testing purpose:**

* :doc:`TestDebugProcessor <https://github.com/yokai-php/batch/blob/0.x/src/Test/Job/Item/Processor/TestDebugProcessor.php>`
  dummy item processor that you can use in your unit tests.

.. seealso::

   :doc:`What is an item job? </domain/item-job>`
