# Character Classes

A **character class** matches *one* character from a set. It's the
answer to "a digit here, I don't care which one."

## Brackets: your own sets

```python
>>> re.findall(r"c[au]t", "cat cut cot")
['cat', 'cut']          # [au] = "a or u"; "cot" doesn't match
>>> re.findall(r"[a-z]+", "hello 123")
['hello']               # [a-z] = any lowercase letter
>>> re.findall(r"[a-zA-Z0-9]+", "Hello 123!")
['Hello', '123']        # letters and digits (+ explained next chapter)
```

Inside brackets, most special characters lose their powers: `[.]`
matches a literal dot, `[*]` a literal star. The exceptions: `^` at
the start *negates*, and `-` defines ranges:

```python
>>> re.findall(r"[^0-9]+", "abc123def")
['abc', 'def']          # [^...] = "anything EXCEPT these"
>>> re.findall(r"[0-9-]+", "2026-10-13")
['2026-10-13']         # - at the start/end is literal
```

All verified. (`[^0-9]` is "not a digit" — the `^` only negates when
it's first inside the brackets. Anywhere else, it's a literal
caret.)

## Shorthands: the ones you'll use daily

| Shorthand | Means | Same as |
|-----------|-------|---------|
| `\d` | A digit | `[0-9]` |
| `\D` | Not a digit | `[^0-9]` |
| `\w` | Word character (letters, digits, `_`) | `[a-zA-Z0-9_]` |
| `\W` | Not a word character | `[^a-zA-Z0-9_]` |
| `\s` | Whitespace (space, tab, newline) | `[ \t\n\r\f\v]` |
| `\S` | Not whitespace | |

(Capital = opposite. That's the whole mnemonic.)

```python
>>> re.findall(r"\d+", "order 42, qty 7")
['42', '7']
>>> re.findall(r"\w+", "hello_world 123")
['hello_world', '123']
>>> re.split(r"\s+", "a  b\tc")
['a', 'b', 'c']
```

All verified. Note `\w` includes underscore and digits — "word
character" is really "identifier character," which is what you want
when grabbing variable names and usernames.

**Careful:** `\d` matches only ASCII `0-9` in most flavors, but in
Python 3 it matches Unicode digits too (like `٣`). For log parsing
it never matters; for strict validation, use `[0-9]` to be explicit.

## Putting it together: a date

```python
>>> re.search(r"\d{4}-\d{2}-\d{2}", "meeting on 2026-10-13 at noon").group()
'2026-10-13'
```

Verified. Read it aloud: "four digits, dash, two digits, dash, two
digits." You're already reading regex. (`{4}` is a quantifier —
next chapter.)

**Next:** [Quantifiers](quantifiers.md)
