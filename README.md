# package
Steps for how to submit a package

● Same steps plus extra package structure before you push:

  1. Create package structure in RStudio
  usethis::create_package("yourPackageName")
  This creates: R/, DESCRIPTION, NAMESPACE automatically.

  2. Write your functions
  - Put each function as a .R file in R/ folder
  - Add roxygen comments above each function (#') for documentation

  3. Generate documentation
  devtools::document()  # creates man/ folder and updates NAMESPACE

  4. Check the package
  # For CRAN:
  devtools::check(cran = TRUE)

  # For Bioconductor:
  devtools::check()
  BiocCheck::BiocCheck(".", `quit-with-status` = FALSE)

  5. Fix all errors and warnings

  6. Push to GitHub (same git steps as before)

  7. Submit
  - CRAN: devtools::build() → upload .tar.gz at cran.r-project.org/submit.html
  - Bioconductor: open issue at github.com/Bioconductor/Contributions → push to their git server
