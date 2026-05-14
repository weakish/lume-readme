# AGENTS.md

Guidance for coding agents working on this repository.

## Project overview

This is a Deno/TypeScript library that provides a Lume plugin. The plugin
rewrites homepage-like source pages (by default `README`) so Lume emits
directory home URLs, while preserving explicit user-configured URLs.

Key files:

- `readme.ts` — plugin implementation and exported helper functions.
- `mod.ts` — public re-export entry point.
- `test/readme_test.ts` — unit tests for helpers and plugin behavior.
- `deno.json` — import map and Deno tasks.
- `.github/workflows/test.yml` — CI expectations.

## Environment and tooling

Use Deno, not Node/npm, for this project.

Common commands:

```sh
deno test
deno lint
deno fmt --check
deno fmt
```

Before returning changes, run at least:

```sh
deno fmt --check && deno lint && deno test
```

If formatting fails, run `deno fmt`, then rerun checks.

## Code style

- Keep the library dependency-light and compatible with Deno import maps.
- Prefer small pure helper functions; export helpers when tests or users benefit
  from direct coverage.
- Preserve explicit user URLs. The plugin should only rewrite Lume's
  auto-generated URL for matching homepage source files.
- Keep option defaults in `defaults` and document option behavior in both code
  comments and `README.md` when behavior changes.
- Path matching should be conservative:
  - `src.path` values are extension-less.
  - Homepage basename matching is case-insensitive.
  - Include/exclude path rules should avoid partial directory matches.
- Do not introduce Node-specific APIs, build steps, or package-manager lockfiles
  unless the project is intentionally migrated.

## Testing expectations

- Add or update tests in `test/readme_test.ts` for every behavior change.
- Cover both pretty URLs and non-pretty URLs when URL generation changes.
- Cover edge cases involving root paths, nested paths, case-insensitive homepage
  names, include/exclude filters, and explicit URL preservation.
- Mock Lume interfaces minimally, as current tests do; avoid pulling in full
  Lume site construction unless necessary.

## Public API and docs

- `mod.ts` is the public entry point. Keep exports intentional and
  backwards-compatible where possible.
- Update `README.md` when changing options, defaults, ordering requirements, or
  observable URL behavior.
- If changing plugin ordering assumptions, update the "Plugin ordering"
  documentation.

## Git hygiene

- Keep commits focused and descriptive.
- Do not commit generated coverage directories/files such as `cov_profile` or
  `cov_profile.lcov` unless explicitly requested.
- Check `git status --short` before committing.
- Do not set AI agents or LLM models as authors or co-authors. Add an
  `Assisted-by: AGENT_NAME:MODEL_VERSION` line to the commit message instead.
