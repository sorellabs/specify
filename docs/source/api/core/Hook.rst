**********
Type: Hook
**********

.. class:: core.Hook

   .. code-block:: haskell
      
      type Hook
        actions: Array<Future<Error, Void>>
        
      implements
        Equality, Setter<Hook>, ToString, Extractor

      methods
        -- Creating instances
        new Hook(Array<Future<Error, Void>>) -> Hook
        create: { ρ | Hook } -> Hook
        set: @Hook => { ρ | α >: Hook } -> Hook

        -- Extracting information
        actions: @Duration => Array<Future<Error, Void>>
        
        -- Comparing and testing
        equals: @Hook => Hook -> Hook
        
        -- Converting to other types
        toString: @Hook => Void -> String
      

   A Hook provides a representation of a set of actions to be executed in
   reaction to some event. They're used in the :class:`Test` ``Suite`` objects
   to represent actions that should be ran before and after tests.

   There are no guarantees on the order each action will be executed, although
   no action is ever executed in parallel, for this reason actions should
   always be self contained.


Extracting information
----------------------

.. attribute:: actions

   .. code-block:: haskell

      Array<Future<Error, Void>>

   The set of actions to be executed when running this hook.


Constructing instances
----------------------

.. method:: Hook.create(values)

   .. code-block:: haskell

      { ρ | Hook } -> Hook

   Constructs a new Hook value with the given field values.

   **Example**::

       var hook = Hook.create({ actions: [] });
       // => Hook([])


.. method:: Hook.set(values)

   :returns: A shallow copy of the Hook with the given fields changed.

   .. code-block:: haskell

      @Hook => { actions: Array<Future<Error, Void>> } -> Hook

   ``.set()`` is an alternative to constructing a new Hook object without
   having to pass all of the fields, when you want an object that looks like an
   existing one, but with a few fields changed.


Comparing and testing
---------------------

.. method:: Hook.equals(aHook)

   :returns: ``true`` if the two Hooks are the same object.

   .. code-block:: haskell

      @Hook => Hook -> Boolean

   Compares two Hooks using reference equality.



Converting to other types
-------------------------

.. method:: Hook.toString()

   :returns: A textual representation of the Hook.

   .. code-block:: haskell

      @Hook => Void -> String



Interacting with actions
------------------------

.. method:: Hook.run()

   :returns: A stream with no values, that is closed when all actions finish
              running.

   .. code-block:: haskell

      @Hook => Void -> Rx.Observable<Error, Void>

   Executes the set of actions in the Hook sequentially. No guarantees are
   made about the order in which actions are ran.

