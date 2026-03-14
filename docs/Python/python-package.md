# Python Packaging Tutorial

1. Create a python project named `sample-project` as following:

```
sample-project/
├── pyproject.toml
└── src
    └── sample
        ├── __init__.py
        └── simple.py
```

- `pyproject.toml`

```toml
[build-system]
requires = ["setuptools"] # REQUIRED if [build-system] table is used
build-backend = "setuptools.build_meta" # If not defined, then legacy behavior can happen.

[project]
name = "sample-project" # REQUIRED, is the only field that cannot be marked as dynamic.
version = "0.1.0" # REQUIRED, although can be dynamic
description = "A sample Python project"
requires-python = ">=3.9"
license = { file = "LICENSE.txt" }
authors = [{ name = "A. Random Developer", email = "author@example.com" }]
dependencies = [] # List your project's dependencies here (e.g., "numpy")

# The following would provide a command line executable called `sample`
# which executes the function `main` from this package when invoked.
[project.scripts]
sample = "sample:main"
```

- `src/sample/__init__.py`

```python
def main():
    """Entry point for the application script"""
    print("Call your main application code here")
```

- `src/sample/simple.py`

```python
def add_one(number):
    return number + 1
```

2. Install your package locally

```
# install in editable mode
python -m pip install -e .
# install regularly
python -m pip install .
```

3. Test your package

```python
from sample.simple import add_one

print(add_one(10))
```

## Uploading your Project to PyPI

```
pip install build twine
# Packaging your project
python -m build --sdist
# Upload to PyPi
twine upload dist/*
```

#### Reference

- [Packaging and distributing projects](https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/)
- [Create a pure Python package](https://www.pyopensci.org/python-package-guide/tutorials/create-python-package.html)