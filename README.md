# .agent
Universal agent setup, designed to be smoothly referenced for all stack choices.

# WHAT
This is an universal, stack-independent designed for agents to operate in a consistent way across platforms.
Followed by some wide-aligned setup, with some tweaks.

# WHY 
There are countless harness and related tools are built, with same or different ideas and approaches around AI agents. The hard part is that we may not come soon to a future when all of those tools shared the same interfaces and core architecture (maybe they shouldn't). It become challenging for engineers or AI users in general to adopt to those specific setup of those tools, ultimately to operate the agent in an expected and desired way. This often collides in:

- How agents should bootstrap resources, skills, and so on.
- Scope of access during runtime.
- Shared and private resources.
- And so on.

This is built as a consistent template to smoothly align those rules. Ideally we can setup a perfect environment with perfect set of scopes, with accurate setup for each stack of agents, but that sounds unrealistic and high-maintaince. This is a best-attempt to align on those rules across stacks for optimizing agent performance (avoiding redundant loops and so on).

# WHEN
- You want a universal setup for your stack.
- You value high-customization solutions, tweaked with your specific use cases.
- You work with multiple agents at a time, and want an efficient way to orchestrate them.
- You prefer not to lock into the same set of vendors, and migration shouldn't sound painful to resist.

# HOW

The template is designed with some required arguments that you should be injecting to during bootstrap to setup a universal, multi-layered environment.

The arguments are placed inside {{}}, agent should ask for those values when bootstraping if not yet explicit defined.
To be more versatile, arguments are accepted to be passed implicitly via environment variables.

The agent should prioritize to respect the original template.

## Main scopes

Define scopes (working directories) that agents are operating (read/write).

### {{ROOT_DIR}}

Root directory shared across agents. This is where shared, universal resources are bootstraped and fetched by agents. 
READ_ONLY by agents.

### {{WORK_DIR}}

Agent private working directory. 
By default:

- Agent produced outputs should be written here.
- Should be accessible by the authorized agent ALONE.

## Directory template

Consitent template across {{ROOT_DIR}}, {{WORK_DIR}}, or any sub-level working directories. 
Bottom-up priority: lowest level rule wins if presented.

### .agent

Include main templates on how agent should inherite and operate.

- `AGENTS.md`: Role, scope and main guidelines.
- `SKILLS.md`: Universal skills and instructions.
- `VERSIONS.md`:
  - Initialize when start producing solutions. 
  - Store append-only version logs, follow trunk-based template. 
  - Bumping version need user verification. Temp version can be hashed.
  - Keep logs concise, finalized, don't include back-n-forth discussions. 
- `tmp/`: Generated temporary outputs.
- `artifacts/`: 
  - Generated artifacts, with clear naming, version included. e.g "prj-A-v0.1".
  - If no version is given, use `-YYYYMMDDHHmm`.

### .{{HARNESS_TOOL}}

Same template as `.agent`, thus can be extended for specific tool choices.

Other specific settings and add-ons are encouraged to stay inside the same dir, following the original template rules of the tools.
If conflicts happen, during bootstraping or runtime, notify user explicitly for final decisions.

Directory with sessions stored should be respected to be accessible by authorized agents only (default the session-owner agent).

Sample add-ons:

- `extensions | skills | prompts`: Harness-specific extensions, skills, add-ons and so on.

# HIERACHY

A brief view on the expected hierachy.

<details>
<summary>Expected hierarchy</summary>

```text
{{ROOT_DIR}}
  .agent
    AGENTS.MD
    SKILLS.MD
  .{{HARNESS_TOOL}} // sessions should be stored here
    SKILLS.MD
    extensions/
...
{{WORK_DIR}}
  .agent
    AGENTS.MD // override the role if needed
    SKILLS.MD
    VERSION.MD
    tmp/
    artifacts/
  .{{HARNESS_TOOL}}
    SKILLS.MD
    extensions/
```

</details>
