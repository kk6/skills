---
name: python-doc-sync
description: Review and minimally update Python project documentation after code changes. Use when an agent needs to check whether docstrings, inline comments, README files, or Markdown under docs/ still match the implementation, especially after modifying public APIs, behavior, configuration, side effects, constraints, examples, or user-facing workflows in a Python repository.
---

# Doc Sync

Keep implementation-adjacent documentation accurate without expanding documentation for its own sake. Prefer the repository's existing documentation style, audience, and maintenance level over introducing a new standard.

## Scope

Check documentation that can drift from Python code:

- Module, class, function, method, and property docstrings.
- Inline and block comments that explain behavior, assumptions, constraints, or workarounds.
- README files and Markdown under `docs/`.
- Usage examples, command snippets, configuration descriptions, API references, and behavior notes.

Do not turn this into a general documentation rewrite. Avoid adding docstrings to obvious private helpers, boilerplate, straightforward data containers, or code whose behavior is already clear from local naming and tests.

## Precedence

Before editing documentation, inspect and follow project-local instructions in this order when present:

1. User request.
2. Repository agent instructions such as `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, or similar files.
3. README or docs contributor guidance.
4. Existing docstring and Markdown style in nearby files.
5. Python ecosystem conventions appropriate to the project.

If instructions conflict, follow the more local or more explicit instruction and mention the conflict in the final response.

## Workflow

1. Identify the code delta.
   - Prefer `git diff`, `git status`, recent commits, or the files the user named.
   - Include tests and call sites when they clarify the intended behavior.

2. Build a documentation map.
   - Search changed symbols, commands, settings, exceptions, environment variables, endpoints, CLI flags, and user-visible terms across docstrings, comments, README files, and `docs/`.
   - Check nearby docs even when names changed indirectly.

3. Compare facts, not wording.
   - Verify documented parameters, return values, yielded values, exceptions, side effects, ordering, defaults, constraints, performance expectations, permissions, IO, network behavior, database behavior, and compatibility notes.
   - Treat examples as executable claims: commands, imports, snippets, file paths, settings, and expected output should still match the implementation.

4. Edit only stale or missing critical facts.
   - Update the smallest text range that restores accuracy.
   - Preserve tone, heading structure, docstring convention, Markdown formatting, and line wrapping where practical.
   - Prefer removing obsolete claims over adding caveats around them.

5. Validate the result.
   - Run relevant formatters, doc tooling, linters, or tests if the repository provides them and the change risk justifies it.
   - At minimum, re-read the edited docs against the changed implementation and check Markdown links or code fences touched by the edit.

## Docstring Guidance

Add or update docstrings primarily for:

- Public APIs and extension points.
- Non-obvious behavior, invariants, algorithms, or state transitions.
- Side effects such as file writes, network calls, database mutations, caching, logging, telemetry, scheduling, or subprocess execution.
- Important constraints such as accepted formats, ordering guarantees, concurrency assumptions, security boundaries, retries, timeouts, or compatibility limits.
- Exceptions or failure modes callers are expected to handle.

Avoid docstring churn:

- Do not add docstrings that merely restate names or type annotations.
- Do not document every private helper unless the behavior is subtle or depended on externally.
- Do not convert the repository to a different docstring style.
- Do not duplicate type hints unless the existing style expects it.

When a docstring exists, prefer correcting stale details over replacing the whole block.

## Comment Guidance

Keep comments only when they explain why code is shaped a certain way, not what each line does. Update or remove comments that reference old behavior, old dependencies, old workarounds, stale TODOs, or constraints that no longer apply.

If code and comments disagree, inspect tests and call sites before deciding whether the code or the comment is wrong. When only documentation is stale, update the comment without changing behavior.

## README and Docs Guidance

For README files and Markdown under `docs/`, check:

- Installation, setup, and configuration instructions.
- Public API examples and import paths.
- CLI commands, options, environment variables, and config keys.
- Feature descriptions, limitations, permissions, and side effects.
- Architecture notes that mention changed modules or data flow.
- Troubleshooting, migration, and compatibility guidance.

Prefer focused edits near the outdated claim. Avoid broad reorganization unless the existing structure prevents an accurate minimal update.

## Decision Rules

Make a documentation edit when:

- The implementation changed a documented fact.
- A public behavior changed and the nearest user-facing docs would mislead a reader.
- A subtle side effect or constraint was introduced and callers or maintainers need to know it.
- A code example, command, import path, setting, or expected output is now wrong.

Do not edit when:

- The documentation is merely less detailed than the code.
- The change only affects internal implementation and no existing doc claims are stale.
- The proposed edit would introduce a new style or maintenance burden.
- The only available statement would be speculative.

When uncertain, leave the documentation unchanged and report the uncertainty with the evidence checked.

## Final Response

Summarize:

- Which documentation surfaces were checked.
- Which files were updated and why.
- Any stale documentation found but intentionally left unchanged, with the reason.
- Validation performed, or why validation was not run.
