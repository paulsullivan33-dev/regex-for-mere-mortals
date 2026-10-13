# Real-World Recipes

Patterns you'll actually use, each verified. Copy freely.

## Log lines: extract the fields

```text
2026-10-13 14:22:01 ERROR disk full on /dev/sda1
2026-10-13 14:23:44 INFO cleanup started
```

```python
>>> log = "2026-10-13 14:22:01 ERROR disk full on /dev/sda1"
>>> m = re.search(r"(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) (\w+) (.*)", log)
>>> m.groups()
('2026-10-13', '14:22:01', 'ERROR', 'disk full on /dev/sda1')
```

Verified. Date, time, level, message — the `(.*)` at the end grabs
"the rest." Then filter:

```python
>>> [l for l in logs if re.search(r"^.*\bERROR\b", l)]   # error lines only
```

## CSV-ish data: split carefully

```python
>>> re.split(r"\s*,\s*", "a, b ,c")
['a', 'b', 'c']    # commas with optional surrounding spaces
```

Verified. (For *real* CSV with quoted commas, use Python's `csv`
module — that's the parser rule from chapter 1.)

## Find all numbers, including decimals

```python
>>> re.findall(r"\d+(?:\.\d+)?", "pi is 3.14, count 42")
['3.14', '42']
```

Verified. The `(?:\.\d+)?` is an optional decimal part — note the
escaped dot.

## Dates: find them, then validate properly

```python
>>> re.findall(r"\b\d{4}-\d{2}-\d{2}\b", "from 2026-10-13 to 2026-11-01")
['2026-10-13', '2026-11-01']
```

Verified. But `\d{2}` matches `99` — this finds date-*shaped*
text, including `2026-99-99`. **Finding** with regex, **validating**
with real code:

```python
from datetime import date
>>> date.fromisoformat("2026-10-13")   # works
datetime.date(2026, 10, 13)
>>> date.fromisoformat("2026-99-99")   # raises ValueError — not a date
Traceback (most recent call last):
ValueError: month must be in 1..12
```

The pattern narrows the haystack; the library judges. Same
principle applies to emails (next section).

## Email: the honest answer

There is no regex that perfectly validates email addresses — the
spec allows absurdities like `" "@example.com`. The pragmatic
approach:

```python
>>> bool(re.fullmatch(r"[^@\s]+@[^@\s]+\.[^@\s]+", "paul@example.com"))
True
>>> bool(re.fullmatch(r"[^@\s]+@[^@\s]+\.[^@\s]+", "not-an-email"))
False
```

Verified. "Something @ something . something, no spaces" catches
typos, which is all client-side validation should do. **Real**
validation is sending the email and seeing if it arrives.

## Renaming files: the shell one-liner

```bash
ls *.jpeg | sed -E 's/\.jpeg$/.jpg/'     # preview the new names
```

`sed -E 's/pattern/replacement/'` — the `s` command substitutes.
Preview first (as shown), then loop to actually rename. (Bulk
renaming *for real* deserves a script with a dry run — see [Bash
for Mere
Mortals](https://paulsullivan33-dev.github.io/bash-for-mere-mortals/).)

## Validation vs. extraction (the mantra)

- **Extraction:** `re.search` / `findall`, no anchors — *find* the
  pattern in the wild.
- **Validation:** `re.fullmatch` or `^...$` — the *whole* input
  must be the pattern.

Say which one you mean before you write the pattern. Half of regex
confusion is answering the wrong question.

**Next:** [Regex Flavors](flavors.md)
