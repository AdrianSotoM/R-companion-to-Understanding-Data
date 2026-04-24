# *Understanding Data* — R companion (student setup)

This folder contains twelve `.Rmd` files, one per chapter of *Understanding
Data*. Each file walks through the chapter's key algorithms in R — mostly
using simulation and resampling, following the book's pedagogical style —
and ends with three exercises.

## 1. Prerequisites

- **R ≥ 4.1** (needed for the native pipe `|>` used throughout).
  Check with `R.version.string` in the R console.
- **RStudio** (recommended) — makes Knitting a single click.

## 2. One-time package install

Run this once in the R console before opening any of the `.Rmd` files:

```r
install.packages(c(
  "tidyverse",    # dplyr, ggplot2, tibble, purrr, tidyr
  "ggbeeswarm",   # beeswarm plots (Chs. 2, 6, 7)
  "patchwork",    # combining ggplots side by side (Chs. 2, 5)
  "quantreg",     # L1 (median) regression (Ch. 10)
  "pwr"           # power calculations (Ch. 11)
))
```

`splines` is used in Ch. 10 but ships with base R — no install needed.

## 3. How to Knit

**In RStudio:**

1. Open the `.Rmd` file.
2. Click the **Knit** button (or press `Ctrl/Cmd + Shift + K`).
3. RStudio will render the file as HTML, run every code chunk, and open
   the result in the Viewer pane.

**From the R console (no RStudio):**

```r
rmarkdown::render("Ch01_The_Problem_with_Statistics.Rmd")
```

This produces a `.html` file in the same folder.

To get a PDF instead, change the YAML header at the top of the `.Rmd` from
`output: html_document` to `output: pdf_document`, and ensure you have a
TeX installation (e.g. via `tinytex::install_tinytex()`).

## 4. Recommended order

The chapters build on each other. Suggested path:

1. **Ch. 01** — orientation, why α matters
2. **Ch. 02** — visualizing data
3. **Ch. 03** — simulation-based p-values (core machinery)
4. **Ch. 04** — confidence intervals by bootstrap
5. **Ch. 05** — the Normal distribution and when to trust it
6. **Ch. 06** — two-group comparisons
7. **Ch. 07** — three or more groups, multiple-testing correction
8. **Ch. 08** — count data and χ
9. **Ch. 09** — correlation
10. **Ch. 10** — regression
11. **Ch. 11** — power
12. **Ch. 12** — Bayes

## 5. Reproducibility

Every file begins with `set.seed(...)`. Output should be deterministic across
runs on the same machine. If a student's result differs from the companion's,
likely causes are: a different R version, an out-of-date package, or a
different random-number algorithm (`RNGkind()`).

## 6. Pedagogical note

Most simulations are written as **explicit `for` loops first**. This is
deliberate — the loop makes the statistical procedure readable line by line.
Idiomatic R alternatives (`replicate()`, `map_dbl()`, vectorized calls) are
shown as comments so students see both the transparent and the efficient
forms.

If a simulation takes a long time on a student laptop, reduce `n_sim` from
10,000 to 1,000 — the shapes of the null distributions are already clear
at that scale, and it runs in a few seconds.

## 7. Cross-references to the book

Every `.Rmd` section header carries an inline reference to the corresponding
book section and page number, e.g.:

```
# §6.3 — The Big Box method  *(book p. 210)*
```

Where the book contains **explicit Python code**, the companion reproduces
that snippet as a blockquote with its page number, alongside the R equivalent
in the code chunk. Explicit Python appears in only four places in the book:

| Book page | Section | Snippet                                      |
|-----------|---------|----------------------------------------------|
| p. 195    | §5.2    | `sm.qqplot(data, line='45')`                 |
| p. 272    | §6.9    | `from scipy import stats; stats.ttest_ind()` |
| p. 364    | §7.10   | `from scipy.stats import f_oneway`           |
| p. 365    | §7.10   | three sequential `stats.ttest_ind()` calls   |
| p. 379    | §8.3    | `np.random.choice()` for χ² simulation       |

Chapters 1, 2, 3, 4, 9, 10, 11, 12 contain no Python code in the book — the
companion's R translations in those chapters are translations of the book's
**algorithms and worked examples**, not of code that literally appears in
the text.

## 8. Troubleshooting

| Symptom                                    | Likely cause                                                       |
|--------------------------------------------|--------------------------------------------------------------------|
| `Error: could not find function "\|>"`     | R < 4.1. Upgrade R.                                                 |
| `there is no package called "ggbeeswarm"`  | Ran the file without the install step in §2.                       |
| Knit stalls at a large `for` loop          | Reduce `n_sim`; start with 1,000.                                  |
| PDF knit fails                             | Install TinyTeX: `install.packages("tinytex"); tinytex::install_tinytex()`. |
| Different numbers from the companion       | Check `R.version.string`; versions < 3.6 used a different RNG.     |
