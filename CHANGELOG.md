# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Early Semantic Versioning](https://docs.scala-lang.org/overviews/core/binary-compatibility-for-library-authors.html#recommended-versioning-scheme) in addition to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.2]

### Fixed

* [#102](https://github.com/typelevel/sbt-tpolecat/issues/102) - a bug which resulted in scalac options contributed by other plugins being overwritten, most notably the Scala.js plugin. Huge thanks to [@sjrd](https://github.com/sjrd) for their work on [#126](https://github.com/typelevel/sbt-tpolecat/pull/126) which resolves this issue.

## [0.4.1]

### Added

* [#84](https://github.com/typelevel/sbt-tpolecat/pull/84) - An `advancedOption` overload with support for multiple argument options was added.
* [#88](https://github.com/typelevel/sbt-tpolecat/pull/88) - A Scalafix migration rule to migrate package names for the upcoming 0.5.0 release was added.

### Changed

* [#85](https://github.com/typelevel/sbt-tpolecat/pull/85) - `-Xcheckinit` was removed from the default option set. After discussion on issues [#10](https://github.com/typelevel/sbt-tpolecat/issues/10) and [#83](https://github.com/typelevel/sbt-tpolecat/issues/83) it has become apparent that although `-Xcheckinit` offers a lot of value for some users, it creates some very tricky problems for users that are writing async code. The initialization checking code this option produces introduces `@volatile` variables that can disguise memory visibility issues.
* [#88](https://github.com/typelevel/sbt-tpolecat/pull/88) - The project was restructured into a multi-module sbt build.

## [0.4.0] - 2022-07-15

NOTE: This release series will be the final one released under the group ID `io.github.davidgregory084`, and the final one using the package `io.github.davidgregory084`.

As of the 0.5.x series, this project will be released under the group ID `org.typelevel` and using the package `org.typelevel.sbt`.

If you are using [Scala Steward](https://github.com/scala-steward-org/scala-steward) to upgrade your libraries, an artifact migration has been provided to update the sbt-tpolecat group ID.

A Scalafix migration will be provided upon the release of `0.5.0` to migrate any usages of the previous package in your projects.

### Changed

* [#75](https://github.com/typelevel/sbt-tpolecat/pull/75) The `ScalacOptions` trait was made package-private. This is to ensure that new methods can be added to this trait without risking binary compatibility breakages.
* [#75](https://github.com/typelevel/sbt-tpolecat/pull/75) The `ScalacOption` constructor was changed from `(tokens: List[String], ...)` to `(option: String, args: List[String], ...)`. This is to ensure that multi-argument options can be filtered out regardless of which arguments were provided to the option.

## [0.3.3] - 2022-06-06

### Added

* Support for the [SIP-22](https://docs.scala-lang.org/sips/async.html) `-Xasync` option was added to the `ScalacOptions` DSL.
* Support for the `-Ybackend-parallelism` option was added to the `ScalacOptions` DSL. This option can be used to enable scalac to emit class files in parallel.
* Support for the `-release` option was added to the `ScalacOptions` DSL. This option can be used to compile for a specific version of the Java platform.

### Fixed

* [#74](https://github.com/typelevel/sbt-tpolecat/issues/74) - a bug in filtering of multiple-argument options. This filtering was broken in the attempt to fix [#60](https://github.com/typelevel/sbt-tpolecat/issues/60). Unfortunately this means that if users append to `ThisBuild / scalacOptions`, those appended options will no longer be inherited by other configurations, e.g. `Test / scalacOptions`.
* [#78](https://github.com/typelevel/sbt-tpolecat/issues/78) - a bug where options were not filtered out of `Test / scalacOptions` as expected. This had the same underlying cause as [#74](https://github.com/typelevel/sbt-tpolecat/issues/74).
* [#77](https://github.com/typelevel/sbt-tpolecat/issues/77) - the JDK version was not being set as expected by the setup-scala action. Resolved by switching to setup-java instead.

## 0.3.2 - 2022-06-06 [YANKED]

Please do not use this release - GitHub accepted a tag push but not its corresponding commit data, so the release proceeded against a commit that is not on the main branch.

## [0.3.1] - 2022-04-25

### Changed

* The dependency on the [sbt-partial-unification](https://github.com/fiadliel/sbt-partial-unification) plugin was dropped. This is because support for partial unification was backported to Scala 2.11.11, so all versions of Scala supported by this plugin either enable partial unification by default or provide a compiler option to enable it. This means that this plugin will no longer enable partial unification on 2.11.x patch releases older than 2.11.11.

## [0.3.0] - 2022-04-22

### Added

* A `defaultConsoleExclude` option set was added to the `ScalacOptions` DSL. This option set can be used for filtering out compiler options that trigger warnings in the Scala REPL.

### Changed

* `tpolecatConsoleOptionsFilter` was replaced by `tpolecatExcludeOptions`. The use of a function to filter out console options did not interact well with the new method of setting `scalacOptions` in sbt-tpolecat [0.2.3](https://github.com/typelevel/sbt-tpolecat/releases/tag/v0.2.3). Please append to `tpolecatExcludeOptions` in the `console` task you wish to configure instead, e.g.

    ```scala
    IntegrationTest / console / tpolecatExcludeOptions ++= ScalacOptions.defaultConsoleExclude
    ```

## [0.2.3] - 2022-04-14

### Added

* Begin keeping [this changelog](./CHANGELOG.md).
* Added `-Xsource` (Scala 2.x) and `-source` (Scala 3.x) early migration settings to the ScalacOptions DSL.

### Fixed

* [#60](https://github.com/typelevel/sbt-tpolecat/issues/60) - a bug in setting `scalacOptions` where it was set using `:=` rather than appended to via `++=`. This prevented scope delegation via `ThisBuild / scalacOptions` from working for some users.

## [0.2.2] - 2022-03-30

### Fixed

* Ensure that all keys with dependencies are derived settings, so that e.g. `Test / tpolecatScalacOptions` can be used to manipulate `Test / scalacOptions`.
* Add a `toString` to `ScalacOption`.

## [0.2.1] - 2022-03-30

### Added

* Enable [MiMa](https://github.com/lightbend/mima) checks in GitHub Actions workflows.
* Set `versionScheme` to clarify version compatibility claims.

### Fixed

* Apply `-Xfatal-warnings` regardless of version once more. Applying `-Werror` for Scala 2.13.x causes problems for users who currently filter out `-Xfatal-warnings` from `scalacOptions` explicitly.

### Changed

* Expanded usage instructions to guide users toward the `ScalacOptions` DSL.

## [0.2.0] - 2022-03-30

### Added

* Development, CI and release modes for setting options differently according to the context.
* Add a simple `ScalacOptions` DSL for setting options in each mode.
* Add mode-setting commands `tpolecatDevMode`, `tpolecatCiMode`, `tpolecatReleaseMode`.
* Environment variable checks in order to decide which mode to enable on startup.

### Changed

* The signature of `scalacOptionsFor` exported via this plugin's `autoImport` - it now requires a `Set` of all selected `ScalacOptions` for the current mode in addition to the current Scala version.
* The `filterConsoleScalacOptions` function exported via this plugin's `autoImport` was renamed to `tpolecatConsoleOptionsFilter` for consistency with other keys provided by the plugin.

### Removed

* The `validFor` function that was previously exported via this plugin's `autoImport`.

[Unreleased]: https://github.com/typelevel/sbt-tpolecat/compare/v0.4.2...HEAD
[0.4.2]: https://github.com/typelevel/sbt-tpolecat/compare/v0.4.1...v0.4.2
[0.4.1]: https://github.com/typelevel/sbt-tpolecat/compare/v0.4.0...v0.4.1
[0.4.0]: https://github.com/typelevel/sbt-tpolecat/compare/v0.3.3...v0.4.0
[0.3.3]: https://github.com/typelevel/sbt-tpolecat/compare/v0.3.1...v0.3.3
[0.3.1]: https://github.com/typelevel/sbt-tpolecat/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/typelevel/sbt-tpolecat/compare/v0.2.3...v0.3.0
[0.2.3]: https://github.com/typelevel/sbt-tpolecat/compare/v0.2.2...v0.2.3
[0.2.2]: https://github.com/typelevel/sbt-tpolecat/compare/v0.2.1...v0.2.2
[0.2.1]: https://github.com/typelevel/sbt-tpolecat/compare/v0.2.0...v0.2.1
[0.2.0]: https://github.com/typelevel/sbt-tpolecat/compare/v0.1.22...v0.2.0
