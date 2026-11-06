# Module 10 Cheatsheet

## Environments

```bash
python -m venv .venv
source .venv/bin/activate              # Windows: .venv\Scripts\activate
pip install -r requirements.txt
pip install -e ".[dev]"                # your own project, editable, with dev tools
pip freeze > requirements-lock.txt     # exact versions you actually used
```

In a notebook: `%pip install pkg` (not `!pip`), and `!{sys.executable} -m tool` to run tools with the kernel's Python.

| file | line | meaning |
|---|---|---|
| requirements / pyproject | `numpy>=1.26` | a range that works |
| lock file | `numpy==2.0.2` | exactly what was used |

## Layout

```
project/
├── pyproject.toml   README.md
├── package_name/    __init__.py, data.py, model.py, cli.py
├── tests/           conftest.py, test_*.py
├── configs/         *.json
└── data/ models/    (in .gitignore)
```

```python
%load_ext autoreload
%autoreload 2                          # pick up edits to .py files
```

## pytest

```python
import pytest

@pytest.fixture
def data():
    return np.random.default_rng(0).normal(size=(50, 3))

def test_mean_is_zero(data):
    assert standardize(data).mean(axis=0) == pytest.approx(0, abs=1e-12)

def test_bad_input_raises():
    with pytest.raises(ValueError, match="2D"):
        standardize(np.ones(3))

@pytest.mark.parametrize("n", [1, 10, 1000])
def test_shapes(n): ...

def test_writes_file(tmp_path): ...    # built-in fixture: a fresh temporary folder
def test_logs(caplog): ...             # built-in fixture: captured log messages
```

```bash
pytest -q                 # quiet
pytest -x                 # stop at the first failure
pytest -k scaling         # only tests whose names match
pytest --tb=short         # short tracebacks
```

`conftest.py` fixtures are shared by all test files. `autouse=True` runs a fixture around every test. Put global state back after a test changes it.

**Worth testing:** shapes, known values on tiny inputs, invariants, edge cases (empty, one row, constant column, NaN), inputs not modified, clear errors, same seed gives the same result. When you find a bug, write the failing test first.

## Logging

```python
log = logging.getLogger(__name__)                 # in library code: just this
log.info("loaded %d rows from %s", n, path)       # arguments, not f-strings
log.exception("failed")                            # inside except: adds the traceback

# in the application, once:
logger = logging.getLogger("my_package")
for h in list(logger.handlers):                   # no duplicates when re-run
    logger.removeHandler(h); h.close()
logger.setLevel(logging.DEBUG)
screen = logging.StreamHandler(sys.stdout); screen.setLevel(logging.INFO)
file = logging.FileHandler("run.log")              # gets everything
for h in (screen, file):
    h.setFormatter(logging.Formatter("%(asctime)s %(levelname)s %(name)s: %(message)s"))
    logger.addHandler(h)
```

Levels: DEBUG < INFO < WARNING < ERROR < CRITICAL.

## Errors

```python
class DataError(ValueError): ...
raise DataError(f"missing columns: {missing}") from e
```

Validate at the boundaries. Catch specific exceptions. No bare `except:`. No `assert` for input checks (`python -O` removes them).

## Config

```python
@dataclass(frozen=True)
class Config:
    lr: float = 0.1
    seed: int = 0
    def __post_init__(self):
        if self.lr <= 0: raise ValueError(...)

unknown = set(values) - {f.name for f in fields(Config)}   # reject typos
config = replace(config, seed=3)                           # changed copy
asdict(config)                                             # for saving
```

## CLI

```python
def main(argv=None):
    parser = argparse.ArgumentParser(description=__doc__)
    sub = parser.add_subparsers(dest="command", required=True)
    t = sub.add_parser("train")
    t.add_argument("--config", type=Path)
    t.add_argument("--seed", type=int)
    t.add_argument("--overwrite", action="store_true")
    t.set_defaults(func=train)
    args = parser.parse_args(argv)       # None: real command line; a list: tests
    return args.func(args)

if __name__ == "__main__":
    main()
```

`[project.scripts] name = "package.cli:main"` in `pyproject.toml` makes it a terminal command.

## Reproducibility

```python
rng = np.random.default_rng(seed)        # pass it around; never np.random.seed
hashlib.sha256(data_bytes).hexdigest()   # data fingerprint (read big files in chunks)
platform.python_version(), np.__version__
datetime.now(timezone.utc).isoformat()
```

Save config + metrics + versions + data fingerprint with every result. Refuse to overwrite a run. Write files atomically: build the content, write to a temp file, then `Path.replace`.

## Profiling

```python
start = time.perf_counter(); f(); time.perf_counter() - start
%timeit f()

profiler = cProfile.Profile()
result = profiler.runcall(f, *args)
pstats.Stats(profiler).strip_dirs().sort_stats("tottime").print_stats(10)

tracemalloc.start(); f(); _, peak = tracemalloc.get_traced_memory(); tracemalloc.stop()

@functools.lru_cache(maxsize=None)       # repeated calls with the same arguments
```

`tottime`: inside the function itself. `cumtime`: including what it calls. Make it right, measure, speed up the slow part, check it's still right. Process big data in batches to bound memory.

## Never commit

`.venv/`, `__pycache__/`, `.ipynb_checkpoints/`, data, model weights, `work/`, `.env` and anything with a password or API key.
