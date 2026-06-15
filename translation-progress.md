# Translation Progress

## Current

- Translate remaining disabled book chapters, currently `paper/chapters/decompositions.tex`

## Done

- Established translation governance, bounded ledger, and XeLaTeX/`ctexbook` Chinese build foundation.
- Fixed current build blockers in enabled chapters: missing `\boxtimes` support and invalid TikZ `\foreach` variable names in `statistics.tex`.
- Translated enabled `paper/chapters/intro.tex`, `paper/chapters/statistics.tex`, and `paper/chapters/appendix.tex`.
- Translated reader-facing `README.md` introduction and examples.
- Translated disabled `paper/chapters/simple_derivatives.tex`, `paper/chapters/kronecker.tex`, and `paper/chapters/functions.tex`.
- Translated disabled `paper/chapters/determinant.tex` and `paper/chapters/advanced_derivatives.tex`.
- Translated disabled `paper/chapters/special_matrices.tex`.
- Verified `special_matrices.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.

## Next

- Translate remaining `paper/chapters/*.tex` in chapter order, starting with `decompositions.tex`.
- Rebuild PDF after each large chapter or build-affecting change.

## Open

- `paper/cookbook.pdf` is tracked upstream; decide at final release whether to commit regenerated Chinese PDF.
