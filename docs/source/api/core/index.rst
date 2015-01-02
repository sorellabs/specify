************
Module: core
************

.. module:: core
   :synopsis: Describe, structure and runs tests.
   :platform: ECMAScript 5

The **core** module provides the basic objects and functions for describing,
structuring and running tests with Specify.


Signals
=======

.. class:: Hook
   :noindex:

   .. code-block:: haskell

      type Hook
        actions: Array<Future<Error, Void>>

      methods
        run: @Hook => Void -> Rx.Observable<Error, α>

   Represents a list of actions that are to be ran in reaction to some event.

   .. rst-class:: detail-link

      :doc:`+ <Hook>`

.. class:: Signal
   :noindex:

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
       
   Represents one of the important events that might happen when running
   tests.

   .. rst-class:: detail-link

      :doc:`+ <Signal>`


Tests
=====

.. class:: Test
   :noindex:

   .. code-block:: haskell

      type Test = Suite
                    name: String
                    tests: Array<Test>
                    beforeAll: Hook
                    afterAll: Hook
                    beforeEach: Hook
                    afterEach: Hook

                | Case
                    name: String
                    test: Future<Error, Void>
                    timeout: Maybe<Number/ms>
                    slow: Maybe<Number/ms>
                    enabled: Maybe<Case -> Boolean>

      methods
        run: [String], Config -> Rx.Observable[Error, Signal]
        
   The ``Test`` type models the components of a specification
   hierarchy.

   .. rst-class:: detail-link

      :doc:`+ <Test>`


Results
=======

.. class:: Duration
   :noindex:

   .. code-block:: haskell

      type Duration
        started: Date
        finished: Date
        slowThreshold: <Number/ms>

      methods
        isSlow: @Duration => Void -> Boolean
        time: @Duration => Void -> <Number/ms>
        toString: @Duration => Void -> String

   Represents the duration of a particular tests.

   .. rst-class:: detail-link

      :doc:`+ <Duration>`


.. class:: LogEntry
   :noindex:

   .. code-block:: haskell

      type LogEntry
        date: Date
        log: Array<Any>

   Represents content that has been logged during the execution of a test.

   .. rst-class:: detail-link

      :doc:`+ <LogEntry>`


.. class:: Result
   :noindex:

   .. code-block:: haskell

      type Result = Success
                      title: Array<String>
                      duration: Duration
                      log: Array<LogEntry>

                  | Failure
                      title: Array<String>
                      exception: Any
                      duration: Duration
                      log: Array<LogEntry>

                  | Ignored
                      title: Array<String>

      methods
        fullTitle: @Result => Void -> String
        name: @Result => Void -> String


   Represents the result of running a test ``Case``.

   .. rst-class:: detail-link

      :doc:`+ <Result>`


Reports
=======

.. class:: Report
   :noindex:

   .. code-block:: haskell

      type Report
        started: Date
        finished: Date
        passed: Array<Result>
        failed: Array<Result>
        ignored: Array<Result>

      methods
        add: @Report => Result -> Report
        empty: @Report => Void -> Report
        time: @Report => Void -> <Number/ms>
        all: @Report => Void -> Array<Result>

   Summarises the execution of a series of test cases.

   .. rst-class:: detail-link

      :doc:`+ <Report>`

Running
=======

.. class:: Config
   :noindex:

   .. code-block:: haskell

      type Config
        slowThreshold: <Number/ms>
        timeout: <Number/ms>
        runOnly: Case -> Boolean


.. data:: defaultConfig

   The default configuration for running tests.


.. function:: makeRunner(config, suites, reporter)
   :noindex:

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observable<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Future<Error, Report>

   Constructs a task for running all tests.

   .. rst-class:: detail-link

      :doc:`+ <makeRunner>`


.. function:: run(config, suites, reporter)
   :noindex:

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observables<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Void

   Runs a series of test cases.

   .. rst-class:: detail-link

      :doc:`+ <run>`


.. function:: runWithDefaults(suites, reporter)
   :noindex:

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observables<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Void

   Runs a series of test cases using the ``defaultConfig``.

   .. seealso::

      Function :func:`run`
          ``runWithDefaults`` is just a partially applied ``run``.



.. toctree::
   :hidden:
   
   Hook
   Signal
   Test
   Duration
   LogEntry
   Result
   Report
   makeRunner
   run
