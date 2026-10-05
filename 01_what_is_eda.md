# What Exploratory Data Analysis Is

## Why this comes first

The rest of this repository is five notebooks of pandas. Before any of it, it is worth
knowing what you are doing and why, because the individual steps are easy and the judgement
is not. Anyone can call `.fillna()`. Knowing whether you should, and what it costs you, is
the actual skill.

This reading gives you the frame. The notebooks then apply it to a real and genuinely messy
file of Seattle weather.

---

## EDA is not a step, it is an attitude

The term comes from the statistician **John Tukey**, who argued in the 1970s that the
profession had become obsessed with confirming hypotheses and had forgotten how to find
them. His counter-proposal was **exploratory data analysis**: look at the data first,
without a hypothesis, and let it tell you what questions are worth asking.

That is still the point. An EDA is not a checklist you complete before the real work. It is
the part where you find out:

- what is actually in the dataset, as opposed to what the column names promise
- which values are wrong, which are merely surprising, and how to tell
- which columns carry information about the thing you care about
- which questions the data can answer, and which it cannot

You will usually leave an EDA with a different question than you entered with. **That is the
sign it worked.**

---

## The two halves

An EDA has two halves and this repository is organised around them.

**Making the data trustworthy.** Fix what is broken: column names you cannot type, rows
that appear twice, numbers stored as text, two different units in one column, five spellings
of the same category, and gaps. Notebooks 02 and 03.

**Finding out what it says.** Describe each column on its own, then measure how columns
relate to each other. Notebooks 04, 05 and 06.

The halves are not independent, and the last notebook exists to prove it: the choices you
make in the first half visibly change the answers you get in the second.

---

## Tidy data

Most cleaning work is moving a dataset towards one shape, which **Hadley Wickham** named
*tidy data*:

1. Each **variable** is a column.
2. Each **observation** is a row.
3. Each **type of observational unit** is a table.

This sounds like housekeeping and is not. Every tool you will use, pandas, seaborn,
scikit-learn, assumes this shape. Data that is not tidy needs a custom workaround at every
step, and those workarounds are where mistakes live.

It also explains why fixing column names is worth a section rather than being fussiness.
A column called `Temp Min` cannot be reached as `df.Temp Min`, and a column with no name at
all cannot be reached at all.

---

## Six ways data goes wrong

It helps to have names for the failures, because then you can go looking for them
deliberately instead of noticing them by accident. These are the standard **data quality
dimensions**, with the example from the file you are about to clean:

| Dimension | The question | In this dataset |
| --- | --- | --- |
| **Completeness** | Is anything missing? | 101 missing precipitation values, 349 missing maximum temperatures |
| **Validity** | Could this value be true at all? | A `$` character sitting in a column of rainfall measurements |
| **Consistency** | Does the same thing always look the same? | `drizzle`, `Drizzle`, `driz.` and `d` are one category wearing four costumes |
| **Uniqueness** | Is anything recorded twice? | 37 duplicated rows |
| **Accuracy** | Is the value right, even though it looks fine? | Minimum temperatures recorded in Fahrenheit while maximums are in Celsius |
| **Timeliness** | Is the data current enough to use? | Not an issue here, the file covers four complete years |

**Accuracy is the dangerous one.** The other five announce themselves: a missing value is
visibly missing, a `$` breaks a type conversion, a duplicate shows up in a count. An
inaccurate value looks perfectly normal. The Fahrenheit column is only detectable because
it produces minimum temperatures higher than the maximums, which is impossible. Nothing in
the data types, the row count, or any summary statistic would have flagged it.

Get in the habit of asking whether a number is *plausible*, not just whether it is present
and correctly typed.

---

## Six questions to ask any new dataset

Before writing code, open the file and ask:

1. **Names.** Can I type every column name? Does every column have one?
2. **Duplicates.** Is any row here more than once?
3. **Missing values.** Where are the gaps, and how many?
4. **Consistency.** Does each category have exactly one spelling?
5. **Types and units.** Is each column stored as what it means? Is any column in the wrong
   unit?
6. **Outliers.** Are there values that cannot be true, or that are merely extreme?

This is the list the first notebook works through, and it transfers unchanged to any other
dataset you explore.

---

## Missing data is not one thing

This distinction is due to **Donald Rubin**, and it decides what you are allowed to do about
a gap. There are three cases:

**MCAR, missing completely at random.** The gap has nothing to do with any value in the
dataset, observed or not. A sensor lost power at random moments. This is the harmless case:
dropping the affected rows loses information but does not bias what remains.

**MAR, missing at random.** The gap depends on something you *did* observe. Suppose the
weather station's thermometer was only read on weekdays. Temperature would then be missing
in a pattern, but a pattern you can see, because you have the date. You can account for it.

**MNAR, missing not at random.** The gap depends on the value that is missing. A rain gauge
that overflows and records nothing in the heaviest storms is missing precisely the largest
values. This is the dangerous case, because no amount of clever imputation recovers what was
never recorded, and every summary you compute will be biased low.

You usually cannot prove which case you are in. What you can do is **look**, which is why
notebook 03 plots the missing values rather than just counting them. Gaps scattered evenly
through the file suggest MCAR. Gaps that cluster, or that line up with another column,
suggest MAR or MNAR and are worth investigating before you fill anything.

> The common mistake is skipping this and going straight to `.fillna(df.mean())`, which
> silently assumes MCAR. If the data is MNAR, that single line quietly biases every result
> that follows.

---

## Cleaning decisions are analysis decisions

This is the idea the repository ends on, so it is worth planting now.

There is no neutral way to handle a missing value. Dropping the rows, filling with the mean,
and interpolating are all defensible, and they give you **different answers to the same
question**. Notebook 06 measures exactly this: three treatments of one column produce
correlations of 0.880, 0.777 and 0.880.

Nothing about the weather in Seattle changed between those three numbers. One line of code
did.

Two habits follow:

- **Decide deliberately**, with a reason you could defend, rather than reaching for whatever
  fills the gap fastest.
- **Report what you did.** A reader cannot reconstruct your imputation from your results,
  and a correlation of 0.78 supports a different conclusion from one of 0.88.

---

## What good looks like

An EDA is finished when you can answer these without looking anything up:

- What is one row of this dataset?
- Which columns are categorical, which are numerical, and which are neither?
- Where is the data missing, and what did I do about it, and why?
- Which values did I change or remove, and on what grounds?
- Which two columns are most strongly related, and does that make sense?
- What question should I actually be asking of this data?

If the last one has the same answer it had before you started, look again.

---

## References and further reading

- [**Tidy Data**](https://vita.had.co.nz/papers/tidy-data.pdf) (Wickham, 2014). The paper
  behind one variable per column. Short, readable, and worth the time.
- [**Exploratory Data Analysis**](https://archive.org/details/exploratorydataa0000tuke)
  (Tukey, 1977). The book that named the field. A historical read rather than a practical
  one, but the argument in the opening chapter still lands.
- [**Statistical Analysis with Missing Data**](https://doi.org/10.1002/9781119482260)
  (Little and Rubin). The source of MCAR, MAR and MNAR. Reference-level, dip into it.
- [**Pandas: working with missing data**](https://pandas.pydata.org/docs/user_guide/missing_data.html).
  The practical counterpart to the above.
