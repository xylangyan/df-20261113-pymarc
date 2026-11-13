# Reproduce the data-engineering environment

Source: https://github.com/edsu/pymarc/commit/cf33051421ac74389c1bc6d54921fb9612083d1b

This repository republishes the tracked files of that upstream commit under their original licenses.
Upstream CI workflows are omitted from the evaluation snapshot. This commit adds environment instructions
and the dependency lock. The legacy invalid Python version specifier >=3.6.* is normalized to >=3.6 in setup.py; library behavior is unchanged. The source license is BSD-2-Clause; original license and attribution files remain.

Python 3.11.16 and uv were used on Linux. From a fresh checkout:

```sh
uv venv --python 3.11.16 .venv
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 SERVER=NONE STRATEGY=MOCK_SYNC SETUPTOOLS_SCM_PRETEND_VERSION_FOR_PYTHON_CAN=4.6.1.dev0 SETUPTOOLS_SCM_PRETEND_VERSION_FOR_BAGIT=1.9.0.dev0
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider test
```

The selected tests use small fixtures or in-process substitutes. They need no remote service after dependency setup.
Start a fresh checkout and rebuild its virtual environment to restore initial state.
