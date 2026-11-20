# Universal agent template

This is the concise, platform-independent agent template to guide agents on behaving in a structured and efficient way. Runtime harness adapters adapts and extends based on the core defined principles, with conflicts should be explicitly addressed during bootstraping the session.

The template uses placeholders in `{{...}}`. The harness adapter must define
them and document the mapping. Values may be supplied through environment
variables during bootstrap; if a required value is missing, the adapter should
ask the user or fail explicitly instead of guessing.

## Role

You are an agent working in an isolated or scoped environment. Research,
execute, test, and build using the tools available within the authorized scope.

Verify ideas and tasks with the accessible tools when possible. Be creative
with the available tools, including simple bottom-up probes for uncertain
problems. Report what you could not verify.

## Main scopes

### {{ROOT_DIR}}

Shared reference root. This is where universal resources are bootstrapped and
read by agents.

- Agents treat this root as READ_ONLY.
- The host/bootstrap process may write to it (for example, syncing shared
  resources).
- Runtime outputs and scratch files do not belong here.
- Shared root: shared reference/config resources. Examples: `skills/`,
  `extensions/`, prompt templates

### {{WORK_DIR}}

Agent working directory.

By default:

- Agent-produced outputs are written here.
- Ownership and isolation by default at prompt level; harness adapter can enhances.

## Directory template

The directory names and entry sets below are conventional. A harness adapter
may rename, relocate, omit optional entries, or map them to native locations.
The adapter must document the mapping.

### .agent || .{{HARNESS_TOOL}}

Conventional contract directory.

- For hierachy reference only. Optional for workdir.
- This is preferrable to provide harness adapter more clarified scope of agent-related refs. Another layout should be provided by the harness adapter if needed.

Entries:

- `AGENTS.md`: reference role/scope at the shared root; optional lane override
  in a workdir. If the harness uses this contract, this is the canonical agent
  guide; otherwise the adapter maps it to the harness's native guide.
- Reference/config resources. Prefer to store/load mainly from {{ROOT_DIR}}, may have {{WORK_DIR}} exceptions.
- Harness adapter extended config/references.

### .runtime

Agent runtime directory. Specifically define for consistency during agent operations.
Should only presents in {{WORK_DIR}}.

- `VERSIONS.md` (recommended with complex, iterative problem solving): append-only execution log. Bumping a
  version requires user verification; temporary versions may be hashed. Keep
  logs concise and finalized; do not include back-and-forth discussions.
- `artifacts/`: durable, versioned outputs. Trunk-based:
  `project-v0.1`, or `-YYYYMMDDHHmm` when no version is given.
- `tmp/`: optional scratch output. Use `tmp/` and rely on OS cleanup when possible. Harness adapter may override the choice for non-durable artifacts.

## Harness adapter

An adapter is required for each harness. It must define:

- the values of `{{ROOT_DIR}}`, `{{WORK_DIR}}`, and `{{HARNESS_TOOL}}`;
- abstraction on how the `.agent` and hierachy contract overall are mapped to the harness's native files; Focus on resource refs to optimize agent scans.

If the adapter cannot satisfy a required entry, it should document the
exception and ask the user for a decision rather than silently dropping it.

## Hierarchy

```text
{{ROOT_DIR}}
  .agent | .{{HARNESS_TOOL}} / # reference contract only
    AGENTS.md
    # shared reference/config; harness-owned
    skills/                  # example
    extensions/              # example
    ...

{{WORK_DIR}}
  .agent | .{{HARNESS_TOOL}} / # optional specific references
    AGENTS.md
    ...
  .runtime/
    VERSIONS.md              # optional version tracking
    artifacts/               # durable outputs
    tmp/                     # optional; adapter may map to /tmp
```
