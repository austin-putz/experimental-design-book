# Experimental Design for Animal Science: A Practical Guide
## Book Planning Document

---

## Overview

This book provides a practical, species-specific guide to experimental design for advanced animal science students. Unlike traditional design texts that rely on agronomy examples, this book uses exclusively livestock scenarios (swine, beef, poultry, dairy, sheep, and meat science) to teach design principles and statistical modeling.

**Target Audience:** Graduate students and advanced undergraduates in animal science who have completed intermediate biostatistics (comfortable with ANOVA, regression, and mixed models).

**Format:** Quarto-based HTML book (~150-200 pages, 10 chapters)

**Software:** R code examples throughout using tidyverse, lme4, nlme, emmeans, and lmerTest packages

---

## Learning Objectives

By the end of this book, students will be able to:
1. Identify the appropriate experimental design for common animal science research questions
2. Distinguish between experimental units and observational units in hierarchical livestock data
3. Specify correct mixed model structures for nested, split-plot, and repeated measures designs
4. Write and interpret R code for analyzing complex livestock experiments
5. Calculate appropriate sample sizes accounting for clustering and correlation
6. Avoid common pitfalls like pseudoreplication in animal studies

---

## Book Structure (10 Chapters)

### Chapter 1: Principles of Experimental Design in Animal Science
**Purpose:** Establish foundational concepts specific to livestock research

**Content:**
- Why experimental design matters in animal agriculture (cost, ethics, biological constraints)
- Three pillars: Replication, Randomization, Control
- Sources of variation unique to livestock (genetic variation, environmental heterogeneity, management effects)
- Experimental unit vs observational unit (the pen problem, the litter problem)
- Treatment structure vs design structure
- Fixed vs random effects: decision framework
- Roadmap of the book

**Examples:**
- Swine: Why individual pig weights within a pen are not independent
- Dairy: Cow-level vs lactation-level experimental units
- Poultry: Cage effects in layer hen studies
- Meat science: Multiple steaks from the same loin

**Equation Coverage:**
- Basic linear model: Y = Xβ + Zγ + ε
- Variance partitioning
- Intraclass correlation

---

### Chapter 2: Completely Randomized and Randomized Complete Block Designs
**Purpose:** Cover fundamental single-factor designs

**Content:**
- Completely Randomized Design (CRD)
  - When animals can be considered homogeneous
  - Model specification
  - ANOVA table and interpretation
- Randomized Complete Block Design (RCBD)
  - Blocking to reduce error variance
  - Blocks as fixed vs random effects
  - Relative efficiency
- Power and sample size for both designs
- Assumptions and diagnostics

**Examples:**
- Swine CRD: Individual housing in metabolism crates (dietary treatments)
- Beef RCBD: Feedlot pens blocked by initial weight group
- Poultry CRD: Broiler pens assigned to lighting programs
- Dairy RCBD: Cows blocked by parity (lactation number)
- Sheep RCBD: Lambs blocked by birth type (singles vs twins)

**R Code:**
- `lm()` for fixed effects models
- `lmer()` for random block effects
- Contrast testing with `emmeans`
- Diagnostic plots

**Equation Coverage:**
- RCBD model: Y_ij = μ + τ_i + β_j + ε_ij
- Expected mean squares
- F-statistics and degrees of freedom

---

### Chapter 3: Factorial Arrangements
**Purpose:** Multiple factors and interaction interpretation

**Content:**
- Factorial treatment structures (2×2, 2×3, 3×3, etc.)
- Main effects and interactions
- Why interactions matter in animal science
- Simple effects analysis
- Graphing interactions effectively
- Higher-order factorials (briefly)

**Examples:**
- Swine: Diet composition × sex (barrows vs gilts) on growth performance
- Beef: Breed × forage type on feedlot gain
- Poultry: Protein level × amino acid supplementation in broilers
- Dairy: Forage source × concentrate level on milk production
- Sheep: Breed × nutrition on carcass composition
- Meat science: Aging period × packaging method on tenderness

**R Code:**
- Factorial ANOVA with `lm()` and `aov()`
- Interaction plots with `emmip()`
- Simple effects with `emmeans()` and `joint_tests()`

**Equation Coverage:**
- Factorial model: Y_ijk = μ + α_i + β_j + (αβ)_ij + ε_ijk
- Interaction interpretation
- Sums of squares partitioning

---

### Chapter 4: Nested and Hierarchical Designs
**Purpose:** Handle the most common issue in livestock research - clustering

