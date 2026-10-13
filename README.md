# Regular Expressions for Mere Mortals

A **regular expression** (regex) is a tiny language for describing
text patterns: "an email-ish thing," "a date," "every error line in
this log." One line of regex replaces dozens of lines of manual
string-chopping code — which is why every programmer eventually
learns them, usually the hard way.

This guide is the easy way. No CS degree required. Ideas first, then
patterns you can try immediately.

## What this guide covers

- **What regex is** — and the famous warning about two problems
- **First patterns** — literal matches and the dot
- **Character classes** — `[abc]`, `\d`, `\w`, `\s`
- **Quantifiers** — `*`, `+`, `?`, `{n,m}`, greedy vs. lazy
- **Anchors** — `^`, `$`, `\b`
- **Groups** — capturing, backreferences, non-capturing groups
- **Alternation** — `this|that`
- **Escaping** — matching literal special characters
- **Flags** — case-insensitivity, multiline, dotall
- **Real-world recipes** — dates, logs, CSV-ish data, validation vs.
  extraction
- **Flavors** — why the same pattern behaves differently in Python,
  JavaScript, and grep
- **Troubleshooting** — catastrophic backtracking and debugging

## What it doesn't cover

Parser theory, writing your own regex engine, or the full PCRE
spec. This guide teaches the 20% of regex that handles 95% of real
tasks — and tells you when to stop and write real code instead.

## How to use this guide

Every example is runnable. Python examples use the `re` module;
shell examples use `grep -E`. **Check:** blocks show verified
output. **Try it:** marks patterns worth typing into a regex tester
([regex101.com](https://regex101.com) explains each token as you
type — use it while reading this guide).

## A note on verification

Every pattern in this guide was executed against Python 3.12's `re`
module (and `grep -E` where shown) before shipping — 60+ assertions,
all green. Where behavior differs between regex flavors, it's called
out explicitly instead of smoothed over.

## Chapters

1. [What Regex Is](what-regex-is.md)
2. [First Patterns](first-patterns.md)
3. [Character Classes](character-classes.md)
4. [Quantifiers](quantifiers.md)
5. [Anchors](anchors.md)
6. [Groups](groups.md)
7. [Alternation](alternation.md)
8. [Escaping](escaping.md)
9. [Flags](flags.md)
10. [Real-World Recipes](recipes.md)
11. [Regex Flavors](flavors.md)
12. [Troubleshooting](troubleshooting.md)
13. [Glossary](glossary.md)

*Written to the [nine-point writing standard](criteria.md) shared by
every guide in this series.*
