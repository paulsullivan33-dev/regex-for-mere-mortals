# Quantifiers

A **quantifier** says *how many* of the previous thing: "three
digits," "one or more letters," "optional."

## The big four

| Quantifier | Means | Example | Matches |
|------------|-------|---------|---------|
| `*` | Zero or more | `a*` | ``, `a`, `aaa` |
| `+` | One or more | `a+` | `a`, `aaa` (not empty) |
| `?` | Zero or one (optional) | `colou?r` | `color`, `colour` |
| `{n,m}` | Between n and m | `\d{2,4}` | `12`, `123`, `1234` |

```python
>>> re.findall(r"\d+", "abc123def45")
['123', '45']                    # one or more digits, greedily
>>> re.findall(r"colou?r", "color colour")
['color', 'colour']              # the u is optional
>>> re.findall(r"\d{2,4}", "1 12 123 12345")
['12', '123', '1234']            # 2-4 digits; "12345" gives "1234"
```

All verified. `{n}` is exactly n; `{n,}` is n or more. The precise
forms (`{4}`, `{2,4}`) are your friends for fixed-format data like
dates and zip codes.

## Greedy vs. lazy: the essential gotcha

By default, quantifiers are **greedy** — they grab as much as
possible:

```python
>>> re.search(r"<.*>", "<b>hi</b>").group()
'<b>hi</b>'     # grabbed EVERYTHING between first < and last >
```

You wanted `<b>`, you got the whole tag pair. The fix: **lazy**
quantifiers (`*?`, `+?`) grab as *little* as possible:

```python
>>> re.search(r"<.*?>", "<b>hi</b>").group()
'<b>'           # stops at the first >
```

Both verified. **The rule:** when matching *between delimiters*
(tags, quotes, brackets), reach for the lazy version first. Greedy
is right when you want "everything up to the end" — lazy is right
when you want "the smallest piece."

```python
>>> re.findall(r'".*?"', 'say "hi" and "bye"')
['"hi"', '"bye"']     # lazy: two separate quoted strings
>>> re.findall(r'".*"', 'say "hi" and "bye"')
['"hi" and "bye"']    # greedy: one giant match
```

Verified. This pair of lines is worth staring at until it clicks —
it's the single most common regex bug in the wild.

## Common shapes

| Shape | Use |
|-------|-----|
| `\d+` | A number |
| `\w+` | A word / identifier |
| `".*?"` | A quoted string |
| `\d{4}-\d{2}-\d{2}` | An ISO date |
| `https?://` | `http://` or `https://` |
| `\s*` | Optional whitespace (the polite glue between tokens) |

**Next:** [Anchors](anchors.md)
