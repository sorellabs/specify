************
Type: Result
************

.. class:: core.Result

   .. code-block:: haskell

      type core.Result = Success
                           title: Array<String>
                           duration: Duration
                           log: Array<LogEntry>

                         Failure
                           title: Array<String>
                           exception: Any
                           duration: Duration
                           log: Array<LogEntry>

                         Ignored
                           title: Array<String>

      implements
        Equality, Setter<Result>, ToString, Extractor, Cata

      methods
        -- Creating instances
        new Result.Success(Array<String>, Duration, Array<LogEntry>) -> Result
        new Result.Failure(Array<String>, Any, Duration, Array<LogEntry>) -> Result
        new Result.Ignored(Array<String>) -> Result

        Success.create: { ρ | Success } -> Result
        Failure.create: { ρ | Failure } -> Result
        Ignored.create: { ρ | Ignored } -> Result

        Success.set: { ρ | α >: Success } -> Result
        Failure.set: { ρ | α >: Failure } -> Result
        Ignored.set: { ρ | α >: Ignored } -> Result

        -- Comparing and testing
        equals: @Result => Result -> Boolean
        isSuccess: @Result => Boolean
        isFailure: @Result => Boolean
        isIgnored: @Result => Boolean

        -- Converting to other types
        toString: @Result => Void -> String

        -- Transforming results
        cata: @Result => Pattern -> α
              where type Pattern
                Success: (Array<String>, Duration, Array<LogEntry>) -> α
                Failure: (Array<String>, Any, Duration, Array<LogEntry>) -> α
                Ignored: Array<String> -> α

        -- Extracting information
        fullTitle: @Result => Void -> String
        name: @Result => Void -> String


   Represents the results of each tests.


Creating instances
------------------

.. method:: Result.create(values)

   :returns: Constructs a new Result instance.

   .. code-block:: haskell

      Success.create: { ρ | Success } -> Result
      Failure.create: { ρ | Failure } -> Result
      Ignored.create: { ρ | Ignored } -> Result


.. method:: Result.set(values)

   :returns: A shallow copy of the Result with the given fields updated.

   .. code-block:: haskell

      @Success => { ρ | α >: Success } -> Result
      @Failure => { ρ | α >: Failure } -> Result
      @Ignored => { ρ | α >: Ignored } -> Result


Comparing and testing
---------------------

.. method:: Result.equals(aResult)

   :returns: ``true`` if both Result objects are the same object.

   .. code-block:: haskell

      @Result => Result -> Boolean

   Compares two Result objects using reference equality.


.. attribute:: Result.isSuccess

   .. code-block:: haskell

      Boolean

   ``true`` if the Result object has a ``Success`` tag.


.. attribute:: Result.isFailure

   .. code-block:: haskell

      Boolean

   ``true`` if the Result object has a ``Failure`` tag.


.. attribute:: Result.isIgnored

   .. code-block:: haskell

      Boolean

   ``true`` if the Result object has an ``Ignored`` tag.


Converting to other types
-------------------------

.. method:: Result.toString()

   :returns: A textual representation of the Result object

   .. code-block:: haskell

      @Result => Void -> String


Transforming results
--------------------

.. method:: Result.cata(aPattern)

   :returns: The transformation for the Result's tag.

   .. code-block:: haskell

      @Result => Pattern -> α

      type Pattern
       Success: (Array<String>, Duration, Array<LogEntry>) -> α
       Failure: (Array<String>, Any, Duration, Array<LogEntry>) -> α
       Ignored: Array<String> -> α

   The :term:`catamorphism` function provides a form of pattern matching
   and structure-based transformation for the Result ADT. Your code
   should provide a transformation for each one of the possible cases in
   the ADT, and the values will be passed as arguments to the function
   you provide.

   .. note::

      If you're using the **Sparkler** library for Sweet.js, it's also
      possible to pattern match on the Signal objects directly, since
      they implement the Extractor interface.
       

Extracting information
----------------------

.. method:: Result.fullTitle()

   :returns: The full title of the Test that yielded this result

   .. code-block:: haskell

      @Result => Void -> String

   Returns a space-separated string containing the entire path to the test that
   resulted in this result, starting from the root.


.. method:: Result.name()

   :returns: The name of the test that yielded this result.

   .. code-block:: haskell

      @Result => Void -> String

