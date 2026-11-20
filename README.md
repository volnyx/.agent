# .agent

Universal agent setup, designed to be smoothly referenced for all stack choices.

## WHAT

This is a universal, stack-independent template for agents to operate in a
consistent way across platforms. It defines scopes, contract files, and
conventions; each harness adapter maps those concepts to its own native layout.

## WHY

There are countless agent harnesses, each with different interfaces and
architecture. That makes it hard to operate agents in an expected and desired
way across tools. The recurring collisions are:

- how agents bootstrap resources and skills;
- scope of access during runtime;
- shared vs private resources;
- where sessions, outputs, and temporary files live.

This template is a best-effort alignment on those rules. It intentionally does
not prescribe a specific harness layout.

## WHEN

- You want a universal setup for your stack.
- You value high-customization solutions tuned to your use cases.
- You work with multiple agents and want an efficient way to orchestrate them.
- You prefer not to lock into one vendor; migration should not be painful.

## HOW

The template defines a set of proposed hierachies and entries and harness adapter will figure out an efficient way to adapt to those priciples. 

See more in `AGENTS.md`.
