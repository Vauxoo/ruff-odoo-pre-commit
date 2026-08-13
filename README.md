# ruff-pre-commit (Vauxoo fork — `ruff-odoo`)

[![image](https://img.shields.io/pypi/v/ruff-odoo/0.16.2.8.svg)](https://pypi.python.org/pypi/ruff-odoo)
[![image](https://img.shields.io/pypi/l/ruff-odoo/0.16.2.8.svg)](https://pypi.python.org/pypi/ruff-odoo)
[![image](https://img.shields.io/pypi/pyversions/ruff-odoo/0.16.2.8.svg)](https://pypi.python.org/pypi/ruff-odoo)
[![Actions status](https://github.com/vauxoo/ruff-odoo-pre-commit/workflows/main/badge.svg)](https://github.com/vauxoo/ruff-odoo-pre-commit/actions)

A [pre-commit](https://pre-commit.com/) hook for [`ruff-odoo`](https://github.com/Vauxoo/ruff),
the Vauxoo fork of [Ruff](https://github.com/astral-sh/ruff) that adds the custom `ODOO` rule
group on top of all upstream rules.

Distributed as a standalone repository to enable installing `ruff-odoo` via prebuilt wheels from
[PyPI](https://pypi.org/project/ruff-odoo/).

Note: this fork uses bare version tags (e.g. `0.16.2.2`, no `v` prefix, matching upstream
astral-sh/ruff's current release-tag style). The `v*`-prefixed tags in this repo's history are
inherited from upstream `astral-sh/ruff-pre-commit` and install the upstream `ruff` package
instead.

### Using ruff-odoo with pre-commit

To run the linter and formatter via pre-commit, add the following to your `.pre-commit-config.yaml`:

```yaml
repos:
- repo: https://github.com/vauxoo/ruff-odoo-pre-commit
  # ruff-odoo version.
  rev: 0.16.2.8
  hooks:
    # Run the linter.
    - id: ruff-check
    # Run the formatter.
    - id: ruff-format
```

To enable lint fixes, add the `--fix` argument to the lint hook:

```yaml
repos:
- repo: https://github.com/vauxoo/ruff-odoo-pre-commit
  # ruff-odoo version.
  rev: 0.16.2.8
  hooks:
    # Run the linter.
    - id: ruff-check
      args: [ --fix ]
    # Run the formatter.
    - id: ruff-format
```

To select or ignore specific rules, pass the relevant Ruff arguments through `args`.
When using inline YAML lists, quote arguments that contain commas:

```yaml
repos:
- repo: https://github.com/vauxoo/ruff-odoo-pre-commit
  # ruff-odoo version.
  rev: 0.16.2.8
  hooks:
    # Run the linter.
    - id: ruff-check
      args: [ --fix, "--extend-select=I,E,ODOO", "--ignore=F401" ]
```

To avoid running on Jupyter Notebooks, remove `jupyter` from the list of allowed filetypes:

```yaml
repos:
- repo: https://github.com/vauxoo/ruff-odoo-pre-commit
  # ruff-odoo version.
  rev: 0.16.2.8
  hooks:
    # Run the linter.
    - id: ruff-check
      types_or: [ python, pyi ]
      args: [ --fix ]
    # Run the formatter.
    - id: ruff-format
      types_or: [ python, pyi ]
```

When running with `--fix`, the lint hook should be placed _before_ the formatter hook, and
_before_ Black, isort, and other formatting tools, as the fix behavior can output code changes
that require reformatting.

When running without `--fix`, the formatter hook can be placed before or after the lint hook.

### Using ruff-odoo with prek

If you prefer using [prek](https://github.com/j178/prek) instead of
pre-commit, you can define a `prek.toml` file with your hooks. Here's an example
equivalent to the `.pre-commit-config.yaml` configuration:

```toml
[[repos]]
repo = "https://github.com/vauxoo/ruff-odoo-pre-commit"
rev = "0.16.2.8" # ruff-odoo version.
hooks = [
  # Run the linter.
  { id = "ruff-check", args = ["--fix"], types_or = ["python", "pyi"] },

  # Run the formatter.
  { id = "ruff-format", types_or = ["python", "pyi"] },
]
```

See the section above on pre-commit for guidance on hook order when using `--fix`.

## Releasing

New `ruff-odoo` versions on PyPI are picked up automatically by the scheduled `main` workflow,
which runs `mirror.py`: it bumps `pyproject.toml`/`README.md`, commits, tags `<version>`,
and creates a GitHub release. It can also be run manually via `workflow_dispatch` or locally with
`uv run --no-project mirror.py` followed by `git push origin main --tags`.

## License

ruff-pre-commit is licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <https://opensource.org/licenses/MIT>)

at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in ruff-pre-commit by you, as defined in the Apache-2.0 license, shall be
dually licensed as above, without any additional terms or conditions.
