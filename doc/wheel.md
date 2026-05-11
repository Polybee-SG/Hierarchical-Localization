# Building and Installing the hloc Wheel

## Prerequisites

- `uv` installed (https://docs.astral.sh/uv/getting-started/installation/) — matches the rest of this monorepo.
- Python 3.12 available to `uv` (the existing wheel was produced with CPython 3.12.3).

`Hierarchical-Localization/` does not declare `[project]` metadata in its `pyproject.toml`, so we create a standalone build venv with `uv venv` instead of `uv sync`.

---

## Build the wheel

From `Hierarchical-Localization/`:

```bash
# 1. Wipe any prior artefacts so the new wheel is clean
rm -rf dist/ build/ hloc.egg-info/ .venv-build

# 2. Create an isolated build venv pinned to Python 3.12
uv venv --python 3.12 .venv-build

# 3. Install the PEP 517 build front-end into it
uv pip install --python .venv-build/bin/python build

# 4. Build the wheel
.venv-build/bin/python -m build --wheel
```

The wheel is produced at:

```
dist/hloc-<version>-py3-none-any.whl
```

where `<version>` is taken from `__version__` in `hloc/__init__.py` (current: `1.5.1`).

### When to rebuild

Rebuild any time you change:

- Python source files under `hloc/`
- `hloc/third_party/` contents (submodule updates)
- `setup.py` dependencies or metadata
- The version string in `hloc/__init__.py`

The `dist/` folder is not tracked by git. Re-run the four commands above; step 1 takes care of stale artefacts.

---

## Install on another machine

Copy the `.whl` file to the target machine, then:

```bash
pip install hloc-<version>-py3-none-any.whl
```

This installs `hloc` and all its dependencies declared in `requirements.txt`.

### Note on the LightGlue dependency

`requirements.txt` includes:

```
lightglue @ git+https://github.com/cvg/LightGlue
```

`pip` fetches it directly from GitHub during install, so the target machine needs internet access and `git` installed.

---

## Verify the installation

```bash
python -c "import hloc; print(hloc.__version__)"
```

Confirm `third_party` files are present:

```bash
python -c "
import pathlib, hloc
tp = pathlib.Path(hloc.__file__).parent / 'third_party'
print(list(tp.iterdir()))
"
```
