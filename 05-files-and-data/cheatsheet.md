# Module 5 Cheatsheet

## pathlib

```python
from pathlib import Path

p = Path("data") / "raw" / "penguins.csv"
p.name, p.stem, p.suffix, p.parent        # 'penguins.csv', 'penguins', '.csv', data/raw
p.exists(), p.is_file(), p.is_dir()
p.parent.mkdir(parents=True, exist_ok=True)
p.read_text(encoding="utf-8")
p.write_text(text, encoding="utf-8")
p.stat().st_size
p.resolve()                                # absolute path
p.relative_to(root).as_posix()             # 'raw/penguins.csv' with forward slashes
sorted(root.rglob("*.csv"))                # recursive search, sorted for reproducibility
p.unlink()                                 # delete a file
tmp.replace(p)                             # atomic rename
```

## Text files

```python
with open(path, "w", encoding="utf-8") as f:     # "w" wipes the file!
    f.write("line\n")

with open(path, encoding="utf-8") as f:
    for line in f:                               # lazy, one line at a time
        line = line.rstrip("\n")
```

| mode | meaning |
|---|---|
| `r` | read (default) |
| `w` | write, wipes existing content |
| `a` | append |
| `x` | create, error if it exists |
| `b` | binary (`rb`, `wb`) |

## Encodings

```python
"Adélie".encode("utf-8")                 # str -> bytes
data.decode("utf-8")                     # bytes -> str
open(path, encoding="utf-8-sig")         # strips Excel's BOM if present
# if UTF-8 fails, try "latin-1" or "cp1252"
```

## CSV

```python
import csv

with open(path, encoding="utf-8-sig", newline="") as f:
    for row in csv.DictReader(f):        # row is a dict of STRINGS
        ...

with open(path, "w", encoding="utf-8", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=columns)
    writer.writeheader()
    writer.writerows(rows)               # None is written as "None" unless you convert it
```

Never parse CSV with `split(",")`. Always pass `newline=""`.

## Missing values

```python
MISSING = {"", "na", "n/a", "nan", "null", "none", "?", "-"}
value = None if text.strip().lower() in MISSING else float(text)
```

Check for missing markers *before* converting: `float("nan")` succeeds. Try `int()` before `float()` to keep whole numbers as ints. Compute fill values from training data only.

## Cleaning patterns

```python
SPECIES = {"adelie": "Adelie", "adélie": "Adelie", "gentoo": "Gentoo"}
SPECIES.get(raw.strip().lower())                 # None for anything unknown

if not 1000 <= grams <= 10000: grams = None      # sanity range from domain knowledge

datetime.strptime(text, "%Y-%m-%d").date().isoformat()

if len(fields) != len(header): skip_and_log()    # broken rows
signature = tuple(normalize(r[k]) for k in keys) # duplicates, via a set
```

Log every issue. Don't fix things silently.

## JSON

```python
import json

json.dumps(obj, indent=2, ensure_ascii=False)
json.loads(text)
json.dump(obj, f, indent=2)
json.load(f)
json.dumps(obj, allow_nan=False)        # error on NaN instead of writing invalid JSON
```

| Python | after a JSON round trip |
|---|---|
| tuple | list |
| `{0: "a"}` | `{"0": "a"}` |
| `float("nan")` | `NaN` (not valid JSON) |
| dataclass | use `asdict()` first |

## JSON Lines

```python
with open(path, "a", encoding="utf-8") as f:
    f.write(json.dumps(record) + "\n")

def read_jsonl(path):
    with open(path, encoding="utf-8") as f:
        for line in f:
            if line.strip():
                yield json.loads(line)
```

## Checksums

```python
import hashlib

def sha256_of(path):
    h = hashlib.sha256()
    with open(path, "rb") as f:
        for chunk in iter(lambda: f.read(65536), b""):
            h.update(chunk)
    return h.hexdigest()
```

## Safe saving

```python
tmp = path.with_name(path.name + ".tmp")
tmp.write_text(text, encoding="utf-8")
tmp.replace(path)                       # atomic: never a half-written file
```

## pickle

Never `pickle.load` a file you didn't create yourself: loading a pickle can run arbitrary code. Prefer JSON or CSV for anything shared or long-lived.

## Reproducible pipelines

- Fixed seeds, and `sorted()` before shuffling or iterating over groups
- No manual steps
- A `metadata.json` describing what was done
- Read outputs back and check them
- Record a checksum of the input data
