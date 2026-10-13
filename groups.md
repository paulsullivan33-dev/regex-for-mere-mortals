# Groups

**Parentheses** do two jobs: group things together, and *capture*
what they match so you can use it.

## Grouping

```python
>>> re.findall(r"ab+", "ab abb")       # + applies to b only
['ab', 'abb']
>>> re.findall(r"(ab)+", "ab abab")    # + applies to the GROUP
['ab', 'ab']
```

Hmm — that second result looks odd (`['ab', 'ab']` instead of
`['ab', 'abab']`). That's `findall`'s quirk with groups, explained
below. The grouping itself works: `(ab)+` matches `abab` as one
match.

## Capturing: keep what you matched

Groups numbered from 1, left to right:

```python
>>> m = re.search(r"(\d{4})-(\d{2})-(\d{2})", "date: 2026-10-13")
>>> m.group(1), m.group(2), m.group(3)
('2026', '10', '13')
>>> m.group(0)
'2026-10-13'    # group 0 is the whole match
```

All verified. This is *extraction*: pull the year, month, day out
of the date separately.

## The findall quirk

`findall` with groups returns the *groups*, not the whole matches:

```python
>>> re.findall(r"(\d{4})-(\d{2})-(\d{2})", "2026-10-13 and 2025-01-01")
[('2026', '10', '13'), ('2025', '01', '01')]    # tuples of groups!
```

Verified. If you wanted whole dates, that's the bug — and the fix
is **non-capturing groups**: `(?:...)` groups without capturing:

```python
>>> re.findall(r"(?:\d{4})-(?:\d{2})-(?:\d{2})", "2026-10-13 and 2025-01-01")
['2026-10-13', '2025-01-01']
```

Verified. **Rule:** use `(?:...)` unless you actually want the
captured text. It also reads better — "I'm grouping, not keeping."

## Named groups: capture with labels

```python
>>> m = re.search(r"(?P<year>\d{4})-(?P<month>\d{2})", "2026-10")
>>> m.group("year"), m.group("month")
('2026', '10')
```

Verified. `(?P<name>...)` — clunky syntax, wonderful readability.
In a pattern with five groups, names beat counting parentheses.

## Backreferences: match the same text again

`\1` means "whatever group 1 matched":

```python
>>> re.search(r"(\w+), \1", "hello, hello").group()
'hello, hello'      # \1 must equal "hello" again
>>> re.search(r"(\w+), \1", "hello, world") is None
True
```

Verified. Real uses: finding doubled words (`\b(\w+) \1\b`),
matching quote pairs (`(['"])(.*?)\1` — the closing quote must match
the opening one).

## sub: replace using groups

```python
>>> re.sub(r"(\d{4})-(\d{2})-(\d{2})", r"\3/\2/\1", "2026-10-13")
'13/10/2026'
```

Verified. `re.sub` replaces matches; `\1` in the replacement
inserts group 1. Date reformatting in one line — this is the
"find-and-replace with superpowers" that sells regex to skeptics.

**Next:** [Alternation](alternation.md)
