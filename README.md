## sbt-tpolecat

[![Build Status](https://api.travis-ci.org/DavidGregory084/sbt-tpolecat.svg)](https://travis-ci.org/DavidGregory084/sbt-tpolecat)
[![License](https://img.shields.io/github/license/DavidGregory084/sbt-tpolecat.svg)](https://opensource.org/licenses/Apache-2.0)

### scalac options for the enlightened

sbt-tpolecat configures your scalac options according to [@tpolecat](https://github.com/tpolecat)'s [recommendations](https://tpolecat.github.io/2017/04/25/scalac-flags.html) where possible, according to the Scala version you are using.

It also enables the excellent [sbt-partial-unification](https://github.com/fiadliel/sbt-partial-unification) plugin.

As of version 0.1.11, it also supports setting options for Dotty.

### Usage

Add the following to your project's `project/plugins.sbt`:

```scala
addSbtPlugin("io.github.davidgregory084" % "sbt-tpolecat" % "0.1.11")
```

If necessary you can filter out scalac options that are unhelpful in the REPL from user-defined tasks or scopes.

By default the plugin only applies this filtering to the `console` task in the `Compile` and `Test` configurations.

```scala
scalacOptions.in(Tut) ~= filterConsoleScalacOptions
```

### Caveat

I can't promise this plugin will work for old minor releases of Scala. It has been tested with:

* 2.13.1
* 2.12.10
* 2.11.12
* 2.10.7

and Dotty versions:

* 0.21.0

### License

All code in this repository is licensed under the Apache License, Version 2.0.  See [LICENSE](./LICENSE).
