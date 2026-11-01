# Module 2 Cheatsheet

## Lists

```python
xs.append(x)          # add one item
xs.extend(other)      # add every item from other
xs.insert(i, x)       # slow at the front: O(n)
xs.pop()              # remove last, O(1)
xs.pop(0)             # remove first, O(n), use a deque instead
xs.remove(x)          # first match only, ValueError if missing
xs.index(x), xs.count(x)

xs.sort()             # in place, returns None
ys = sorted(xs)       # new list
sorted(runs, key=lambda r: (-r.acc, r.time))   # multi-key: acc high to low, then time

first, *rest = xs
grid = [[0] * 3 for _ in range(3)]   # not [[0] * 3] * 3

import copy
copy.deepcopy(nested_config)          # .copy() is shallow
```

## Tuples

```python
shape = 28, 28, 3        # commas make the tuple
h, w, c = shape          # unpack
a, b = b, a              # swap
(5,)                     # one-item tuple needs the comma

from collections import namedtuple
Prediction = namedtuple("Prediction", ["label", "score"])
p = Prediction("cat", 0.9); p.label
```

## Dictionaries

```python
d.get(k, default)        # no KeyError
k in d                   # checks keys
d.pop(k, default)        # remove and return
d.items(), d.keys(), d.values()

merged = {**defaults, **overrides}    # right side wins
merged = defaults | overrides          # 3.9+
defaults.update(overrides)             # modifies defaults in place

d.setdefault(k, []).append(v)
{v: k for k, v in d.items()}           # invert
list(dict.fromkeys(items))             # dedupe, keep order
```

Keys must be hashable: numbers, strings, tuples. Not lists or dicts.

## Sets

```python
s = set()                # {} is an empty dict
s.add(x); s.discard(x)
a | b    # union
a & b    # intersection
a - b    # in a, not in b
a ^ b    # in exactly one
a <= b   # subset

oov = set(test_tokens) - set(train_tokens)
leaked = set(train_ids) & set(test_ids)
```

## Speed

| Operation | list | dict / set | deque |
|---|---|---|---|
| `x in c` | O(n) | O(1) | O(n) |
| index `c[i]` | O(1) | n/a | O(n) middle |
| add at end | O(1) | O(1) | O(1) |
| add/remove at front | O(n) | n/a | O(1) |

If you check `x in something` inside a loop, `something` should be a set or dict.

## collections

```python
from collections import Counter, defaultdict, deque

c = Counter(labels)
c.most_common(3); c["missing"]   # 0, no KeyError
c.update(more); c1 + c2; c.total()

groups = defaultdict(list)
groups[label].append(x)
per_class = defaultdict(Counter)
per_class[label].update(tokens)
# careful: reading groups[missing] creates the key; use `in` to check

window = deque(maxlen=100)       # keeps the last 100
window.append(x); window.appendleft(x); window.popleft()
```

## heapq

```python
import heapq
heapq.nlargest(5, scores.items(), key=lambda kv: kv[1])
heapq.nsmallest(5, items)

heap = []
heapq.heappush(heap, (priority, item))
priority, item = heapq.heappop(heap)    # smallest first
```

## Handy idioms

```python
for a, b in zip(words, words[1:]): ...        # consecutive pairs
max(counts, key=counts.get)                   # key with the biggest value
word_to_id.get(token, unk_id)                 # unknown words
[s + [pad] * (n - len(s)) for s in seqs]      # pad to length n
```

## Which one?

| Need | Use |
|---|---|
| ordered items, index by position | `list` |
| fixed record or composite key | `tuple` / `namedtuple` |
| lookup by key | `dict` |
| fast membership, dedupe, compare | `set` |
| counting | `Counter` |
| grouping | `defaultdict(list)` |
| queue or last-N window | `deque` |
| top-k or priority queue | `heapq` |
