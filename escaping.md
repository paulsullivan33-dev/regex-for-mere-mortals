# Escaping

A dozen characters are *special* in regex. To match them literally,
**escape** them with a backslash.

## The special characters

```text
. ^ $ * + ? { } [ ] \ | ( )
```

To match a literal dot (not "any character"), write `\.`:

```python
>>> re.search(r"3.14", "3x14").group()    # . is "any char" — matches!
'3x14'
>>> re.search(r"3\.14", "3x14") is None   # \. is a literal dot — no match
True
>>> re.search(r"3\.14", "pi is 3.14").group()
'3.14'
```

All verified. **The #1 regex bug:** writing `www.example.com`
when you mean `www\.example\.com`. The unescaped version matches
`wwwXexampleYcom` too — usually harmless, occasionally a real bug
in validation.

## re.escape: let Python do it

Matching user input literally? Don't hand-escape — automate it:

```python
>>> user_input = "price: $5.00 (sale!)"
>>> pattern = re.escape(user_input)
>>> pattern
'price:\\ \\$5\\.00\\ \\(sale!\\)'
>>> bool(re.search(pattern, "the price: $5.00 (sale!) today"))
True
```

Verified. `re.escape` backslashes everything special. Whenever the
pattern comes from *data* rather than your head, escape it.

## Raw strings: why r"..." always

In a normal Python string, `"\d"` is... actually fine (`\d` isn't a
recognized escape, so Python leaves it). But `"\b"` is a *backspace
character*, not a word boundary! Compare:

```python
>>> "\b" == chr(8)      # backspace — NOT what you want
True
>>> r"\b" == "\\b"      # two characters: backslash, b — correct
True
```

Verified. **Always write regex as raw strings** (`r"\b\d+"`).
It's not about `\d` — it's about the day you need `\b` and get a
backspace instead.

## In the shell: quoting

```bash
grep -E "3\.14" file.txt     # double quotes: backslash survives
grep -E '3\.14' file.txt     # single quotes: safest — shell touches nothing
```

Single quotes are the shell's raw strings. With double quotes, the
shell may eat your backslashes before grep sees them (`"\d"` →
`d`). **Single-quote your patterns in the shell**, same instinct as
raw strings in Python.

## Cheat sheet

| To match | Write | Example |
|----------|-------|---------|
| A literal dot | `\.` | `3\.14` |
| A literal backslash | `\\` | `C:\\path` |
| Literal parens | `\( \)` | `\(555\)` |
| A literal `+`/`*`/`?` | `\+` `\*` `\?` | `1\+1` |
| Any user-supplied text | `re.escape(text)` | — |

**Next:** [Flags](flags.md)
