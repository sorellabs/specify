************
Type: Signal
************

.. class:: core.Signal

   .. code-block:: haskell

      type Signal = Started
                      value: Test
                      path: Array<String>

                  | Finished
                      value: Test
                      path: Array<String>

                  | TestResult
                      value: Result

      methods
        fullTitle: @Signal => Void -> String


   The Signal ADT models the possible signals that might be used in the stream
   of events in a test execution.

   * **Started**: Used when a ``Suite`` or ``Case`` is about to start, contains
     a reference to that test and a list of the names of all its parents.

   * **Finished**: Used when a ``Suite`` or ``Case`` has finished
     running. Contains a reference to that test and a list of the names of all
     its parents.

   * **TestResult**: Used when a ``Case`` has finished running, and always
     included before ``Finished``. Contains the result of running the ``Case``.


Creating instances
------------------

.. method:: Signal.create(values)

   .. code-block:: haskell

      @Started => { value: Test, path: Array<String> } -> Signal
      @Finished => { value: Test, path: Array<String> } -> Signal
      @TestResult => { value: Result> } -> Signal

   Constructs a new Signal value with the given field values.

   .. warning::

      This method has to be called on one of the ADT cases (``Started``,
      ``Finished``, ``TestResult``), rather than on ``Signal`` directly.

   **Example**::

       var a = Signal.Started.create({ value: someTest, path: [] });
       // => Started(...)

       var b = Signal.Finished.create({ value: someTest, path: [] });
       // => Finished(...)

       var c = Signal.TestResult.create({ value: someTestResult });
       // => TestResult(...)


.. method:: Signal.set(values)

   :returns: A shallow copy of the Signal with the given fields changed.

   .. code-block:: haskell

      @Started => { value: Test, path: Array<String> } -> Signal
      @Finished => { value: Test, path: Array<String> } -> Signal
      @TestResult => { value: Result } -> Signal

   ``.set()`` is an alternative to constructing a new Signal object
   without having to pass all of the fields, when you want an object
   that looks like an existing one, but with a few fields changed.

   .. warning::

      This method has to be called on one of the ADT cases (``Started``,
      ``Finished``, ``TestResult``), rather than on ``Signal`` directly.


Comparing and testing
---------------------

.. method:: Signal.equals(aSignal)

   :return: ``true`` if the two Signals are the same object.

   .. code-block:: haskell

      @Signal => Signal -> Boolean

   Compares two Signals using reference equality.


.. attribute:: Signal.isStarted

   .. code-block:: haskell

      Boolean

   ``true`` if the signal has a ``Started`` tag.


.. attribute:: Signal.isFinished

   .. code-block:: haskell

      Boolean

   ``true`` if the signal has a ``Finished`` tag.


.. attribute:: Signal.isTestResult

   .. code-block:: haskell

      Boolean

   ``true`` if the signal has a ``TestResult`` tag.


Converting to other types
-------------------------   

.. method:: Signal.toString()

   :returns: A textual representation of the Signal

   .. code-block:: haskell

      @Signal => Void -> String


.. method:: Signal.fullName()

   :returns: The full path of a the :class:`Test` in the Signal.

   .. code-block:: haskell

      @Signal => Void -> String

   Signals store the path of the tests as an Array of strings. This method
   gives you a convenient way of getting the full path concatenated in a
   single string, where each path component is separated by a single space.

   **Examples**::

       var test = Test.Suite.Create({ name: 'World' });
       Signal.Started.create({ value: test, path: ['Hello'] });
       // => 'Hello World'

       var res = Result.Ignored.create({ title: ['Hello', 'World'] });
       Signal.TestResult.create({ value: res });
       // => 'Hello World'


Transforming signals
--------------------

.. method:: Signal.cata(aPattern)

   :returns: The transformed value

   .. code-block:: haskell

      @Signal => Pattern -> α

      type Pattern
        Started: (Test, Array<String>) -> α
        Finished: (Test, Array<String>) -> α
        TestResult: Result -> α

   The :term:`catamorphism` function provides a form of pattern matching
   and structure-based transformation for the Signal ADT. Your code
   should provide a transformation for each one of the possible cases in
   the ADT, and the values will be passed as arguments to the function
   you provide.

   .. note::

      If you're using the **Sparkler** library for Sweet.js, it's also
      possible to pattern match on the Signal objects directly, since
      they implement the Extractor interface.
