# package
Steps for how to submit a package

# R Package Submission Steps
# For CRAN and Bioconductor

---

## STEP 1 — Create Package Structure

In RStudio console:

```r
usethis::create_package("yourPackageName")
```

This creates automatically:
- `R/` — folder for your functions
- `DESCRIPTION` — package metadata
- `NAMESPACE` — exports (do not edit manually)

---

## STEP 2 — Fill in DESCRIPTION

Open `DESCRIPTION` and fill in:

```
Package: yourPackageName
Title: One Line Description
Version: 0.99.0        # for Bioconductor start at 0.99.0
                       # for CRAN start at 0.1.0
Authors@R:
    person("Your", "Name",
           email = "your@email.com",
           role = c("aut", "cre"),
           comment = c(ORCID = "your-orcid"))
Description:
    What your package does. Software names in 'single quotes'.
License: MIT + file LICENSE
Depends: R (>= 4.3)
Imports:               # packages your functions use (required)
Suggests:              # packages used in vignette/tests only (optional)
URL: https://github.com/yourusername/yourPackageName
BugReports: https://github.com/yourusername/yourPackageName/issues
```

For Bioconductor also add:
```
biocViews: SingleCell, DataImport   # pick from Bioconductor biocViews list
```

---

## STEP 3 — Write Functions

- Put each function as a `.R` file in the `R/` folder
- Add roxygen comments above each function:

```r
#' Title of your function
#'
#' @param x Description of parameter
#' @return Description of what is returned
#' @examples
#' yourFunction(x = 1)
#'
#' @export
yourFunction <- function(x) {
  x + 1
}
```

---

## STEP 4 — Generate Documentation

```r
devtools::document()
```

This creates:
- `man/` folder with `.Rd` help files
- Updates `NAMESPACE` with your exports

Run this every time you change roxygen comments.

---

## STEP 5 — Write a Vignette

```r
usethis::use_vignette("yourPackageName")
```

- Edit the `.Rmd` file in `vignettes/`
- Show real working examples
- For Bioconductor: Seurat-specific chunks should be `eval=FALSE`
- For Bioconductor: at least 50% of chunks must be runnable (`eval=TRUE`)

---

## STEP 6 — Write Tests

```r
usethis::use_testthat()
usethis::use_test("your-function-name")
```

- Edit test files in `tests/testthat/`
- Test your core functions directly (not the Shiny UI)

---

## STEP 7 — Check the Package

```r
# For CRAN:
devtools::check(cran = TRUE)

# For Bioconductor:
devtools::check()
BiocCheck::BiocCheck(".", `quit-with-status` = FALSE)
```

**Target:**
- CRAN: 0 errors | 0 warnings | 0 notes
- Bioconductor: 0 errors | 0 warnings | notes are acceptable

Fix all errors and warnings before submitting.

---

## STEP 8 — Push to GitHub

```bash
# First time only:
git init
git add .
git commit -m "initial package"
git remote add origin https://github.com/yourusername/yourPackageName.git
git branch -M main
git push -u origin main

# Every update after:
git add .
git commit -m "describe what you changed"
git push origin main
```

---

## STEP 9 — Submit

### CRAN

```r
devtools::build()   # creates yourPackageName_0.1.0.tar.gz
```

Then go to: https://cran.r-project.org/submit.html
Upload the `.tar.gz` file.

### Bioconductor

1. Open a new issue at: https://github.com/Bioconductor/Contributions/issues
2. Follow their template (package name, GitHub link, description)
3. They will add your package to their git server
4. Add their remote:
```bash
git remote add upstream git@git.bioconductor.org:packages/yourPackageName
```
5. Push to both remotes for every fix:
```bash
git push origin devel       # GitHub
git push upstream devel     # Bioconductor (triggers new build)
```

---

## STEP 10 — Respond to Reviewer Comments

- Bioconductor: reviewers post comments on your GitHub issue
- CRAN: reviewers email you directly
- Fix the issues, bump version number, check again, resubmit/push

---

## Important Notes

| Topic | Rule |
|---|---|
| Version for CRAN | Start at `0.1.0` |
| Version for Bioconductor | Start at `0.99.0`, bump for every push |
| Software names in Description | Must be in `'single quotes'` for CRAN |
| Compiled files | Never commit `.o` or `.dll` — add to `.gitignore` |
| BiocCheck folder | Delete `yourPackageName.BiocCheck` before pushing |
| Bioconductor branch | Always work on `devel` branch, not `main` |
| GitHub Actions | Set up `.github/workflows/check-bioc.yml` for Linux testing |
| Seurat on Linux | Cannot install easily — use `_R_CHECK_FORCE_SUGGESTS_=false` |
