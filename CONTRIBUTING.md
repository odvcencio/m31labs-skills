# Contributing

This repository is open source, but the skill corpus is editorial material for M31 Labs products. Contributions should make agents more accurate, more useful, and less likely to overreach.

## What Belongs Here

- New production-ready skill bodies under `skills/`.
- Corrections to examples, references, and product usage guidance.
- Better activation descriptions that reduce false positives or false negatives.
- Root documentation that helps humans or agents consume the repo.

## What Does Not Belong Here

- Empty skill stubs under `skills/`.
- Private roadmap details or unreleased API promises.
- Promotional copy without concrete usage guidance.
- Contributor/internal implementation skills for product repos. Those are out of scope for v1.

## Adding A Skill

1. Copy `templates/SKILL.md` to `skills/using-<name>/SKILL.md`.
2. Replace the frontmatter with a specific `name` and trigger-focused `description`.
3. Keep the body provider-agnostic markdown.
4. Add references under `skills/using-<name>/references/` only when they provide real depth.
5. Add examples under `skills/using-<name>/examples/` only when they are complete and runnable.
6. Update README and AGENTS.md from Planned to Available.

## Skill Checklist

Before opening a pull request, verify:

- The `description` names concrete tasks that should trigger the skill.
- "When to reach" explains decision signals, not just product benefits.
- "What it is" gives a compact mental model.
- "Quick start" is current and copy-pasteable.
- "When NOT to use" is present and specific.
- All reference and example links resolve.
- Runnable examples have been tested against current product behavior.
- Claims are backed by public docs, product source, CLI output, or examples.
- The skill can be read by an agent with no private M31 Labs context.

## Review Standard

Activation descriptions are treated like API surface. Small wording changes can change when agents load a skill, so review them carefully.

Prefer small pull requests scoped to one skill or one root-document concern. Do not combine broad prose rewrites with product-behavior changes.
