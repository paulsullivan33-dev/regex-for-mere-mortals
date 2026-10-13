# Flags

**Flags** change how the engine interprets your pattern. Three
cover nearly everything.

## re.IGNORECASE: case doesn't matter

```python
>>> re.findall(r"cat", "Cat CAT cat")
['cat']
>>> re.findall(r"cat", "Cat CAT cat", re.IGNORECASE)
['Cat', 'CAT', 'cat']
```

Verified. (`re.I` is the short form.) For user input and log
scanning, this is almost always what you want.

## re.MULTILINE: ^ and $ per line

Without it, `^` matches only at the start of the *string*. With
`re.MULTILINE`, it matches at the start of every *line*:

```python
text = "error: disk full\nwarning: low mem\nerror: timeout"
>>> re.findall(r"^error.*", text)
['error: disk full']
>>> re.findall(r"^error.*", text, re.MULTILINE)
['error: disk full', 'error: timeout']
```

Verified. (`re.M`.) Without MULTILINE, `^` matches only at the start
of the *string* — you get just the first line. With it, `^` matches
at the start of every *line*. The classic use: "every line starting
with ERROR" in a multi-line log.

## re.DOTALL: dot matches newlines too

`.` matches everything *except* `\n` by default. `re.DOTALL` removes
the exception:

```python
>>> re.search(r"start.*end", "start\nmiddle\nend") is None
True
>>> re.search(r"start.*end", "start\nmiddle\nend", re.DOTALL).group()
'start\nmiddle\nend'
```

Verified. (`re.S`.) Needed when your match spans lines — multi-line
quoted strings, HTML blocks (though see the parser warning in
chapter 1).

## Combining and inline flags

```python
>>> re.findall(r"^error.*", text, re.IGNORECASE | re.MULTILINE)
['error: disk full', 'error: timeout']
```

Verified — flags combine with `|`. You can also write them inline
at the pattern's start: `(?i)` = IGNORECASE, `(?m)` = MULTILINE,
`(?s)` = DOTALL:

```python
>>> re.findall(r"(?im)^error.*", text)
['error: disk full', 'error: timeout']
```

Verified. Inline flags are handy in tools (grep, editors) where
there's no flags argument — `grep -i` and `grep -E` cover the common
cases there.

## The flag cheat sheet

| Flag | Short | Effect |
|------|-------|--------|
| `re.IGNORECASE` | `re.I` / `(?i)` | Case-insensitive |
| `re.MULTILINE` | `re.M` / `(?m)` | `^`/`$` match per line |
| `re.DOTALL` | `re.S` / `(?s)` | `.` matches newlines |

**Next:** [Real-World Recipes](recipes.md)
