# MessagePassingRulesTestUtils

[![CI](https://github.com/ReactiveBayes/MessagePassingRulesTestUtils.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/ReactiveBayes/MessagePassingRulesTestUtils.jl/actions/workflows/CI.yml)
[![Docs: stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://reactivebayes.github.io/MessagePassingRulesTestUtils.jl/stable/)
[![Docs: dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://reactivebayes.github.io/MessagePassingRulesTestUtils.jl/dev/)
[![Coverage](https://codecov.io/gh/ReactiveBayes/MessagePassingRulesTestUtils.jl/graph/badge.svg)](https://codecov.io/gh/ReactiveBayes/MessagePassingRulesTestUtils.jl)
[![Code style: Runic](https://img.shields.io/badge/code_style-%E1%9A%B1%E1%9A%A2%E1%9A%BE%E1%9B%81%E1%9A%B2-black)](https://github.com/fredrikekre/Runic.jl)

Tools for testing message passing rules on their own, without a graph, for packages that define
nodes and rules with [MessagePassingRulesBase](https://github.com/ReactiveBayes/MessagePassingRulesBase.jl):

- tables of cases for message rules, marginal rules and average energies, each case also run
  with inputs of other float types, through `rule!` for an in-place rule and on a poisoned
  scratch;
- verification of a message rule, and its log scale, against the node's log-density,
  integrated numerically;
- derivative checks, ForwardDiff through a rule against finite differences;
- the rule-coverage gate, which fails a suite when a rule has no test.

```julia
import Pkg; Pkg.add("MessagePassingRulesTestUtils")
```

```julia
using MessagePassingRulesBase, MessagePassingRulesTestUtils, BayesBase, ExponentialFamily, Test

struct Gaussian end   # out ~ Normal(μ, v)
Gaussian(μ, v) = NormalMeanVariance(μ, v)

@define_factor_node(node = Gaussian, type = Stochastic, interfaces = [:out, :μ, :v])

@define_message_update_rule(
    node = Gaussian, target = :out,
    args = (m[:μ]::NormalMeanVariance, m[:v]::PointMass),
    logscale = 0,
    body = (args) -> NormalMeanVariance(mean(args.m[:μ]), var(args.m[:μ]) + mean(args.m[:v])),
)

@testset "Gaussian: out" begin
    @test_message_update_rule(
        node = Gaussian, target = :out,
        cases = [(m = (μ = NormalMeanVariance(1.0, 2.0), v = PointMass(0.5)),) => NormalMeanVariance(1.0, 2.5)],
    )
    @verify_message_update_rule(node = Gaussian, target = :out, m = (μ = NormalMeanVariance(1.0, 2.0), v = PointMass(0.5)))
end

isempty(check_rule_coverage(@__MODULE__))   # true: every rule has a test
```

- Documentation: <https://reactivebayes.github.io/MessagePassingRulesTestUtils.jl/stable/>, with two tutorials and
  a page on reading a failure. `make docs` builds it locally, into `docs/build`.
- Tests: `make test` runs the suite as CI does; `make test test_args="name:coverage"` selects
  items by tag, by name or by file. `make help` lists the other targets: `docs`, `docs-serve`,
  `format`, `check-format`, `test-fast`.
- Depends on MessagePassingRulesBase, BayesBase, Distributions, ForwardDiff and HCubature, and
  on no engine. A test dependency: nothing at run time needs it. Julia 1.10 or later. MIT
  licence.
