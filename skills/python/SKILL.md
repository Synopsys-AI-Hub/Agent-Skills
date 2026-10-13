---
name: pyansys-python-style
description: Applies PyAnsys Python coding style conventions — PEP 8 rules, naming conventions, formatting tool configuration (Ruff, pre-commit), docstring style, and deprecation patterns — when writing or reviewing Python code for Synopsys/Ansys libraries.
license: Complete terms in LICENSE.txt
---

# PyAnsys Python Coding Style

## Overview

Enforces the PyAnsys coding style for all Python code in Ansys/Synopsys libraries. These rules build on PEP 8 and align with the conventions used by NumPy, SciPy, and pandas.

**Keywords**: python, coding style, PEP 8, linting, formatting, ruff, pre-commit, deprecation, naming conventions, docstrings, PyAnsys, Ansys, Synopsys

**Goals:**
- Prevent common programming errors
- Limit code complexity
- Maintain readable, maintainable code
- Establish consistent style across all PyAnsys libraries
- Provide an objective basis for code review

## PEP 8 Rules

### Imports

- Place all imports at the top of the file, after module comments and docstrings, before global variables and constants.
- Never import inside functions — surface `ImportError` at load time.
- Group imports in this order, separated by a blank line:
  1. Standard library
  2. Related third-party packages
  3. Local or library-specific
- Sort alphabetically within each group.
- One import per line, except modules from the same package may share one `from` line.
- Prefer absolute imports for readability and reliability:

```python
# Correct
from ansys.mapdl.core.plotting import general_plotter

# Avoid
from .plotting import general_plotter
```

- Never use wildcard imports (`from module import *`).

### Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Modules | Short lowercase, underscores allowed | `my_module.py` |
| Packages | Short lowercase, no underscores | `ansys-mapdl-core` |
| Classes | CamelCase, no underscores | `FieldAnalysis2D` |
| Functions/Methods | Lowercase with underscores | `assign_material()` |
| Variables | Lowercase with underscores | `node_count` |
| Constants | Uppercase with underscores | `MAX_ITERATIONS` |
| Private methods | Double leading underscore | `__internal_method()` |
| Protected methods | Single leading underscore | `_helper_method()` |
| Dunder methods | Double underscores both sides | `__init__()` |

**General rules:**
- Use descriptive, unambiguous, pronounceable, and searchable names.
- Make meaningful distinctions. Replace magic numbers with named constants.
- Never encode type information in names (no Hungarian notation).
- Never use `l`, `O`, or `I` as single-character variable names.

### Indentation and Line Breaks

- **Four spaces** per indentation level. Never use tabs.
- Two blank lines before and after top-level function and class definitions.
- One blank line before each method inside a class.
- Use blank lines to separate logical sections of code.
- Keep `if`/`elif`/`else` branches together without blank lines between them.

### Maximum Line Length

- **Source code:** 100 characters maximum.
- **Docstrings and comments:** 72 characters maximum.
- Break long list comprehensions across lines, or use a classic loop:

```python
# Acceptable
squares = [
    i * i
    for i in range(10)
]

# Also acceptable
squares = []
for i in range(10):
    squares.append(i * i)
```

### Comments

- Write complete sentences. Capitalize the first word (unless it is a lowercase identifier).
- Keep comments current — comments that contradict the code are worse than none.
- Make code self-documenting with clear names first.
- Don't be redundant or add obvious noise.
- Don't add closing-brace comments.
- Delete unused code instead of commenting it out.
- Explain intent, clarify code, and warn of consequences.
- Document non-obvious behavior so future developers don't remove code they think is unnecessary:

```python
# Reset the cache before writing output to avoid stale entries
# being carried over from the previous solve step.
self._cache.clear()
```

- **Inline comments:** Use sparingly. Separate from the statement by two spaces. Avoid obvious ones.
- **Domain context:** PyAnsys libraries span multiple physics domains, so readers may lack the author's background — comment generously.

### Docstrings

