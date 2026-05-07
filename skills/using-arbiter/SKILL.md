---
name: using-arbiter
description: |
  Use this skill when the user is modeling governed decisions, entitlements,
  pricing tiers, feature flags, workflow gates, eligibility logic, expert
  inference, continuous decision loops, or runtime overrides that would
  otherwise become nested conditionals, scattered config, OPA/Rego, CEL, or a
  generic rules engine.
---

# Using Arbiter

## When to reach for Arbiter

Reach for Arbiter when the code is making governed outcomes: approve this transaction, show this variant, block this request, compute this tax bracket, route this workflow, or emit a regulated action.

Use the lightest Arbiter modality that matches the decision shape:

- `rule`: many matching governed outcomes.
- `strategy`: exactly one governed route with an explicit fallback.
- `flag`: governed variant resolution with rollout and segments.
- `expert rule`: forward-chaining fact mutation until quiescence.
- `arbiter`: a long-lived loop over fact sources, workers, chains, and outcome handlers.

Good triggers: repeated `if/else` decision trees, business logic with owners or tickets, rollout percentages, kill switches, prerequisites, explainability requirements, audit trails, and decisions that need to move independently of deploys.

## What Arbiter is

Arbiter is a compact language and runtime for governed outcomes. The language compiles `.arb` source into bytecode and evaluates it through a VM. Its mental model is closer to "SQL for decisions" than "policy language for authorization": the source declares typed inputs, facts, outcomes, segments, rules, strategies, flags, expert inference, and continuous arbiter loops.

Arbiter is not only authorization. It can express authz, but the broader fit is decision modeling where governance, rollout, explainability, and runtime override behavior matter.

## Quick start

Write the smallest governed decision first:

```arb
outcome ShippingDecision {
    method: string
    cost: number
}

segment domestic {
    order.country == "US"
}

rule FreeShipping {
    when segment domestic {
        order.total >= 35
    }
    then ShippingDecision {
        method: "standard",
        cost: 0,
    }
}
```

Validate and evaluate through the CLI when available:

```bash
arbiter check shipping.arb
arbiter eval shipping.arb --data '{"order":{"country":"US","total":42}}'
```

For services, publish bundles once and evaluate many times over gRPC. The public SDKs are thin clients over the same API: Go module APIs in the main repo, plus Node, Python, and Rust SDKs under `sdks/`.

## When NOT to use Arbiter

Do not introduce Arbiter for a single local conditional with no governance, no audit requirement, and no need to change behavior independently of code.

Do not use it as a general-purpose programming language. Keep data loading, side effects, network calls, and UI behavior in the host application. Arbiter should decide outcomes from facts and context.

Do not hide arbitrary transport or worker behavior inside decision source. Continuous arbiters can route outcomes to sources, sinks, and workers, but the host runtime owns those capabilities.

Do not model vague product strategy before the business decision vocabulary is clear. First name the inputs, outcomes, owners, and failure modes.

## Going deeper

- Use rules for stateless match-and-emit decisions.
- Use strategies when exactly one route must win.
- Use flags when the outcome is a named variant with rollout and segment behavior.
- Use expert rules when facts should create or retract other facts across rounds.
- Use continuous arbiters when polling, source freshness, worker results, chain edges, and delivery reliability are part of the runtime story.
- Public source: [odvcencio/arbiter](https://github.com/odvcencio/arbiter)
