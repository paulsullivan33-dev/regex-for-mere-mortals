# Regex Flavors

"Regular expression" is one idea with many dialects. The core
(literals, `.`, `*`, `+`, `?`, classes, anchors, groups) works
everywhere. The edges differ — and the edges are where people get
bitten moving between tools.

## The dialects you'll meet

| Flavor | Where | Notes |
|--------|-------|-------|
| PCRE | PHP, `grep -P`, many tools | The de facto standard; most featureful |
| Python `re` | Python | Close to PCRE; `(?P<name>...)` for named groups |
| JavaScript | Browsers, Node | No lookbehind until ES2018; `/pattern/flags` syntax |
| `grep -E` (ERE) | Shell | No non-capturing groups in strict POSIX; no lookaround |
| `grep` basic (BRE) | Shell (default) | `\( \)` for groups, `\+` for one-or-more — avoid; use `-E` |

## Differences that bite

**Named groups differ:**
```python
(?P<year>\d{4})    # Python
(?<year>\d{4})     # PCRE, JavaScript, .NET
```

**Lookaround support varies.** `(?=...)` (lookahead) is widespread;
`(?<=...)` (lookbehind) is missing in older JavaScript and strict
ERE. If a pattern uses lookbehind, check your flavor before
committing to it.

**`\d` and Unicode.** In Python 3, `\d` matches Unicode digits
(`٣`); in most other flavors it's ASCII-only. For strict ASCII,
write `[0-9]` — explicit everywhere.

## The portable core

If you stick to this subset, your patterns run identically in
Python, JavaScript, Go, Rust, Java, `grep -E`, and VS Code:

```text
literals  .  *  +  ?  {n,m}  [abc]  [^abc]  \d \w \s
^  $  \b  ( )  (?: )  |  \1  escaping with \
```

That's everything this guide teaches except named groups and
lookaround — which is to say, everything you need 95% of the time.
When you reach for the exotic 5%, check your flavor's docs first.

## A note on performance

Most engines use **backtracking**: on failure, they re-try the
pattern different ways. Pathological patterns (nested quantifiers
like `(a+)+$`) can take *exponential* time on certain inputs —
[catastrophic backtracking](troubleshooting.md). Some engines (Go's
`regexp`, Rust's `regex`) guarantee linear time by design; PCRE /
Python / JavaScript do not. If a pattern will face untrusted input,
keep it simple or use a linear-time engine.

**Next:** [Troubleshooting](troubleshooting.md)
