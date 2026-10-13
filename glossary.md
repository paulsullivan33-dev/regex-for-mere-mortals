# Glossary

| Term | Meaning |
|------|---------|
| alternation (`\|`) | "Or" — match this or that |
| anchor | Pins a pattern to a position (`^` start, `$` end, `\b` word edge) |
| backreference (`\1`) | Match the same text group 1 matched |
| capturing group `(...)` | Groups and remembers the matched text |
| catastrophic backtracking | Exponential match time from nested quantifiers — the freeze |
| character class `[...]` | Match one character from a set |
| DOTALL (`re.S`) | `.` matches newlines too |
| engine | The software that executes a pattern against text |
| escaping | Backslashing special characters to match them literally |
| extraction | Finding a pattern *within* text (`search`/`findall`) |
| flag | A modifier changing how the engine interprets the pattern |
| flavor/dialect | A specific regex implementation (PCRE, Python `re`, ERE...) |
| greedy | Quantifier grabs as much as possible (default) |
| group | Parenthesized part of a pattern |
| IGNORECASE (`re.I`) | Case-insensitive matching |
| lazy (`*?`, `+?`) | Quantifier grabs as little as possible |
| literal | A character matching itself |
| lookaround | Zero-width assertions (`(?=...)`, `(?<=...)`) — peek without consuming |
| MULTILINE (`re.M`) | `^`/`$` match at each line's start/end |
| non-capturing group `(?:...)` | Groups without remembering |
| quantifier | How many: `*` `+` `?` `{n,m}` |
| raw string (`r"..."`) | Python string where backslashes stay literal — always use for regex |
| shorthand (`\d` `\w` `\s`) | Prebuilt character classes (capitals = opposite) |
| validation | Requiring the *whole* input to match (`fullmatch` / `^...$`) |
| word boundary (`\b`) | The edge between word and non-word characters |
