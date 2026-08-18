---
name: firecrawl-parse
description: |
  Convert a local file (PDF, DOCX, XLSX, HTML, …) to markdown, or answer questions about its content. Use whenever the input is a file path, not a URL.
allowed-tools:
  - Bash(firecrawl *)
  - Bash(npx firecrawl-cli *)
---

# firecrawl parse

Turn a local document into clean markdown on disk. Supports **PDF, DOCX, DOC, ODT, RTF, XLSX, XLS, HTML/HTM**.

## When to use

- You have a file on disk (not a URL) and want its text as markdown
- User drops a PDF/DOCX and asks what it says, or to summarize it
- Use `scrape` instead when the source is a URL

## Quick start

Always save to `.firecrawl/` with `-o` — parsed docs can be hundreds of KB and blow up context if streamed to stdout. Add `.firecrawl/` to `.gitignore`.

```bash
mkdir -p .firecrawl

# File → markdown
firecrawl parse ./paper.pdf -o .firecrawl/paper.md

# AI summary
firecrawl parse ./paper.pdf -S -o .firecrawl/paper-summary.md

# Ask a question about the doc
firecrawl parse ./paper.pdf -Q "What are the main conclusions?" \
  -o .firecrawl/paper-qa.md
```

Then `head`, `grep`, `rg` etc., or incrementally read the file - don't load the whole thing at once.

## Options

| Option                   | Description                                                                                                                      |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `-S, --summary`          | AI-generated summary                                                                                                             |
| `-Q, --query <prompt>`   | Ask a question about the parsed content                                                                                          |
| `-o, --output <path>`    | Output file path — **always use this**                                                                                           |
| `-f, --format <formats>` | Comma-separated: `markdown`, `html`, `rawHtml`, `links`, `images`, `summary`, `json`, `attributes`. Multiple formats output JSON |
| `--timeout <ms>`         | Timeout for the parse job                                                                                                        |
| `--timing`               | Show request duration                                                                                                            |

## Tips

- Quote paths with spaces: `firecrawl parse "./My Doc.pdf" -o .firecrawl/mydoc.md`.
- Max upload size: **50 MB** per file.
- Credits: ~1 per PDF page; HTML is 1 flat.
- Check `.firecrawl/` before re-parsing the same file.
- To check your credit balance (recommended for batch processing and similar workflows), use `firecrawl credit-usage` (requires authentication).

## See also

- [firecrawl-scrape](../firecrawl-scrape/SKILL.md) — same idea for URLs
