<h1>
<a href="http://specify.origamitower.com/"><img alt="Specify logo" src="https://raw.githubusercontent.com/origamitower/specify/master/specify-logo.png"></a>
</h1>

[![Build status](https://img.shields.io/travis/origamitower/specify/master.svg?style=flat)](https://travis-ci.org/origamitower/specify)
[![NPM version](https://img.shields.io/npm/v/specify.svg?style=flat)](https://npmjs.org/package/specify)
[![Dependencies status](https://img.shields.io/david/origamitower/specify.svg?style=flat)](https://david-dm.org/origamitower/specify)
![Licence](https://img.shields.io/npm/l/specify-framework.svg?style=flat&label=licence)
![Stable API](https://img.shields.io/badge/API_stability-stable-green.svg?style=flat)

Specify is a cross-platform library for defining and running tests using
a BDD style. It supports pluggable assertions, custom reporters, and
custom interfaces.


## Example

( TBD )


## Installing

The officially supported way of getting Specify is through [npm][]:

    $ npm install specify-framework

> **NOTE**
>
> If you don't have npm, you'll need to install Node.js and npm in your
> system before installing My Library. For browsers, you can follow the
> alternative installation instructions instead.

A tool like [Browserify][] or [Webpack][] can be used to run Specify in
platforms that don't implement Node-style modules, like the Browser.

[npm]: https://www.npmjs.com
[Browserify]: http://browserify.org/
[Webpack]: https://webpack.github.io/


## Documentation

( TBD )


## Why not (insert-popular-testing-library-here)?

Mocha, and other popular BDD testing frameworks, have been designed with
a particular environment in mind (Node for Mocha, Browser for Jasmine),
so while they can be extended, it's often awkward to use them in
different environments. In particular, running Mocha under CIs like
Testling requires specific Mocha extensions.

Furthermore, most popular testing frameworks and libraries (like Mocha
and Tape) are designed for asynchronous testing with callbacks. This
isn't a deal breaker, but it's a minor inconvenience since one would
need to provide wrappers for using Promises or Tasks. Specify uses
Tasks, and provides wrappers for other styles out of the box, which
makes just writing tests simpler.


## Supported platforms

Specify is supported in all platforms that support ECMAScript 5. For
platforms that don't support ECMAScript 5, (like IE8 and 9) the
[es5-shim][] library can be used to provide the additional runtime
support.

[es5-shim]: https://github.com/es-shims/es5-shim


## Support

If you think you've found a bug in the project, or want to voice your
frustration about using it (maybe the documentation isn't clear enough? Maybe
it takes too much effort to use?), feel free to open a new issue in the
[Github issue tracker](https://github.com/origamitower/specify/issues).

Pull Requests are welcome. By submitting a Pull Request you agree with releasing
your code under the MIT licence.

You can contact the author over [email](mailto:queen@robotlolita.me), or
[Twitter](https://twitter.com/robotlolita).

Note that all interactions in this project are subject to Origami Tower's
[Code of Conduct](https://github.com/origamitower/conventions/blob/master/code-of-conduct.md).


## Licence

Copyright (c) 2013-2014 [Origami Tower](http://www.origamitower.com).

Released under the [MIT](http://origami-tower.mit-license.org/) licence.
