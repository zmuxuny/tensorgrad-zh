# Translation Progress

## Current

- Translate remaining disabled book chapters, currently `paper/chapters/special_matrices.tex`

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
- Translated reader-facing `README.md` introduction and examples.
- Translated disabled `paper/chapters/simple_derivatives.tex`.
- Translated disabled `paper/chapters/kronecker.tex`.
- Translated disabled `paper/chapters/functions.tex`.
- Verified `functions.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.
- Translated disabled `paper/chapters/determinant.tex`.
- Verified `determinant.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.
- Translated disabled `paper/chapters/advanced_derivatives.tex`.
- Verified `advanced_derivatives.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.

## Next

- Translate remaining `paper/chapters/*.tex` in chapter order, starting with `special_matrices.tex`.
- Rebuild PDF after each large chapter or build-affecting change.

## Open

- `paper/cookbook.pdf` is tracked upstream; decide at final release whether to commit regenerated Chinese PDF.
