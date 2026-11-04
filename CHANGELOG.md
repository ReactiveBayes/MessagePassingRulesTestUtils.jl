# Changelog

All notable changes to MessagePassingRulesTestUtils.jl are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.1] - 2026-11-04

### Fixed
- Make the `tables:failures` test independent of how Distributions prints distributions, which changed in Distributions v0.25.133.

## [1.0.0] - 2026-10-06

The first release: the test tooling of the ReactiveMP ecosystem's rule packages (tables, verification
against the node, derivative checks and the rule-coverage gate), developed in the
[ReactiveMP](https://github.com/ReactiveBayes/ReactiveMP.jl) repository, whose history this
repository keeps.

[Unreleased]: https://github.com/ReactiveBayes/MessagePassingRulesTestUtils.jl/compare/v1.0.1...HEAD
[1.0.1]: https://github.com/ReactiveBayes/MessagePassingRulesTestUtils.jl/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/ReactiveBayes/MessagePassingRulesTestUtils.jl/releases/tag/v1.0.0
