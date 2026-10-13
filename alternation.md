# Alternation

**Alternation** — `|` — means "or": match *this* or *that*.

```python
>>> re.findall(r"cat|dog", "cat and dog")
['cat', 'dog']
>>> re.findall(r"gr(a|e)y", "gray grey")
['a', 'e']     # findall returns the GROUP — see the quirk in Groups
```

Both verified. (That second result shows the findall-with-groups
quirk again — use `(?:a|e)` if you want whole matches.)

## Scope: | splits the whole pattern

```python
>>> re.findall(r"^cat|dog$", "my dog")
['dog']        # means (^cat) OR (dog$) — NOT ^(cat|dog)$
```

Verified — and surprising. `|` has the *lowest* precedence: it
splits everything around it. To alternate inside anchors, group it:

```python
>>> re.findall(r"^(?:cat|dog)$", "dog")
['dog']
>>> re.search(r"^(?:cat|dog)$", "my dog") is None
True
```

Both verified. **When in doubt, wrap alternations in `(?:...)`.**
It's never wrong and often necessary.

## Realistic uses

```python
>>> re.findall(r"\.(?:jpg|jpeg|png|gif)\b", "photo.jpg doc.pdf pic.png")
['.jpg', '.png']                    # image extensions (note \b, not $)
>>> re.search(r"https?://", "https://x").group()
'https://'                          # the s is optional — alternation's cousin
>>> re.findall(r"\b(?:Mon|Tue|Wed|Thu|Fri)\b", "see you Fri")
['Fri']
```

All verified. Alternation shines for fixed vocabularies: file
extensions, day names, log levels (`ERROR|WARN|INFO`).

## Alternation order matters (a little)

The engine tries alternatives left to right and takes the *first*
that lets the overall match succeed:

```python
>>> re.search(r"cat|cats", "cats").group()
'cat'     # "cat" matched first; the engine stopped looking
```

Verified. Usually harmless (both alternatives matched *something*),
but for extraction, put the longer/more specific alternative first:
`cats|cat`.

**Next:** [Escaping](escaping.md)
