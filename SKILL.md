---
name: volven-debt-php
description: Review PHP code for technical debt that creates future cost. Use when asked to find PHP maintenance risk, weak array contracts, mixed boundaries, magic workflow strings, misleading PHPDoc, static helper coupling, or shallow tests.
---

# Vølven PHP Debt Review

Vølven sees the PHP code that will hurt later.

Use this skill with the base `volven-debt` skill. The base skill decides what counts as debt. This PHP variant names the PHP-specific pressure points worth checking.

Do not report style issues, ordinary type errors, or mechanical cleanup already owned by Pint, PHPStan, Psalm, Rector, or project coding standards. Vølven is here for future cost, not indentation theatre.

## PHP Debt Signals

Look for real debt in these places:

- weak arrays used as domain models across service, controller, command, or job boundaries
- `mixed`, `array`, or untyped parameters on public or shared code paths where the shape matters
- magic strings for states, roles, permissions, event names, command names, or workflow transitions
- static helpers hiding dependencies that tests or callers cannot see
- untyped collections crossing service boundaries
- service classes that collect unrelated procedural workflows
- PHPDoc that lies to appease PHPStan or Psalm instead of describing the real contract
- `@phpstan-ignore*` and `@psalm-suppress` comments without a concrete reason or repayment path
- tests that only prove a method returns something, while the boundary contract remains invisible

## Hidden Contracts

Be extra suspicious when PHP code depends on:

- array keys that are only documented in fixtures or test setup
- call order that is not enforced by the type system or constructor
- comments that describe required state instead of code validating it
- one class knowing another class' internal string values
- mocks that encode behavior production code never states

## Reporting Rules

Use the base finding format.

Set `language: php` unless the finding is framework-specific. Use `language: laravel` for Laravel lifecycle, Eloquent, queue, policy, config, or container debt.

Prefer small repayment steps:

- add a value object or enum at the boundary first
- validate one important array shape before it spreads
- replace one magic string family with a named contract
- move one hidden dependency into the constructor
- rewrite one misleading test around behavior instead of implementation trivia

## Ignore If

Do not report a finding when:

- the code is generated, temporary migration glue, or a one-off script with no shared contract
- the weak shape is immediately validated at the edge
- the suppression is explained, linked to tracked work, and narrow
- the mess is ugly but local and cheap to delete later
