# Troubleshooting

## "It matches too much"

Almost always **greedy quantifiers**. `".*"` on `"hi" and "bye"`
matches the whole `"hi" and "bye"`. Fix: lazy — `".*?"`. (See
[Quantifiers](quantifiers.md).) If it still over-matches, your
"anything" is too permissive: `[^"]*` ("not a quote") is more
precise than `.*?` for quoted strings.

## "It matches inside words"

You forgot `\b`. `cat` matches `concatenate`; `\bcat\b` doesn't.
(See [Anchors](anchors.md).)

## "It doesn't match and I can't see why"

1. **Print the pattern.** Actually look at it — is that `\.` or
   `.`? Is the raw string prefix there?
2. **Bisect it.** Test halves: does `\d{4}` match? Does
   `\d{4}-\d{2}`? The half that fails contains the bug.
3. **Paste it into regex101.com.** It explains each token and shows
   the match step-by-step. This solves most mysteries in seconds.
4. **Check the flags.** Multiline text needs `re.MULTILINE` for
   `^`; case differences need `re.IGNORECASE`.

## "No match" from findall with groups

`findall` returns *groups*, not whole matches, when the pattern has
capturing groups. You got `[('2026','10','13')]` and wanted
`['2026-10-13']`. Fix: `(?:...)` for groups you don't need to keep.
(See [Groups](groups.md).)

## Catastrophic backtracking: the freeze

Your program hangs on a 50-character string. The pattern has nested
quantifiers — `(a+)+`, `(\w+\s*)+` — and the engine tries
exponential combinations. Fixes, in order:

1. **Be specific.** `[^"]*` instead of `.*?`; `\d+` instead of
   `(\d+)+`.
2. **Use possessive/atomic groups** if your flavor has them
   (`(?>...)` in PCRE/Java) — they forbid backtracking into the
   group.
3. **Don't regex it.** If the pattern needs nesting, that's the
   parser signal from chapter 1.

## "Works in the tester, fails in my code"

- **Double escaping.** In JSON/Java strings, `"\\d"` is needed to
  get `\d` to the engine. The tester took your pattern raw; your
  code's string layer ate a backslash. Print the pattern your code
  actually compiles.
- **Different flavor.** The tester defaulted to PCRE; you're running
  Python/JavaScript. Check the [flavors](flavors.md) table —
  lookbehind and named-group syntax are the usual culprits.
- **find vs. matches.** Java's `matches()` requires full-string
  match (like `fullmatch`); `find()` scans. Python's `search` vs
  `fullmatch` — same trap, different names.

## The debugging mantra

Smallest failing input. Fewest pattern pieces. One change at a
time. Regex rewards the same method as everything else — it just
punishes guessing harder.

**Next:** [Glossary](glossary.md)