**Content:**
- The "pen problem" in swine and beef research
- Nested vs crossed factors
- Experimental unit identification (critical!)
- Mixed models with random effects
- Variance components and ICC
- Proper inference with correct error terms
- Consequences of ignoring nesting (pseudoreplication)
- Sample size calculations for clustered data

**Examples:**
- Swine: Pigs nested within pens, pens receive dietary treatments
  - Model: Y_ijk = μ + τ_i + p_ij + ε_ijk where p_ij ~ N(0, σ²_p)
  - Why you can't use individual pig as n
- Beef: Steers nested within feedlot pens
- Poultry: Individual birds nested within cages or floor pens
- Dairy: (Less common, but cows within management groups)
- Sheep: Lambs nested within ewes (litter effects)

**R Code:**
- `lmer()` syntax: `response ~ treatment + (1|pen)`
- Extracting variance components
- Likelihood ratio tests for random effects
- Computing ICC: σ²_pen / (σ²_pen + σ²_residual)
- Power analysis for clustered designs

**Equation Coverage:**
- Mixed model in matrix notation: **y** = **Xβ** + **Zu** + **ε**
- Variance-covariance structure
- REML estimation (conceptual)
- Design effect and effective sample size

**Special Section:**
- Decision tree: "Is pen my experimental unit or is pig?"
- Table showing common mistakes and corrections

---

### Chapter 5: Split-Plot and Split-Unit Designs
**Purpose:** Master designs where experimental units differ for different factors

**Content:**
- Split-plot design structure
  - Whole-plot factors
  - Sub-plot (split-plot) factors
  - Why two error terms
- Common applications in meat science
- Repeated measures as a split-plot variant (preview of Ch 6)
- Strip-plot designs (briefly)
- Sample size considerations

**Examples:**
- Meat Science (PRIMARY EXAMPLES):
  - Whole plot: Entire carcass receives pre-treatment (e.g., electrical stimulation vs control)
  - Sub-plot: Individual loins/muscles receive different aging or packaging treatments
  - Model accounts for correlation within carcass

- Meat Science Example 2:
  - Steaks from the same loin assigned different cooking methods
  - Loin is whole plot unit, steak is sub-plot unit

- Swine meat quality:
  - Pigs receive pre-slaughter treatments (whole plot)
  - Multiple chops from each pig evaluated under different display conditions (sub-plot)

- Dairy:
  - Cows receive feeding management (whole plot)
  - Milk from each cow tested with different processing methods (sub-plot)

- Feedlot:
  - Pens receive implant strategy (whole plot)
  - Individual steers within pen receive supplement (sub-plot) - less common but possible

**R Code:**
- `lmer()` with nested random effects
- Model: `response ~ whole_plot_trt * sub_plot_trt + (1|carcass) + (1|carcass:muscle)`
- Multiple error terms
- Correct denominator degrees of freedom

**Equation Coverage:**
- Split-plot model: Y_ijk = μ + α_i + W_ij + β_k + (αβ)_ik + ε_ijk
  - Where W_ij is whole-plot error, ε_ijk is sub-plot error
- Variance structure showing two error levels
- Expected mean squares for proper F-tests

**Special Section:**
- "Recognizing split-plot designs in the wild"
- Table contrasting nested vs split-plot structures

---

### Chapter 6: Repeated Measures Designs
**Purpose:** Longitudinal data common in growth and production studies

**Content:**
- Repeated measures structure
- Why time creates correlation
- Covariance structures (compound symmetry, AR(1), unstructured)
- Growth curves
- Time as categorical vs continuous
- Sphericity assumption (and why we use mixed models instead of old-school RM-ANOVA)
- Subject-specific curves

**Examples:**
- Swine: Weekly body weights from weaning to market
  - Model individual growth curves
  - Compare treatment effects on growth trajectory
