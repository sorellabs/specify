**********
Type: Test
**********

.. class:: core.Test

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

      implements
        Equality, Setter<Test>, ToString, Extractor, Cata

      methods
        -- Creating instances
        new Test.Suite(String, Array<Test>, Hook, Hook, Hook, Hook) -> Test
        new Test.Case( String, Future<Error, Void>, Maybe<Number/ms>
                     , Maybe<Number/ms>, Maybe<Case -> Boolean>) -> Test

        Suite.create: { ρ | Suite } -> Test
        Case.create: { ρ | Case } -> Test

        Suite.set: { ρ | α >: Suite } -> Test
        Case.set: { ρ | α >: Case } -> Test

        -- Comparing and testing
        equals: @Test => Test -> Boolean
        isSuite: @Test => Boolean
        isCase: @Test => Boolean

        -- Converting to other types
        toString: @Test => Void -> String

        -- Transforming tests
        cata: @Test => Pattern -> α
              where type Pattern
                Suite: (String, Array<Test>, Hook, Hook, Hook, Hook) -> α
                Case: ( String, Future<Error, Void>, Maybe<Number/ms>
                      , Maybe<Number/ms>, Maybe<Case -> Boolean) -> α

        -- Running tests
        run: @Test => (Array<String>, Config) -> Rx.Observable<Error, Signal>
                


   The ``Test`` type models the components of a specification
   hierarchy. ``Suite`` define branches in the hierarchy, whereas ``Case``
   defines leaves in it.


Creating instances
------------------

.. method:: Test.create(values)

   .. code-block:: haskell

      @Suite => { ρ | Suite } -> Test
      @Case => { ρ | Case } -> Test

   Constructs a new Test object with the given field values.

   .. warning::

      This method has to be called on one of the ADT cases (``Case``,
      ``Suite``) rather than on the ``Test`` object directly.

   **Example**::

       var t = Test.Case.create({
         name: 'Foo',
         test: Future.of(),
         timeout: Maybe.Nothing(),
         slow: Maybe.Nothing(),
         enabled: Maybe.Nothing()
       });
       
       var s = Test.Suite.create({
         name: 'Foo',
         tests: [t],
         beforeAll: Hook.create([]),
         afterAll: Hook.create([]),
         beforeEach: Hook.create([]),
         afterEach: Hook.create([])
       })


.. method:: Test.set(values)

   :returns: A shallow copy of the Test with the given fields changed.

   .. code-block:: haskell

      @Suite => { ρ | α >: Suite } -> Test
      @Case => { ρ | α >: Case } -> Test

   ``.set()`` is an alternative to constructing a new Test object without
   having to pass all of the fields, when you want an object that looks like an
   existing one, but with a few fields changed.

   .. warning::

      This method has to be called on one of the ADT cases (``Case``,
      ``Suite``) rather than on the ``Test`` object directly.


Comparing and testing
---------------------

.. method:: Test.equals(aTest)

   :returns: ``true`` if the two Tests are the same object.

   .. code-block:: haskell

      @Test => Test -> Boolean

   Compares two Test objects using reference equality.


.. attribute:: Test.isSuite

   .. code-block:: haskell

      Boolean

   ``true`` if the Test has a ``Suite`` tag.


.. attribute:: Test.isCase

   .. code-block:: haskell

      Boolean

   ``true`` if the Test has a ``Case`` tag.


Converting to other types
-------------------------

.. method:: Test.toString()

   :returns: A textual representation of the Test

   .. code-block:: haskell

      @Test => Void -> String


Transforming tests
------------------

.. method:: Test.cata(aPattern)

   :returns: The transformed value

   .. code-block:: haskell

      @Signal => Pattern -> α

      type Pattern
        Suite: (String, Array<Test>, Hook, Hook, Hook, Hook) -> α
        Case: ( String, Future<Error, Void>, Maybe<Number/ms>
              , Maybe<Number/ms>, Maybe<Case -> Boolean) -> α

   The :term:`catamorphism` function provides a form of pattern matching
   and structure-based transformation for the Test ADT. Your code
   should provide a transformation for each one of the possible cases in
   the ADT, and the values will be passed as arguments to the function
   you provide.

   .. note::

      If you're using the **Sparkler** library for Sweet.js, it's also
      possible to pattern match on the Test objects directly, since
      they implement the Extractor interface.


Running tests
-------------

.. method:: Test.run(path, config)

   :returns: A stream of signals with the results of the tests.

   .. code-block:: haskell

      @Test => (Array<String>, Config) -> Rx.Observable<Error, Signal>
