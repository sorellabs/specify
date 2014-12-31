************
Module: Core
************

.. module:: core
   :synopsis: Describe, structure and runs tests.
   :platform: ECMAScript 5

The **core** module provides the basic objects and functions for describing,
structuring and running tests with Specify.


Signals
=======

.. class:: Hook

   .. code-block:: haskell
      
      type Hook
        actions: Array<Future<Error, Void>>
      
      methods
        run: @Hook => Void -> Rx.Observable[Error, α]


  Hooks represent a list of actions that are to be ran in reaction to some
  event.

  .. rst-class:: detail-link

     :doc:`+ <hook>`


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
   tests:

   * **Started**: Used when a ``Suite`` or ``Case`` is about to start, contains
     a reference to that test and a list of the names of all its parents.

   * **Finished**: Used when a ``Suite`` or ``Case`` has finished
     running. Contains a reference to that test and a list of the names of all
     its parents.

   * **TestResult**: Used when a ``Case`` has finished running, and always
     included before ``Finished``. Contains the result of running the ``Case``.

   .. rst-class:: detail-link

      :doc:`+ <signal>`


Tests
=====

.. data:: Test

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
   hierarchy. ``Suite`` objects sit at the top and contain many ``Case`` and
   other ``Suite`` objects.

   .. rst-class:: detail-link

      :doc:`+ <test>`


Results
=======

.. class:: Duration

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

      :doc:`+ <duration>`


.. class:: LogEntry

   .. code-block:: haskell

      type LogEntry
        date: Date
        log: Array<Any>

   Represents content that has been logged during the execution of a test.

   .. rst-class:: detail-link

      :doc:`+ <log-entry>`


.. class:: Result

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

      :doc:`+ <result>`


Reports
=======

.. class:: Report

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

      :doc:`+ <report>`

Running
=======

.. class:: Config

   .. code-block:: haskell

      type Config
        slowThreshold: <Number/ms>
        timeout: <Number/ms>
        runOnly: Case -> Boolean


.. data:: defaultConfig

   The default configuration for running tests.


.. function:: makeRunner(config, suites, reporter)

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observable<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Future<Error, Report>

   Constructs a task for running all tests.

   .. rst-class:: detail-link

      :doc:`+ <makeRunner>`


.. function:: run(config, suites, reporter)

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observables<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Void

   Runs a series of test cases.

   .. rst-class:: detail-link

      :doc:`+ <run>`


.. function runWithDefaults(suites, reporter)

   .. code-block:: haskell

      Config
      -> Array<Test>
      -> (Rx.Observables<α, Signal>, Rx.Observable<α, Report> -> Void)
      -> Void

   Runs a series of test cases using the ``defaultConfig``.

   .. seealso::

      Function :py:fun:`run`
          ``runWithDefaults`` is just a partially applied ``run``.



