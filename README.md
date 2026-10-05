# Data Cleaning and Exploratory Data Analysis

A hands-on introduction to exploratory data analysis (EDA) with pandas, on a real and genuinely messy dataset. Working through a raw Seattle weather file, you first **clean** it: column names, duplicates, data types, units, categorical labels and missing values. You then **explore** it: distributions, outliers, and how columns relate to one another, whatever their types. The last notebook closes the loop by measuring how your cleaning choices changed your findings. This is the foundation for your first project.

The five notebooks are a **pipeline**. Each one saves its result and the next one picks it up, so you can work through them in order, stop for the day, and start again where you left off:

```
seattle-weather_raw.csv
        |
        v
  02_data_cleaning  --->  seattle-weather_clean.csv
                                    |
              +---------------------+---------------------+
              v                     v                     v
      03_missing_data   04_descriptive_statistics   05_correlation
                                                          |
                                                          v
                                          seattle-weather_features.csv
                                                          |
                                                          v
                                          06_mutual_information  (optional)
```

The two intermediate files are committed, so **every notebook runs on its own**. You do not have to finish 02 before starting 04.

**Notebooks 02 to 05 are the core path.** [06](06_mutual_information.ipynb) is optional: do it if you finish the others with time to spare, or come back to it before the Friday project.

## Learning Objectives

By the end of this repository, you should be able to:

**Cleaning (02, 03):**

- Inspect a raw dataset, identify its data quality issues, and fix them: column names, duplicates, data types, dates, units and categorical labels.
- Diagnose missing data with missingno, handle it by dropping or imputing, and show how the imputation choice changes the correlations you report.

**Exploring (04, 05):**

