# What Regex Is

**The one-sentence version:** A regular expression is a pattern that
describes a set of strings — and a regex engine finds text matching
that pattern.

## The idea

You already match patterns informally: "find every line with ERROR
in it," "grab the date from these filenames." A regex writes that
thought down precisely enough for a computer:

```text
ERROR
```

matches any line containing `ERROR`. And:

```text
\d{4}-\d{2}-\d{2}
```

matches `2026-10-13` — four digits, dash, two digits, dash, two
digits. (Don't worry about the syntax yet; by chapter 4 you'll read
that fluently.)

## Where regex lives

Nearly everywhere text is processed:

- **Every major language** — Python (`re`), JavaScript (built in),
  Go (`regexp`), Rust (`regex` crate), Java, C#...
- **The command line** — `grep -E`, `sed`, `awk`
- **Editors** — VS Code, vim: find-and-replace with patterns
- **Databases** — `WHERE name REGEXP ...` in MySQL/Postgres

Learn it once, use it everywhere. That's the pitch, and it's true.

## The famous warning

There's an old joke: *"Some people, when confronted with a problem,
think 'I know, I'll use regular expressions.' Now they have two
problems."*

It's funny because it's half-true. Regex is a power tool: perfect
for *finding* and *extracting* patterns, terrible for *parsing*
nested structures (HTML, JSON, code). The wisdom isn't avoiding
regex — it's knowing which job it's for:

| Reach for regex | Reach for a real parser |
|---|---|
| Finding dates in text | Parsing HTML |
| Validating a zip code's shape | Validating an email address fully |
| Splitting logs into fields | Parsing JSON |
| Renaming files in bulk | Anything with nesting |

**The rule of thumb:** regex describes *flat* patterns beautifully
and *nested* structures terribly. When your pattern needs to count
("match the closing tag for *this* opening tag"), stop — that's a
parser's job.

**Next:** [First Patterns](first-patterns.md)
