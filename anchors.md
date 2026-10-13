# Anchors

A pattern matches *anywhere* in the string by default. **Anchors**
pin it to a position: the start, the end, or a word edge.

## ^ and $: start and end

```python
>>> re.search(r"^cat", "cat sat").group()      # starts with cat
'cat'
>>> re.search(r"^cat", "the cat") is None      # "the cat" doesn't START with cat
True
>>> re.search(r"cat$", "the cat").group()      # ends with cat
'cat'
>>> re.search(r"^cat$", "cat").group()         # the ENTIRE string is "cat"
'cat'
>>> re.search(r"^cat$", "cats") is None
True
```

All verified. `^cat$` means "the whole string is exactly `cat`" —
that's the shape of *validation* ("is this input a 5-digit zip?":
`^\d{5}$`).

**Careful:** `^` means "start" here, but "not" inside brackets
(`[^0-9]`). Same symbol, two jobs — context tells you which. (And
in most flavors `^` also means start-of-*line* with the multiline
flag — see [Flags](flags.md).)

## \b: word boundaries

`\b` matches the *edge* between a word character and a non-word
character — the invisible fence around words:

```python
>>> re.findall(r"\bcat\b", "cat concatenate wildcat")
['cat']    # only the standalone word; "concatenate" untouched
```

Verified. This is the fix for chapter 1's problem (`cat` matching
inside `concatenate`). Whenever you mean "the word X," write
`\bX\b`.

```python
>>> re.findall(r"\b\d+\b", "order 42 ships")
['42']
```

## Putting it together: validation

```python
>>> bool(re.search(r"^\d{5}$", "90210"))
True
>>> bool(re.search(r"^\d{5}$", "902101"))
False
>>> bool(re.search(r"^\d{5}$", "abcde"))
False
```

All verified. **Search vs. validation is the key distinction:**
`re.search(r"\d{5}", ...)` *finds* five digits anywhere (extraction);
`re.search(r"^\d{5}$", ...)` *requires* the whole string to be five
digits (validation). Most "my regex doesn't work" complaints are
this mix-up.

## fullmatch: the explicit version

Python has `re.fullmatch` — the pattern must match the *entire*
string, no anchors needed:

```python
>>> bool(re.fullmatch(r"\d{5}", "90210"))
True
```

Verified. Use it when you mean validation; it says what you mean.

**Next:** [Groups](groups.md)
