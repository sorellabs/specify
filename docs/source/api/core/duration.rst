**************
Type: Duration
**************

.. class:: core.Duration

   .. code-block:: haskell

      type Duration
        started: Date
        finished: Date
        slowThreshold: Number/ms

      implements
        Equality, Setter<Duration>, ToString, Extractor

      methods
        -- Creating instances
        new Duration(Date, Date, Number/ms) -> Duration
        create: { ρ | Duration } -> Duration
        set: @Duration => { ρ | α >: Duration } -> Duration

        -- Extracting information
        started: @Duration => Date
        finished: @Duration => Date
        slowThreshold: @Duration => Number/ms
        isSlow: @Duration => Void -> Boolean
        time: @Duration => Void -> Number/ms

        -- Comparing and testing
        equals: @Duration => Duration -> Boolean

        -- Converting to other types
        toString: @Duration => Void -> String


   Represents the duration of a particular test.


Creating instances
------------------

.. method:: Duration.create(values)

   :returns: A new Duration instance.

   .. code-block:: haskell

      { ρ | Duration } -> Duration

   Constructs a new instance of the Duration.


.. method:: Duration.set(values)

   :returns: A shallow copy of the Duration with the given fields updated.

   .. code-block:: haskell

      @Duration => { ρ | α >: Duration } -> Duration

   Constructs a new instance of the Duration, based off an existing one.

       

Extracting information
----------------------

.. attribute:: Duration.started

   .. code-block:: haskell

      Date

   The start time of this duration.


.. attribute:: Duration.finished

   .. code-block:: haskell

      Date

   The ending time of this duration.


.. attribute:: Duration.slowThreshold

   .. code-block:: haskell

      Number/ms

   The threshold for considering this duration *slow* or not, in milliseconds.


.. method:: Duration.isSlow()

   :returns: Whether this duration should be considered slow

   .. code-block:: haskell

      @Duration => Void -> Boolean


.. method:: Duration.time

   :returns: The total time of this duration (in milliseconds)

   .. code-block:: haskell

      @Duration => Void -> Number/ms


Comparing and testing
---------------------

.. method:: Duration.equals(aDuration)

   :returns: ``true`` if both Durations are the same object

   .. code-block:: haskell

      @Duration => Duration -> Boolean

   Compares two durations using reference equality.


Converting to other types
-------------------------

.. method:: Duration.toString()

   :returns: A textual representation of the duration.

   .. code-block:: haskell

      @Duration => Void -> Number/ms

