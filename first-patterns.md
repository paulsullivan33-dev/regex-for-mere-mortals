# First Patterns

## Literals: what you type is what you match

Most characters match themselves. The pattern `cat` matches the
three letters `cat` wherever they appear:

```python
import re

re.search(r"cat", "the cat sat")   # match! found "cat"
re.search(r"cat", "the dog sat")   # no match
```

**Check (verified):**

```python
>>> re.search(r"cat", "the cat sat").group()
'cat'
>>> re.search(r"cat", "the dog sat") is None
True
```

`re.search` scans the whole string for the pattern. `.group()`
shows what matched. (`r"..."` is a **raw string** — backslashes stay
backslashes. Always use raw strings for regex in Python; you'll see
why in [Escaping](escaping.md).)

Note: `cat` also matches inside `concatenate`. Regex doesn't know
about words — yet. ([Anchors](anchors.md) fixes that.)

## The dot: "any character"

`.` matches any single character (except a newline, by default):

```python
>>> re.search(r"c.t", "cat").group()
'cat'
>>> re.search(r"c.t", "cut").group()
'cut'
>>> re.search(r"c.t", "ct") is None   # . needs ONE character
True
```

All verified. The dot is your first *wildcard* — "I don't care what
goes here, something goes here."

## findall: every match, not just the first

```python
>>> re.findall(r"c.t", "cat cut cot cit")
['cat', 'cut', 'cot', 'cit']
```

`findall` returns a list of all non-overlapping matches. `search`
finds the first; `findall` finds them all. You'll use both
constantly.

## Try it in the shell

```bash
echo -e "cat\ncut\ndog" | grep -E "c.t"
```

**Check (verified):**

```text
cat
cut
```

`grep -E` uses "extended" regex — closest to what this guide
teaches. (Plain `grep` uses an older dialect where `+` and `?`
need backslashes. Save yourself the confusion: always `-E`.)

## What "match" means

A pattern doesn't have to match the *whole* string — it matches a
*substring* unless you say otherwise. `re.search(r"cat", "concatenate")`
matches. This surprises beginners exactly once, then becomes the
most useful thing about regex: you're usually hunting for a needle,
not validating the haystack.

**Next:** [Character Classes](character-classes.md)
