# Contributing

Thanks for considering a contribution. Please open an issue before making a substantial change so the problem and design can be discussed.

## Development setup

- Python 3.10 or newer
- No runtime dependencies

```bash
python -m venv .venv
# Activate the environment, then:
python -m pip install -e .
python -m compileall -q src tests
python -m unittest discover -s tests -v
```

## Pull requests

- Keep changes focused and explain the motivation in the PR description.
- Add tests for changed behavior and regression tests for bugs.
- Update user-facing docs and the changelog when behavior changes.
- Do not commit credentials, generated reports containing private data, or large test artifacts.
- Prefer the standard library; discuss new dependencies before adding them.

## Reporting bugs

Include the operating system, Python version, exact command, a minimal sanitized input, and expected versus actual behavior. Never attach real secrets or private customer data.