- Beef: Monthly weights in backgrounding or feedlot
- Dairy: Daily or weekly milk production across lactation
  - Lactation curves (Wood's model)
- Poultry: Weekly egg production in laying hens
- Sheep: Lamb growth rates over time
- Meat science: Steak color measured over days of retail display

**R Code:**
- `lmer()` with time effects: `response ~ treatment * time + (time|animal)`
- Random intercepts and random slopes
- Selecting covariance structures with AIC/BIC
- `nlme::lme()` for more covariance structures
- Plotting individual trajectories

**Equation Coverage:**
- Level 1 (within-subject): Y_ij = a_i + b_i*time_j + ε_ij
- Level 2 (between-subject): a_i = α_0 + α_1*treatment_i + u_i
- Compound symmetry vs AR(1) covariance matrices
- Variance-covariance structure for repeated measures

---

### Chapter 7: Crossover and Latin Square Designs
**Purpose:** Designs where animals receive multiple treatments over time

**Content:**
- Crossover designs (switchback designs)
  - Each animal is its own control
  - Period effects
  - Carryover effects
  - Washout periods
- Latin Square designs
  - Controlling for two blocking factors
  - Application in dairy nutrition (very common!)
- Incomplete Latin Squares
- Analysis considerations

**Examples:**
- Dairy (PRIMARY):
  - 4×4 Latin Square: 4 cows, 4 diets, 4 periods
  - Controls for cow effects and period effects
  - Common in dairy nutrition trials

- Dairy 2:
  - Switchback design for comparing TMR formulations
  - 2 periods or multiple periods

- Swine:
  - Growing pig nutrition: animals crossed over diets in metabolism study
  - Digestibility trials

- Small ruminants:
  - Sheep or goat metabolism studies with crossover

**R Code:**
- `lmer()` with crossed random effects
- Model: `response ~ treatment + period + (1|cow) + (1|period:cow)`
- Testing for carryover effects
- Accounting for period effects

**Equation Coverage:**
- Latin Square model: Y_ijk = μ + ρ_i + κ_j + τ_k + ε_ijk
  - ρ for rows (e.g., cows), κ for columns (e.g., periods), τ for treatments
- Crossover model with carryover: Y_ijk = μ + π_k + γ_j(k-1) + τ_j + c_i + ε_ijk
  - Where γ is carryover effect

**Special Section:**
- Minimum washout period recommendations by species/trait
- When crossover designs are inappropriate (e.g., growth studies)

---

### Chapter 8: Sample Size, Power, and Precision
**Purpose:** Practical planning of experiments

**Content:**
- Power analysis framework (α, β, effect size, n)
- Sample size for CRD and RCBD
- Sample size for clustered data (accounting for ICC)
- Effect of block efficiency on sample size
- Power for factorial designs
- Power for detecting interactions (needs larger n!)
- Pilot studies and variance estimates
- Practical constraints in animal research

**Examples by Design:**
- Swine pen study: How many pens needed for detecting 5% improvement in gain?
  - Accounting for pen-level variation (ICC)
  - Trade-offs: more pens with fewer pigs vs fewer pens with more pigs

- Beef feedlot: Sample size for carcass trait differences

- Dairy: Power to detect milk production differences
  - Within-cow vs between-cow designs

- Poultry: Sample size for cage-level vs bird-level outcomes

- Meat science: How many carcasses needed for tenderness study?
  - Accounting for multiple steaks per carcass

**R Code:**
- `pwr` package for basic designs
- Custom functions for clustered designs
- `simr` package for mixed model power simulation
- Plotting power curves

**Equation Coverage:**
- Power formula: 1 - β = P(reject H₀ | H₁ is true)
- Effect size (Cohen's d): δ = (μ₁ - μ₂) / σ
- Design effect for clustered data: DE = 1 + (m - 1)ρ
  - Where m is cluster size, ρ is ICC
- Effective sample size: n_eff = n / DE
- Detectable difference given n and power

---

### Chapter 9: Analysis of Covariance (ANCOVA)
**Purpose:** Improving precision through covariate adjustment

**Content:**
- Purpose of ANCOVA (reduce error variance)
- Covariates vs blocking factors
- Model specification
- Adjusted means
- Assumptions (homogeneity of slopes)
- When to use baseline measurements as covariates

**Examples:**
- Swine: Adjusting final weight for initial weight in growth studies
  - Why this is almost always done

- Beef: Adjusting feed efficiency for metabolic mid-weight

- Dairy: Adjusting milk production for days in milk (DIM)

- Poultry: Adjusting feed conversion for body weight

- Sheep: Adjusting weaning weight for birth weight

- Meat science: Adjusting tenderness for marbling score or pH

**R Code:**
- `lm()` with covariate: `final_weight ~ treatment + initial_weight`
- `emmeans()` for adjusted means
- Testing homogeneity of slopes assumption
- Comparing models with/without covariate (precision gain)

**Equation Coverage:**
- ANCOVA model: Y_ij = μ + τ_i + β(X_ij - X̄) + ε_ij
- Adjusted treatment means
- Reduction in error variance: σ²_adj = σ²(1 - r²)
- Degrees of freedom for error term

**Special Section:**
- Should you use change scores (Δ = final - initial) or ANCOVA?
- ANCOVA is almost always more powerful

---

### Chapter 10: Special Topics and Common Pitfalls
**Purpose:** Advanced considerations and avoiding mistakes

**Content:**
- **Pseudoreplication: The Cardinal Sin**
  - What it is and why it happens
  - Examples from each species
  - How to avoid it

- **Confounding**
  - Treatment confounded with time, space, or management
  - Examples and solutions

- **Multiple Comparisons**
  - When to adjust (and when not to)
  - FDR vs Bonferroni vs Dunnett
  - Planned contrasts vs post-hoc tests

- **Unequal Sample Sizes**
  - Type I, II, III sums of squares
  - Missing data in mixed models (REML is your friend)

- **Outliers**
  - Identification and handling
  - Robust methods

- **Non-Normal Data**
  - Transformations
  - Generalized linear mixed models (GLMM) - brief intro

- **Species-Specific Recommendations**
  - Swine: Considerations for littermate assignment, pen effects, sex effects
  - Beef: Pasture/pen as experimental unit, implant timing
  - Dairy: Carryover effects, lactation curves, parity effects
  - Poultry: Cage/pen effects, high mortality, sex-separate analyses
  - Sheep: Litter effects (singles vs multiples), seasonal effects
  - Meat science: Sample storage, measurement error, subsampling

**Case Studies (Mini Examples):**
- Correcting a flawed design after data collection (what can be salvaged?)
- Designing around practical constraints (limited pens, budget, time)

**R Code:**
- Detecting outliers with diagnostics
- Type II/III SS with `car::Anova()`
- Multiple comparison adjustments with `emmeans()`

**Tables:**
- Common experimental designs by species and research question
- Troubleshooting guide: "Which design should I use?"
- Checklist before starting data collection

---

## Quarto Book Structure

### Repository Organization
```
ExperimentalDesign/
├── _quarto.yml                 # Book configuration
├── index.qmd                   # Book homepage/preface
├── references.bib              # Bibliography
├── chapters/
│   ├── 01-principles.qmd
│   ├── 02-crd-rcbd.qmd
│   ├── 03-factorial.qmd
│   ├── 04-nested-hierarchical.qmd
│   ├── 05-split-plot.qmd
│   ├── 06-repeated-measures.qmd
│   ├── 07-crossover-latin.qmd
│   ├── 08-sample-size-power.qmd
│   ├── 09-ancova.qmd
│   └── 10-special-topics.qmd
├── data/                       # Example datasets
│   ├── swine_growth.csv
│   ├── beef_feedlot.csv
│   ├── dairy_milk.csv
│   ├── poultry_eggs.csv
│   ├── sheep_performance.csv
│   └── meat_tenderness.csv
├── R/                          # Helper functions
│   └── analysis_functions.R
└── images/                     # Figures and diagrams
```

### _quarto.yml Configuration
```yaml
project:
  type: book
  output-dir: docs

book:
  title: "Experimental Design for Animal Science"
  subtitle: "A Practical Guide Using Livestock Examples"
  author: "Your Name"
  date: today
  chapters:
    - index.qmd
    - chapters/01-principles.qmd
    - chapters/02-crd-rcbd.qmd
    - chapters/03-factorial.qmd
    - chapters/04-nested-hierarchical.qmd
    - chapters/05-split-plot.qmd
    - chapters/06-repeated-measures.qmd
    - chapters/07-crossover-latin.qmd
    - chapters/08-sample-size-power.qmd
    - chapters/09-ancova.qmd
    - chapters/10-special-topics.qmd
  appendices:
    - appendices/datasets.qmd
    - appendices/r-resources.qmd

bibliography: references.bib

format:
  html:
    theme: cosmo
    toc: true
    toc-depth: 3
    number-sections: true
    code-fold: false
    code-tools: true
    link-external-newwindow: true
```

### Key Quarto Features to Use
- Code folding for optional viewing
- Tabsets for organizing multiple examples
- Callout blocks for important notes:
  - `{.callout-warning}` for common mistakes
  - `{.callout-tip}` for practical advice
  - `{.callout-note}` for important concepts
- Cross-references between chapters
- Equation numbering
- Figure and table captions
- Interactive exercises (optional, with webR)

---

## Pedagogical Approach

### Each Chapter Will Include:
1. **Learning Objectives** (3-5 specific outcomes)
2. **Motivating Example** (real-world scenario)
3. **Conceptual Explanation** (why this design?)
4. **Statistical Theory** (models and equations)
5. **Multiple Worked Examples** (different species)
6. **R Code Demonstrations** (fully reproducible)
7. **Interpretation Practice** (what does output mean?)
8. **Common Mistakes** (what to avoid)
9. **Practice Problems** (3-5 per chapter)
10. **Further Reading** (key references)

### Writing Style
- Direct and practical, not overly theoretical
- Start each example with research question
- Show design diagrams/schematics
- Step-by-step R code with comments
- Interpretation in plain English first, then statistical terms
- Emphasize biological interpretation, not just statistical significance

### Equations
- Present in three formats:
  1. Conceptual (words): "Response equals treatment effect plus pen effect plus residual"
  2. Mathematical notation: Y_ijk = μ + τ_i + p_ij + ε_ijk
  3. R syntax: `response ~ treatment + (1|pen)`

---

## Example Dataset Requirements

We need to create or source realistic datasets:

1. **Swine growth study** (nested design)
   - ~100 pigs in 20 pens
   - 4 dietary treatments
   - Weekly weights over 8 weeks

2. **Beef feedlot trial** (RCBD)
   - 40 pens, 4 blocks
   - 4 implant strategies
   - ADG, feed efficiency, carcass traits

3. **Dairy lactation study** (Latin Square)
   - 8 cows
   - 4 dietary treatments
   - 4 periods
   - Daily milk yield

4. **Poultry broiler study** (factorial)
   - 2 diets × 2 stocking densities
   - 24 pens (6 reps per treatment combo)
   - Growth and welfare measures

5. **Meat science tenderness** (split-plot)
   - 30 carcasses
   - 2 electrical stimulation treatments (whole plot)
   - 3 aging periods (sub-plot)
   - Warner-Bratzler shear force

6. **Sheep growth study** (ANCOVA)
   - 60 lambs
   - 3 creep feeding treatments
   - Birth weight as covariate
   - Weaning weight as outcome

---

## Next Steps

1. ✅ Finalize chapter outline (THIS DOCUMENT)
2. Set up Quarto book structure (_quarto.yml, folder organization)
3. Create/simulate example datasets
4. Write Chapter 1 (establish tone and style)
5. Develop Chapter 4 (nested designs - highest priority given the "pen problem")
6. Develop Chapter 5 (split-plot - second priority for meat science)
7. Complete remaining chapters
8. Create practice problems with solutions
9. Build references.bib with key literature
10. Render and test HTML output
11. Peer review with animal science faculty
12. Iterate based on feedback

---

## Key Messages to Emphasize Throughout

1. **The experimental unit is what receives the treatment** - this determines your n
2. **Independence assumption matters** - correlated observations need mixed models
3. **Design the experiment with the analysis in mind** - don't figure it out after data collection
4. **Replication at the correct level** - more pens with fewer pigs > fewer pens with more pigs
5. **Mixed models are your friend** - embrace random effects for realistic livestock data
6. **Know your variance components** - pen-to-pen variation vs animal-to-animal variation
7. **Plot your data** - always visualize before modeling
8. **Biological relevance ≠ statistical significance** - interpret effect sizes

---

## Success Metrics

This book will be successful if students can:
- Correctly identify experimental units in complex livestock scenarios
- Write appropriate mixed model code for nested and split-plot designs
- Avoid pseudoreplication in their thesis research
- Calculate realistic sample sizes for grant proposals
- Interpret statistical output in biologically meaningful ways
- Confidently analyze their own research data

---

## Deployment and Package Management

### GitHub Actions Deployment Setup

**Configured:** 2025-12-04

The book is now deployed automatically via GitHub Actions instead of local `docs/` directory.

**Live Site:** https://austin-putz.github.io/experimental-design-book/

**Repository:** https://github.com/austin-putz/experimental-design-book

#### Key Configuration Files

1. **`.github/workflows/publish.yml`** - Automated build and deploy workflow
   - Triggers on push to `main` branch
   - Installs system dependencies (cmake, libcurl, libx11, graphics libraries)
   - Sets up R 4.4.2 and restores renv packages
   - Renders Quarto book to `_site/`
   - Deploys to GitHub Pages
   - Build time: ~2-5 minutes (with caching)

2. **`renv.lock`** - R package version manifest
   - Ensures reproducible builds across environments
   - Contains exact versions of all R packages
   - Updated via `renv::snapshot()`

3. **`.gitignore`** - Excludes build artifacts
   - `docs/` - no longer used
   - `_site/` - build output (created by Quarto)
   - `renv/library/` - local R packages (not committed)
   - `_freeze/` - **KEPT in version control** for computation caching

4. **`_quarto.yml`** - Book configuration
   - `output-dir: _site` - outputs to `_site/` for GitHub Actions
   - `publish: gh-pages` - configured for GitHub Pages deployment

#### Current R Package Dependencies

**Core Packages (from renv.lock):**
- tidyverse (includes dplyr, ggplot2, tidyr, readr, purrr, tibble, stringr, forcats)
- lme4 - Linear mixed-effects models
- nlme - Nonlinear mixed-effects models
- emmeans - Estimated marginal means
- lmerTest - Tests for linear mixed models
- knitr - Dynamic report generation
- rmarkdown - R Markdown integration
- car - Companion to Applied Regression
- pwr - Power analysis
- simr - Power simulation for mixed models
- broom - Tidy model output

**Total Packages:** ~145 (including dependencies)

### Adding New R Packages

When you need to add a new R package to the book:

#### Step 1: Install and Snapshot Locally
```R
# In R console at project root
renv::install("packagename")

# Update the lockfile
renv::snapshot()
```

#### Step 2: Commit the Updated Lockfile
```bash
git add renv.lock
git commit -m "Add packagename to dependencies"
git push
```

#### Step 3: Automatic Deployment
- GitHub Actions automatically detects the new `renv.lock`
- Installs the new package during build
- Renders and deploys the updated book

**Note:** First build with a new package may take longer (~8-12 minutes) as it compiles from source. Subsequent builds use cached packages (~2-5 minutes).

### Local Development Workflow

#### Preview Book Locally
```bash
# Start interactive preview (auto-reloads on changes)
quarto preview

# Or render once
quarto render
# Output: _site/ directory (gitignored)
```

#### Make Changes and Deploy
```bash
# Edit .qmd files
git add chapters/01-principles.qmd
git commit -m "Update principles chapter with new example"
git push

# GitHub Actions automatically rebuilds and deploys
# Monitor at: https://github.com/austin-putz/experimental-design-book/actions
```

#### Check Deployment Status
```bash
# View recent workflow runs
gh run list --limit 5

# Watch current run
gh run watch

# View specific run logs
gh run view <run-id>
```

### Troubleshooting Common Issues

**Build Fails with Missing Package:**
- Ensure package is in `renv.lock` via `renv::snapshot()`
- Check package is available on CRAN
- View error logs: `gh run view --log-failed`

**Build Takes Too Long:**
- First build: 10-20 minutes (normal - compiling packages)
- Subsequent builds: 2-5 minutes (cached)
- If always slow, check if cache is being used

**_site/ Directory Not Found:**
- Ensure `output-dir: _site` in `_quarto.yml`
- Quarto books default to `_book/` without this setting

**System Dependency Missing:**
- Edit `.github/workflows/publish.yml`
- Add package to `Install system dependencies` step
- Ubuntu package names (e.g., `libcurl4-openssl-dev`)

### Repository Structure (Current)

```
experimental-design-book/
├── .github/
│   └── workflows/
│       └── publish.yml           # GitHub Actions workflow
├── .gitignore                    # Excludes docs/, _site/, renv/library/
├── _quarto.yml                   # Book config (output-dir: _site)
├── _freeze/                      # Computation cache (committed!)
├── renv.lock                     # R package versions (committed!)
├── .Rprofile                     # Activates renv (committed!)
├── renv/
│   ├── activate.R               # Bootstrap script (committed!)
│   ├── settings.json            # renv settings (committed!)
│   └── library/                 # Local packages (gitignored!)
├── chapters/
│   ├── 01-principles.qmd        # ~43KB, well-developed
│   ├── 02-10.qmd                # Skeleton files (78-114 bytes)
├── appendices/
│   ├── datasets.qmd
│   └── r-resources.qmd
├── data/                         # Example datasets (empty)
├── images/                       # Figures (empty)
├── R/                           # Helper functions (empty)
├── index.qmd                    # Book homepage
├── references.bib               # Bibliography
├── custom.scss                  # Custom styling
└── styles.css                   # Additional CSS
```

**Note:** `docs/` directory removed - no longer used for deployment.

---

**Document Version:** 1.1
**Last Updated:** 2025-12-04
**Status:** GitHub Actions deployment active and operational
