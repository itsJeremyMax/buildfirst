# BuildFirst

**Just-enough engineering for fast product delivery.**

BuildFirst is an agent skill for building MVPs, prototypes, experiments, internal tools, and early-stage products. It gives your coding agent a simple discipline: build the smallest reasonable solution that works, validate it in proportion to the risk, and stop when the requested work is done.

> Build what is needed now. Nothing more.

Use it when you want a working feature without speculative architecture, unnecessary compatibility layers, or a small task growing into a rewrite. BuildFirst is a Markdown instruction set your agent reads; it has no runtime, service, or API key of its own.

## Install

With [Node.js and npm](https://nodejs.org/) and [Git](https://git-scm.com/) installed, run this from the project where you want to use the skill:

```sh
npx skills add itsJeremyMax/buildfirst
```

The [Skills CLI](https://github.com/vercel-labs/skills) supports Codex, Claude Code, Cursor, and other coding agents. It detects installed agents; use `--agent` below to select a specific one. Installation is scoped to the current project by default. Add `--global` to make it available across your projects:

```sh
npx skills add itsJeremyMax/buildfirst --global
```

You can also target an agent directly:

```sh
npx skills add itsJeremyMax/buildfirst --skill buildfirst --agent codex
npx skills add itsJeremyMax/buildfirst --skill buildfirst --agent claude-code
```

To inspect the available skill without installing it:

```sh
npx skills add itsJeremyMax/buildfirst --list
```

These commands install directly from this GitHub repository; BuildFirst does not need a separate npm package or build step.

## Use

Ask your agent to apply BuildFirst alongside a concrete task:

```text
Use the buildfirst skill to add a waitlist form that saves email addresses.
Build the smallest working flow and validate submission and duplicate handling.
```

For a change to existing code, state any compatibility requirements:

```text
Use the buildfirst skill to rename our internal createUser function to
registerUser and update its callers. Keep the public HTTP API unchanged.
```

If the agent does not see the new skill, start a fresh session in the project where you installed it and explicitly ask it to use `buildfirst`.

## What it changes

The skill encourages the agent to:

- Build the smallest end-to-end implementation that satisfies the current requirement.
- Prefer direct code and a small number of dependencies.
- Update controlled callers and remove obsolete internal interfaces when compatibility is unnecessary.
- Add tests, abstractions, and infrastructure when they address a concrete need or risk.
- Preserve meaningful security, data integrity, and contracts that real users depend on.
- Finish after sufficient targeted validation.

Here is how that translates into everyday tasks:

| Task | BuildFirst approach |
| --- | --- |
| Add one API endpoint | Use a direct handler and the existing persistence layer. Introduce more layers only if the endpoint needs them. |
| Rename an internal function | Update the function and controlled callers; remove the old name when no consumer needs it. |
| Add an MVP form | Build submission, relevant validation, and clear success/error states; check the main flow. |
| Change persisted production data | Preserve important data and use the smallest safe migration the change requires. |

Explicit user requirements and real operational needs still matter. Required public contracts, security checks, production data, and necessary tests remain part of the job. Describe those constraints in your request so the agent can keep its solution small without omitting them.

BuildFirst guides agent behavior; it does not enforce it. Review the resulting changes and validation as you would for any agent-assisted work.

Read the full instructions in [skills/buildfirst/SKILL.md](skills/buildfirst/SKILL.md).

## Manage your installation

List installed skills:

```sh
npx skills list
```

To refresh BuildFirst from GitHub, rerun its install command with the same agent and scope options you originally used.

Remove a project installation:

```sh
npx skills remove buildfirst
```

For a global installation, use `npx skills remove buildfirst --global`.

## Repository layout

```text
skills/
  buildfirst/
    SKILL.md    # Installable skill and canonical instructions
README.md       # Installation and usage
LICENSE         # MIT license
```

The skill folder uses the standard `SKILL.md` format with YAML `name` and `description` fields, which the Skills CLI discovers automatically.

## Contributing

Open an issue or pull request at [itsJeremyMax/buildfirst](https://github.com/itsJeremyMax/buildfirst). Keep changes focused on helping agents deliver working software with less unnecessary engineering. Include a concrete example when proposing a change to the guidance.

GitHub installation requires these files to be published in the repository. You can verify unpublished changes using the local commands below.

Edit `skills/buildfirst/SKILL.md` as the single source of truth. From a local checkout, check discovery before submitting a change:

```sh
npx skills add . --list
```

To try your local changes, run the following from a separate test project, replacing the path with your checkout location:

```sh
npx skills add /absolute/path/to/buildfirst --skill buildfirst --agent codex --yes
```

## License

[MIT](LICENSE) © 2026 itsJeremyMax.
