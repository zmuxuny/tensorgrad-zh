# Translation Progress

## Current

- Translate remaining disabled book chapters, starting with `paper/chapters/simple_derivatives.tex`

## Done

- Added project-wide translation rules in `AGENTS.md`.
- Added bounded progress ledger format in `translation-progress.md`.
- Committed translation governance files.
- Enabled XeLaTeX Chinese build in `paper/cookbook.tex` with `ctexbook`.
- Verified `cd paper && latexmk -xelatex cookbook.tex` succeeds; remaining warnings are undefined refs to disabled chapters and existing font/TikZ warnings.
- Fixed current build blockers: missing `\boxtimes` support and invalid TikZ `\foreach` variable names in `statistics.tex`.
- Translated enabled `paper/chapters/intro.tex`.
- Translated enabled `paper/chapters/appendix.tex`.
- Translated enabled `paper/chapters/statistics.tex`.

## Next

- Translate remaining `paper/chapters/*.tex` in chapter order, starting with `simple_derivatives.tex`.
- Update README reader-facing introduction after enabled chapters.
- Rebuild PDF after each large chapter or build-affecting change.

## Open

- `paper/cookbook.pdf` is tracked upstream; decide at final release whether to commit regenerated Chinese PDF.
