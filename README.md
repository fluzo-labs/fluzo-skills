# Fluzo skills

English | [Español](README.es.md)

Original, reusable instructions for designing and building Fluzo developer experiences. Skills guide an agent; they are not an application, enforcement sandbox, or unattended workflow.

## Catalog

| Skill | Use it for |
| --- | --- |
| [tui-design](skills/tui-design/SKILL.md) | Designing, implementing, and reviewing terminal interfaces, including agent sessions, safe approvals, streaming results, accessibility, and automation |
| [fluzo-deterministic-testing](skills/fluzo-deterministic-testing/SKILL.md) | Isolated Rust fixtures, strict simulators, controlled scheduling, and independent regression evidence |
| [fluzo-rust-boundaries](skills/fluzo-rust-boundaries/SKILL.md) | Resolved Cargo dependency paths, feature configurations, state ownership, and application protocols |

All three skills are available as reviewed instruction sources, not a versioned release. The Rust procedures have been checked with positive and negative disposable Cargo fixtures; this is not an agent-behavior evaluation or native Fluzo E2E test. TUI validation remains static only. No release, installer, bundle, or CI pipeline is provided.

## Use a local copy

Copy the complete `skills/tui-design/` directory from a reviewed revision into the skill directory supported by your agent, for example `.agents/skills/tui-design/` when that host supports it. Preserve its `LICENSE` and `references/`, check for an existing installation before copying, and confirm that the agent discovers the skill. Do not copy the repository root as a skill.

For either Rust skill, use the same complete-folder procedure with its catalog name. Their MIT licenses retain the original Jose Corral notice as well as the Fluzo adaptation notice. Do not overwrite an existing consumer installation or migrate it until a reviewed published revision is available.

Discovery and optional frontmatter fields depend on the host. Copies do not update themselves. Review subsequent changes explicitly; this collection does not download instructions or alter permissions at runtime.

Example messages to your agent:

- "Use tui-design to review our terminal interface. Report concrete findings without changing files."
- "Use tui-design to propose a task inspector that works at 80 by 24 cells and in a narrow terminal. Do not implement yet."
- "Implement the approved terminal interaction using the existing framework. Test cancellation, untrusted output, and non-interactive mode. Do not commit or publish."

- "Use fluzo-deterministic-testing to review cancellation tests and propose a strict offline fixture. Do not change files."
- "Use fluzo-rust-boundaries to inspect allowed dependency paths and supported feature configurations. Report graph evidence separately from ownership findings."

## Scope and dependencies

The skill uses the consumer's existing framework and tools. It includes integration checkpoints for Go, Rust, Python, and TypeScript, without requiring all four ecosystems or downloading version-specific examples. API claims must be checked against the installed dependency version.

The references cover interaction contracts, terminal safety, agent sessions, framework integration, and verification. They remain inside the skill folder; no other skill is required. Screenshots and recordings are optional and require approved existing tooling and reviewed content.

The Rust skills discover the consumer's architecture and test commands. Their Fluzo profiles are conditional examples, not mandatory crate names or Python scripts. They do not install dependencies or access real inference services. The local `crushrc` uses an already installed rust-analyzer 1.98.0 with offline Cargo, build scripts and proc macros disabled, and no save-time compiler checks; it is not a requirement for using the skills or a sandbox. Its activation in Crush remains to be checked after reopening this project.

## Maintenance

Read the [complete usage guide](docs/USAGE.md) for installation, inputs/outputs, examples for all three skills, approval boundaries, composition, local LSP setup, and publication.

See [AGENTS.md](AGENTS.md) for authoring rules and [validation guidance](docs/VALIDATION.md) for checks and their limits. Repository maintenance documentation is in Spanish; skill instructions and references are in English. Skill outputs follow the consumer's language, or the user's language when no convention is defined.

See the [Rust research and implementation plan](docs/RESEARCH-RUST-SKILLS.md) and [fixture evidence](docs/RUST-VALIDATION.md) for scope, results, and remaining limitations.

## License

[MIT](LICENSE). Each distributable skill includes its own license copy so standalone redistribution retains the notice.