- A docstring is the first statement in a module, function, class, or method (becomes `__doc__`).
- Write docstrings for all public modules, functions, classes, and methods.
- Private methods need a comment describing what they do, not a full docstring.
- Use triple double quotes (`"""`).
- One-line docstrings: opening and closing quotes on the same line.
- Multi-line docstrings: closing quotes on their own line.
- Follow the **NumPy docstring convention** for formatting.

### Programming Recommendations

**Booleans and comparisons:**
```python
# Correct
if is_valid:
if not my_list:
if value is not None:

# Avoid
if is_valid == True:
if len(my_list) == 0:
if not value is None:
```

**Strings:**
```python
# Correct
if filename.endswith(".py"):

# Avoid
if filename[-3:] == ".py":
```

**Don't read/write the Windows registry.** This is dangerous and complicates cross-platform deployment.

**DRY (Don't Repeat Yourself):** Keep each piece of knowledge in one authoritative place:
```python
# Correct — single function, unit-testable
def fahrenheit_to_kelvin(fahrenheit):
    """Convert Fahrenheit to Kelvin."""
    return (fahrenheit - 32) * 5 / 9 + 273.15
```

**Nesting:**
- Maximum recommended depth for conditional blocks: **four levels**.
- If more is needed, extract small, reusable, unit-testable functions.
- Avoid loops nested more than **two levels** deep.

**List comprehensions:**
```python
# Readable — use it
squares = [i * i for i in range(10)]

# Complex logic — use a helper function
def is_consonant(char):
    return char.isalpha() and char.lower() not in "aeiou"

consonants = [c for c in text if is_consonant(c)]
```

### Security

- Libraries should be secure and follow practices that avoid or mitigate risks.
- Especially important for libraries that accept user input (web services).
- Reference: "10 Unknown Security Pitfalls for Python" (Sonar source blog).

## Formatting Tools

### Ruff (Primary Linter and Formatter)

Ruff replaces Flake8, Black, and isort. Written in Rust for speed.

**Install:** `pip install ruff`

**Minimum `pyproject.toml` configuration:**

```toml
[tool.ruff]
line-length = 100
fix = true

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
docstring-code-format = true

[tool.ruff.lint]
select = [
    "E",    # pycodestyle errors
    "W",    # pycodestyle warnings
    "D",    # pydocstyle
    "F",    # pyflakes
    "I",    # isort
    "N",    # pep8-naming
    "PTH",  # use pathlib
    "TD",   # flake8-todos
]

[tool.ruff.lint.ignore]
"TD003" = true  # Missing issue link in TODO

[tool.ruff.lint.pydocstyle]
convention = "numpy"

[tool.ruff.lint.isort]
combine-as-imports = true
force-sort-within-sections = true
```

**Migration guidance:** Add lint rules step by step when migrating a project to Ruff. Resolve errors each rule triggers before adding the next.

### Pre-commit Configuration

**Minimum `.pre-commit-config.yaml`:**

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: vX.Y.Z
    hooks:
      - id: ruff
      - id: ruff-format

  - repo: https://github.com/codespell-project/codespell
    rev: vX.Y.Z
    hooks:
      - id: codespell

  - repo: https://github.com/PyCQA/pydocstyle
    rev: X.Y.Z
    hooks:
      - id: pydocstyle
        additional_dependencies: [toml]
        exclude: "tests/"

  - repo: https://github.com/ansys/pre-commit-hooks
    rev: v0.2.4
    hooks:
      - id: add-license-headers
```

**Install and run:**
```bash
python -m pip install pre-commit
pre-commit install
pre-commit run --all-files --show-diff-on-failure
```

### License Headers

- Use the `add-license-headers` hook from `ansys/pre-commit-hooks`.
- Default scope: PROTO files in any directory, Python files in `src`, `examples`, and `tests`.
- License: MIT (from the repository's `MIT.txt`).

### Code Coverage (pytest-cov)

**Minimum `pyproject.toml` configuration:**

```toml
[tool.coverage.run]
source = ["ansys.<product>"]

[tool.coverage.report]
show_missing = true
```

### Tox Configuration

**Minimum `tox.ini`:**

```ini
[tox]
envlist = style, py310, py311, py312, py313, py314
skip_missing_interpreters = true
isolated_build = true

[testenv]
deps = -r requirements/requirements_tests.txt
commands = pytest {env:PYTEST_MARKERS:} {env:PYTEST_EXTRA_ARGS:}
setenv = PYTHONUNBUFFERED = yes

[testenv:style]
deps = pre-commit
commands = pre-commit run --all-files --show-diff-on-failure

[testenv:doc]
deps = -r requirements/requirements_doc.txt
commands = sphinx-build -vW -b html doc/source doc/build/html
```

**Coverage variant:**
```bash
pytest --cov=ansys.product --cov-report=term --cov-report=xml --cov-report=html
```

**Usage:**
```bash
tox -e style    # Check style
tox -e py       # Run tests
tox -e doc      # Build docs
tox              # Run all environments
```

### CI Integration

- **pre-commit.ci:** Free for public repos. Ask the PyAnsys Core team to enable it.
- **Ansys reusable actions:** `ansys/actions/code-style@vX.Y.Z` with `python-version` input.

**Conventional commits config for pre-commit.ci:**
```yaml
ci:
  autofix_commit_msg: "chore: auto fixes from pre-commit hooks"
  autoupdate_commit_msg: "chore: pre-commit automatic update"
  autoupdate_schedule: weekly
```

## Deprecation Best Practices

Python has no official guidance on deprecation. These are PyAnsys conventions.

### Core Rules

1. **Never make abrupt breaking changes** — warn first, then remove after one or two minor releases.
2. When deprecating a method, class, or function, either:
   - Have the old method call the new one and raise a warning, **or**
   - Raise an `AttributeError` if removing entirely.
3. Add a Sphinx `deprecated` directive in the old method's docstring.

### Pattern 1: Deprecate with Warning and Forwarding

```python
import warnings

class FieldAnalysis2D:
    def assignmaterial(self, *args, **kwargs):
        """Assign a material to an object.

        .. deprecated:: 0.4.0
           Use :func:`FieldAnalysis2D.assign_material` instead.
        """
        warnings.warn(
            "`assignmaterial` is deprecated. Use `assign_material` instead.",
            DeprecationWarning,
            stacklevel=2,
        )
        return self.assign_material(*args, **kwargs)

    def assign_material(self, *args, **kwargs):
        """Assign a material to an object."""
        ...
```

### Pattern 2: Remove Entirely

```python
class FieldAnalysis2D:
    def assignmaterial(self, *args, **kwargs):
        """Assign a material to an object.

        .. deprecated:: 0.4.0
           This method has been removed.
        """
        raise AttributeError(
            "`assignmaterial` has been deprecated and removed. "
            "Use `assign_material` instead."
        )
```

### Pattern 3: Custom DeprecationError

Python has no built-in deprecation error. Define one:

```python
class DeprecationError(RuntimeError):
    """Raised when a deprecated feature is used."""

    def __init__(self, message="This feature has been deprecated."):
        super().__init__(message)


# Usage
raise DeprecationError("`my_function` has been deprecated.")
```

### Docstring Directive

Always add the Sphinx directive to deprecated items:

```rst
.. deprecated:: X.Y.Z
   Use :func:`NewClass.new_method` instead.
```

## Quick Reference

| Rule | Value |
|---|---|
| Line length (code) | 100 characters |
| Line length (docstrings/comments) | 72 characters |
| Indentation | 4 spaces (no tabs) |
| Quote style | Double quotes |
| Import order | stdlib → third-party → local |
| Import style | Absolute preferred |
| Docstring convention | NumPy |
| Naming: modules | `lowercase_underscores` |
| Naming: packages | `lowercase` (no underscores) |
| Naming: classes | `CamelCase` |
| Naming: functions/variables | `lowercase_underscores` |
| Naming: constants | `UPPERCASE_UNDERSCORES` |
| Max nesting (conditionals) | 4 levels |
| Max nesting (loops) | 2 levels |
| Formatter/linter | Ruff |
| Docstring linter | pydocstyle (NumPy convention) |
| Spell checker | codespell |
| License | MIT via `add-license-headers` |
| Test runner | pytest with pytest-cov |
| Environment manager | tox |
