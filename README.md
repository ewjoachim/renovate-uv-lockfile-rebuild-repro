# renovate-uv-lockfile-rebuild-repro

Minimal reproduction: Renovate rebuilds and force-pushes a pep621/uv lock-file-only update branch on every run where the base branch has moved, even though the update itself is unchanged.

- `pyproject.toml`: `attrs>=23.1.0`, locked at 23.1.0 in `uv.lock`
- `renovate.json`: `rangeStrategy: "update-lockfile"`

Steps (Renovate 44.246.4, `LOG_LEVEL=debug`):

1. Run Renovate: it opens #1 and #2.
2. Run again without touching `main`: logs `Restarting processBranch() with a clean branch`, then `No file changes detected. Skipping commit`.
3. Push an unrelated commit to `main` and run again: both branches are force-pushed with an identical patch, only the parent changed.

An unrelated change on main.