- Classify every column as categorical, ordinal or metric, and summarize it with central tendency and dispersion, saying when the mean misleads.
- Read distributions from histograms, boxplots and bar plots, and tell a data error apart from a genuine extreme value.
- Choose the right plot for a pair of columns from their types, and survey all numerical pairs at once with a pairplot and a masked correlation heatmap.
- Measure the relationship between any two columns with the right statistic (Pearson, Spearman, Kendall, chi-squared with Cramér's V, correlation ratio), and use mutual information when correlation misses it.

The optional notebook 06 covers mutual information and how imputation shifts correlations.

## Learning Path

**Scope** marks the core path every student is expected to complete. Optional lessons stay in the repository and are worth returning to, but the day does not depend on them. **Units** are indicative pacing, where one unit is 45 minutes.

The lessons build on each other in order, and each one has a single job: a reading that
frames the work, then raw to clean, then the gaps and what goes in them, then one column
at a time, then two columns at a time, and finally the relationships correlation cannot
see.

| File / Folder | Description | Scope | Units |
| --- | --- | --- | --- |
| [**1 - What Exploratory Data Analysis Is**](01_what_is_eda.md) | Reading on what an EDA is for, tidy data, the six ways data goes wrong, and why a missing value is not one thing but three. | Core | 0.5 |
| [**2 - Data Cleaning**](02_data_cleaning.ipynb) | Inspect the raw file, fix column names, drop duplicates, correct data types and units, and map the messy categorical labels onto real categories. Saves the cleaned data. | Core | 1.75 |
| [**3 - Missing Data**](03_missing_data.ipynb) | Count and visualize the gaps, look for a pattern in them, then drop or impute: a constant, a summary statistic, or interpolation. | Core | 1.5 |
| [**4 - Descriptive Statistics**](04_descriptive_statistics.ipynb) | One column at a time. Variable types, central tendency and dispersion, histograms and boxplots for numbers, bar plots for categories, and telling a data error apart from a genuine extreme value. | Core | 1.75 |
| [**5 - Correlation**](05_correlation.ipynb) | Two columns at a time, with the plot and the measure chosen together from the column types. Pearson, Spearman and Kendall for numbers, the chi-squared test and Cramer's V for categories, the correlation ratio for a mixed pair. Derives and saves `month` and `season`. | Core | 2 |
| [**6 - Mutual Information**](06_mutual_information.ipynb) | The measure that assumes nothing about the shape of a relationship, and a look back at what the imputation in 03 did to the correlations in 05. | Optional | 0.75 |

> [!NOTE]
> **Optional material.** Notebook 06 in full, and two subsections of notebook 03,
> *Flag the missing value* and *Prediction*. The latter two are short reads with no
> exercise, and prediction-based imputation needs modelling you have not met yet. Notebooks
> 02 to 05 are the path everyone completes.

### Additional Folders and Files

| File / Folder | Description |
|---|---|
| [**Data**](data/) | `seattle-weather_raw.csv`, the raw dataset, plus the two intermediate files the notebooks write and read. |
| [**Solutions**](solutions/) | Reference solutions, one per notebook. |
| [**pyproject.toml**](pyproject.toml) | Project configuration and dependencies. |
| [**uv.lock**](uv.lock) | Dependency lock file. |

## Setup

> [!NOTE]
> Throughout these steps, text in angle brackets like `<repo-name>` is a **placeholder**. Replace it, including the `< >` brackets, with your own value. For example, `cd <repo-name>` becomes `cd amle-data-cleaning-eda`.

### 1. Create the Repository from the Template

Click **Use this template** on GitHub.

When creating the repository:

- Set yourself as the **Owner**
- Choose a repository name
- Disable **Include all branches**
- Click **Create repository**

> [!IMPORTANT]
> If you are working in pairs or groups, only **one person** should complete this step.

---

### 2. Add Collaborators (Pairs/Groups Only)

If working with teammates:

1. Open the repository on GitHub
2. Go to **Settings → Collaborators**
3. Add your teammates as collaborators
4. Share the repository link with your team

Teammates should accept the invitation before continuing.

---

### 3. Clone the Repository

Copy the SSH URL from the **Code** button on GitHub, then run:

```bash
git clone <copied-ssh-url>
```

The copied SSH URL will look like `git@github.com:<your-username>/<repo-name>.git`.

---

### 4. Move into the Project Folder and Install Dependencies

This installs all dependencies and creates a virtual environment in `.venv/`.

```bash
cd <repo-name>
uv sync
```

---

### 5. Open the Notebook

> [!NOTE]
> Make sure you open VS Code from the project root so it automatically detects the environment created by `uv sync`.

Launch VS Code in the project root folder:

```bash
code .
```

Then read `01_what_is_eda.md`, and open `02_data_cleaning.ipynb`, selecting the Python environment created by `uv sync` as the kernel. Work through the lessons in order; 01 to 05 are the core path.

## References & Further Reading

- [**Pandas: Working with missing data**](https://pandas.pydata.org/docs/user_guide/missing_data.html): Official guide to detecting, dropping, and filling missing values.
- [**Pandas: Getting started tutorials**](https://pandas.pydata.org/docs/getting_started/intro_tutorials/index.html): Short, practical introductions to the core pandas workflow.
- [**Pythonic data cleaning with pandas and NumPy**](https://realpython.com/python-data-cleaning-numpy-pandas/): A worked tutorial that applies these same cleaning steps to other real datasets.
- [**Kaggle: Data Cleaning course**](https://www.kaggle.com/learn/data-cleaning): A short, free course with exercises on missing values, scaling, and inconsistent text entries.
- [**Tidy Data**](https://vita.had.co.nz/papers/tidy-data.pdf) (Wickham, 2014): The paper behind "one variable per column, one observation per row", and why that shape makes everything downstream easier.
- [**scipy.stats.chi2_contingency**](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.chi2_contingency.html): The chi-squared test of independence that Cramer's V is built on.
- [**sklearn: mutual information**](https://scikit-learn.org/stable/modules/feature_selection.html#univariate-feature-selection): `mutual_info_regression` and `mutual_info_classif`, the production versions of the estimator that notebook 06 first writes out longhand.
- [**Seaborn: statistical data visualization**](https://seaborn.pydata.org/tutorial.html): The plotting library used throughout notebooks 04 to 06.
