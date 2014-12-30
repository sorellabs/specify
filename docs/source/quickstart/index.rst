Getting Started
===============

This page will guide you through all the things you need to do to start writing
Specify tests for Node and Browsers (the instructions should be similar to
other platforms). It assumes you'll be using the `Sweet.js`_ DSL and the
built-in assertion library.

.. note::
  
   This tutorial does not assume any previous knowledge of **Sweet.js** or
   **Node.js**, only JavaScript. However, you'll need to have **Node.js**
   installed in your system.


.. _Sweet.js: http://sweetjs.org/



Installing
----------

1) The first step is installing `Node.js`_. Follow the instructions in the
   `Node.js download page`_ for your OS.
   
   .. _Node.js: http://nodejs.org/
   .. _Node.js download page: http://nodejs.org/download/
   
   After finishing this step, you should have both the ``node`` and ``npm``
   applications available in your system. Check the command line to see if
   they're there:
   
   .. code-block:: shell
   
      $ node --version
      v0.10.35
   
      $ npm --version
      1.4.28
   
   
   .. warning::
   
      When installing Node.js from the system package manager in some Linux
      systems (e.g.: `Ubuntu`_), the Node binary will be ``nodejs`` instead of
      ``node``, to avoid conflict with another package with the same name. In
      Debian-based systems, you can install the ``nodejs-legacy`` package, or
      you can symlink the Node binary as ``node`` somewhere in your path
      manually.

.. _Ubuntu: http://www.ubuntu.com/


2) The second step is installing the `Sweet.js`_ package. We'll be using
   Sweet.js macros to write tests and assertions with a more expressive and
   clean syntax than what JavaScript provides.

   To install Sweet.js, run the following command in your project directory:
   
   .. code-block:: shell
   
      $ npm install sweet.js@0.7.1


3) After installing Node and Sweet.js, we can install the **Specify**
   framework, which will provide everything else that we need for writing
   tests.

   To install the Specify framework, run the following command in your project
   directory:

   .. code-block:: shell

      $ npm install specify-framework


Using Specify in the Browser
''''''''''''''''''''''''''''

We'll be using Node modules for writing modular test files, and loading the
Specify framework. Since Browsers don't implement Node modules by default,
we'll need another compile step to transform these modules in a format that the
Browser can understand. For this, we'll use the `Browserify`_ tool.

To install Browserify, run the following command in your project directory:

.. code-block:: shell

   $ npm install browserify

.. _Browserify: http://browserify.org/


Writing tests
-------------

The idiomatic way of writing tests in Specify is very similar to `Mocha`_,
but the Sweet.js DSL reduces some of the boilerplate. In Specify, you organise
your tests into **Suites**, which may contain many definitions and suites, and
definitions, which may contain many assertions related to a particular
functionality.

.. _Mocha: http://mochajs.org/

As an example, let's consider the ``add.js`` module::

    function add(a, b) {
      if (b === 0) {
        return a;
      } else {
        return add(a + 1, b - 1);
      }
    }

    module.exports = add;

A specification for this module could look like this:

add(a, b)
    * When adding 0 to any value, the result is that value.
    * When adding two positive numbers, the result is always greater than both.

We can capture this specification by writing a test file for the ``add.js``
module. Let's call it ``test-add.sjs``:

.. code-block:: js
   :linenos:
   :emphasize-lines: 2,5,6,11

   // First we load the `add` module
   var add = require('./add');

   // Then we define the specification
   module.exports = spec 'add(a, b)' {
     it 'when adding 0 to any value, the result is that value.' {
       add(4, 0) => 4;
       add(0, 5) => 5;
     }

     it 'when adding two positive numbers, the result is always greater than both.' {
       add(4, 5) => 9;
       add(3, 2) => 5
     }
   }

``test-add.sjs`` does two things: first it loads the ``add`` module that we
want to test. Then it exports a ``Suite`` object (which is created by the
``spec '<description>' { <definitions or suites...> }`` syntax). Each ``it
'<description>' { <javascript statements...> }`` creates a new definition
inside the ``spec`` group, and corresponds to one of the bullet points in the
specification we drafted above.

The ``<expression> => <expected result>`` syntax is added by the Specify
assertion module, and allows one to make (deep) equality assertions in tests in
a clean and concise manner.

.. note::

   ``require(...)`` and ``module.exports`` are part of Node's module system,
   which is a better take on the CommonJS Modules specification. If you're not
   familiar with it, you can read the `Node Modules to Rule Them All`_ article,
   which describes module systems in JS, and Node modules in particular.

.. _Node Modules to Rule Them All: http://robotlolita.me/2013/06/06/node-modules-to-rule-them-all.html


Running a test module
---------------------

Before running the test module we just wrote, we'll need to compile it. To do
that, we can type the following in the command line:

.. code-block:: shell

   $ node_modules/.bin/sjs --module specify/macros --output test-add.js test.sjs

And since we're going to be running it a bit, we can put it in a
``package.json`` file on the root of the project, and have npm work as a task
runner. To do so create a ``package.json`` file with the following contents::

    {
      "scripts": {
        "compile-tests": "node_modules/.bin/sjs --module specify/macros --output test-add.js test.sjs"
      }
    }

This way you can type ``npm run compile-tests`` in the command line to invoke
the compilation command. You can also use ``Grunt``, ``Make``, or any other
task runner tool you feel comfortable with.


Using the built-in test runner
''''''''''''''''''''''''''''''

If you're running the tests directly, you may use the the command line tool to
run them either on the Browser or Node. To do so, we invoke the ``specify``
application from the command line, and give it the module we want to execute:

.. code-block:: shell

   $ node_modules/.bin/specify test-add.js
   



