**************
Type: LogEntry
**************

.. class:: LogEntry

   .. code-block:: haskell

      type LogEntry 
        date: Date
        log: Array<Any>

      implements
        Equality, Setter<LogEntry>, ToString, Extractor

      methods
        -- Creating instances
        new LogEntry(Date, Array<Any>) -> LogEntry
        create: { ρ | LogEntry } -> LogEntry
        set: @LogEntry => { ρ | α >: LogEntry } -> LogEntry

        -- Comparing and testing
        equals: @LogEntry => LogEntry -> Boolean

        -- Converting to other types
        toString: @LogEntry => Void -> LogEntry

        -- Extracting information
        date: @LogEntry => Date
        log: @LogEntry => Array<Any>


   Represents content that has been logged during the execution of a test.


Creating instances
------------------

.. method:: LogEntry.create(values)

   :returns: A new LogEntry instance

   .. code-block:: haskell

      { ρ | LogEntry } -> LogEntry


.. method:: LogEntry.set(values)

   :returns: A shallow copy of a LogEntry with the given fields updated.

   .. code-block:: haskell

      @LogEntry => { ρ | α >: LogEntry } -> LogEntry


Comparing and testing
---------------------

.. method:: LogEntry.equals(aLogEntry)

   :returns: ``true`` if both LogEntries are the same object.

   .. code-block:: haskell

      @LogEntry => LogEntry -> Boolean

   Compares two LogEntry objects using reference equality.


Converting to other types
-------------------------

.. method:: LogEntry.toString()

   :returns: A textual representation of the LogEntry.

   .. code-block:: haskell

      @LogEntry => Void -> String


Extracting information
----------------------

.. attribute:: LogEntry.date

   .. code-block:: haskell

      Date

   The time at which the entry was logged.


.. attribute:: LogEntry.log

   .. code-block:: haskell

      Array<Any>

   A list of all arguments passed to ``console.log``


