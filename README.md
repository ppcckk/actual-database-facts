# Actual Database — dated price, capacity and throughput figures

A machine-readable dump of every dated figure published on [actualdatabase.com](https://actualdatabase.com/).
Each row is one claim that a human read off a vendor or agency page on the date shown, with a link to the page that carries the source list.

- **Site:** https://actualdatabase.com/
- **Dataset page:** https://actualdatabase.com/data/
- **Change log:** https://actualdatabase.com/changes/
- **Topic hubs:** https://actualdatabase.com/topics/ (GPUs: https://actualdatabase.com/topics/gpu/)
- **Machine-readable index:** https://actualdatabase.com/llms.txt

## Files

| File | Rows | What it is |
| --- | --- | --- |
| `data/facts.csv` | ~1000 | `fact,page,title,verified` — one line per dated claim |
| `data/facts.json` | ~1000 | same rows as JSON, plus `generated` timestamp |

Both are regenerated from source Markdown on every site build, so the copy here is a snapshot: the
site's `/data/facts.json` is always the newest.

## Columns

| Column | Meaning |
| --- | --- |
| `fact` | The claim, with `(checked YYYY-MM-DD)` for values read off a source page and `(calculated YYYY-MM-DD)` for values derived arithmetically from them |
| `page` | Absolute URL of the note that owns the claim and lists its primary sources |
| `title` | The note's question-shaped headline |
| `verified` | Date the figure was last confirmed against the primary source |

## How the numbers are checked

Every note ships a `tools/verify-facts.mjs`-style check: the build fetches each cited source, strips
markup, parses every number on the page, and requires each claim's number to appear (with 10^k scaling
and a 0.6% tolerance). Derived values are labelled `(calculated …)` so they are never mistaken for
published figures. Numbers are never edited to make a check pass; when a source is unreachable the
note keeps its previous date instead.

## Licence

Data: **CC BY 4.0** — reuse with attribution to *Actual Database* and the date shown.
Cite as: `Actual Database (2026). Dated price and capacity figures. https://actualdatabase.com/data/`

## Caveats worth knowing before you use this

- Prices are list prices read off public pages, not invoices. Regional pricing, taxes and negotiated rates differ.
- Some sources are rendered client-side; where a vendor does not print a number (for example NVIDIA's consumer pages omit memory bandwidth) the note says so instead of guessing.
- "Verified" means a human read the source that day, not that the vendor has not changed it since.
