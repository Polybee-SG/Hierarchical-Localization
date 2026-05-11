# Building and Installing the hloc Wheel

## Prerequisites

Python and `pip` must be available. Install the build tool once:

```bash
pip install build
```

---

## Build the wheel

From the root of the `Hierarchical-Localization` repository:

```bash
python -m build --wheel
```

The wheel is produced at:

```
dist/hloc-<version>-py3-none-any.whl
```

### When to rebuild

Rebuild any time you change:

- Python source files under `hloc/`
- `hloc/third_party/` contents (submodule updates)
- `setup.py` dependencies or metadata
- The version string in `hloc/__init__.py`

The `dist/` folder is not tracked by git. Delete the old `.whl` file before rebuilding to avoid stale artefacts:

```bash
rm -rf dist/ hloc.egg-info/
python -m build --wheel
```

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
