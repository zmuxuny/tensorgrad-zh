# Translation Progress

## Current

- Translate enabled book files, currently `paper/chapters/statistics.tex`

## Done

- Added project-wide translation rules in `AGENTS.md`.
- Added bounded progress ledger format in `translation-progress.md`.
- Committed translation governance files.
- Enabled XeLaTeX Chinese build in `paper/cookbook.tex` with `ctexbook`.
- Verified `cd paper && latexmk -xelatex cookbook.tex` succeeds; remaining warnings are undefined refs to disabled chapters and existing font/TikZ warnings.
- Fixed current build blockers: missing `\boxtimes` support and invalid TikZ `\foreach` variable names in `statistics.tex`.
- Translated enabled `paper/chapters/intro.tex`.
- Translated enabled `paper/chapters/appendix.tex`.

## Next

- Translate enabled `paper/chapters/statistics.tex`.
- Translate remaining `paper/chapters/*.tex` in chapter order.
- Update README reader-facing introduction after enabled chapters.

## Open

- `paper/cookbook.pdf` is tracked upstream; decide at final release whether to commit regenerated Chinese PDF.
