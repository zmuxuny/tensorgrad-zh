# Translation Progress

## Current

- Final cleanup after full-book Chinese build verification.

## Done

- Established translation governance, bounded ledger, and XeLaTeX/`ctexbook` Chinese build foundation.
- Fixed current build blockers in enabled chapters: missing `\boxtimes` support and invalid TikZ `\foreach` variable names in `statistics.tex`.
- Translated enabled `paper/chapters/intro.tex`, `paper/chapters/statistics.tex`, and `paper/chapters/appendix.tex`.
- Translated reader-facing `README.md` introduction and examples.
- Translated disabled `paper/chapters/simple_derivatives.tex`, `paper/chapters/kronecker.tex`, and `paper/chapters/functions.tex`.
- Translated disabled `paper/chapters/determinant.tex` and `paper/chapters/advanced_derivatives.tex`.
- Translated disabled `paper/chapters/special_matrices.tex`.
- Verified `special_matrices.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.
- Translated disabled `paper/chapters/decompositions.tex`.
- Verified `decompositions.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.
- Translated disabled `paper/chapters/ml.tex`.
- Verified `ml.tex` by temporarily enabling it in `cookbook.tex`.
- Translated disabled `paper/chapters/tensor_algos.tex`.
- Verified `tensor_algos.tex` by temporarily enabling it in `cookbook.tex`; this resolves `sec:opt_contr` while enabled.
- Translated disabled `paper/chapters/tensorgrad.tex`.
- Verified `tensorgrad.tex` by temporarily enabling it in `cookbook.tex`; final main build restored enabled chapter set and succeeds.
- Enabled all translated chapters in `paper/cookbook.tex` for the full Chinese book.
- Verified full-book XeLaTeX build succeeds with all translated chapters enabled.
- Fixed remaining visible English section labels in `paper/chapters/simple_derivatives.tex` and a stray connector in `paper/chapters/intro.tex`.

## Next

- Review final git diff and commit the full-book integration stage.
- Confirm worktree cleanliness after commit.
- Push `main` to `origin/main` once everything is green.

## Open

- `paper/cookbook.pdf` is tracked upstream and has been regenerated for the full Chinese build.
