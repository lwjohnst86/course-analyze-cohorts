---
title: 'Presentation of results from cohort analyses'
description: 'Cohorts are important scientific sources of health and wellness information. Because of this, how results are presented needs to be carefully considered. The medium of presentation, be it plots or tables, can impact how the findings are seen and consumed. In this chapter we will cover some tips and ways of presenting cohort findings.'
---

## Presenting cohort findings is tricky, be careful

```yaml
type: VideoExercise
key: b1dbfa466f
xp: 50
```

`@projector_key`
24be66708a350c97c6e8d86a7b2f7bf4

---

## Adding more details to each model item in a list

```yaml
type: BulletExercise
key: 921d9175f4
xp: 100
```

Imagine you've ran several models. You:

- Scaled predictors to compare estimates.
- Set confounders and other predictors as baseline age, sex, smoking, and education.
- Have each predictor with unadjusted and adjusted models (time and subject ID were included in all).
- Tidied models and exponentiated estimates.

You have 8 models in total, stored as a list. We should add more details to each model to differentiate them from each other. Use `map()` from `purrr` to wrangle each model simultaneously. The new code here, `term[2]`, selects the main predictor, which is the second element in the `term` column.

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/07241773b32bd45d4d1eb59cb3524e5519e117e7/unadjusted_models_list.rda"))
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/dc0bc2887b3a9f02c65275512b851fd3d0a8ddee/adjusted_models_list.rda"))
library(dplyr)
library(purrr)
```

***

```yaml
type: NormalExercise
key: 0dffe6f56f
xp: 50
```

`@instructions`
- Using `map()`, add a `model` column to indicate the models are `"Unadjusted"`.

`@hint`
- Add the adjustment column using `model = "Unadjusted"`.

`@sample_code`
```{r}
# Add predictor and model type to each list item
unadjusted_models_list <- ___(
    unadjusted_models_list,
  	# .x is purrr for "model goes here"
    ~mutate(.x,
            # This selects predictor, not confounder
            predictor = term[2],
          	# Indicate model "adjustment"
            model = ___
           ))
```

`@solution`
```{r}
# Add predictor and model type to each list item
unadjusted_models_list <- map(
    unadjusted_models_list,
  	# .x is purrr for "model goes here"
    ~mutate(.x,
            # This selects predictor, not confounder
            predictor = term[2],
            # Indicate model "adjustment"
            model = "Unadjusted"
           ))
```

`@sct`
```{r}
success_msg("Nice!")
```

***

```yaml
type: NormalExercise
key: b91f78ffbb
xp: 50
```

`@instructions`
- Do the same thing for the `"Adjusted"` model list.

`@hint`
- Refer to each list object in `map` with `.x`.
- Include the `~` before `mutate`.

`@sample_code`
```{r}
# Add predictor and model type to each list item
adjusted_models_list <- map(
    adjusted_models_list,
  	# .x is purrr for "model goes here"
    ~mutate(.x,
          	# This selects predictor, not confounder
            predictor = term[2],
          	# Indicate model "adjustment"
            model = ___
           ))
```

`@solution`
```{r}
# Add predictor and model type to each list item
adjusted_models_list <- map(
    adjusted_models_list,
  	# .x is purrr for "model goes here"
    ~mutate(.x,
          	# This selects predictor, not confounder
            predictor = term[2],
          	# Indicate model "adjustment"
            model = "Adjusted"
           ))
```

`@sct`
```{r}
success_msg("Excellent! You made use of map to add more details to each model object.")
```

---

## Combining the list of models into one data frame

```yaml
type: NormalExercise
key: f30b964cce
xp: 100
```

The most efficient approach to later plotting and creating tables is to have all models in a single data frame. You've already prepared them a bit, now it's time to combine them together so you can continue working with them.

`@instructions`
- Use `bind_rows()` to combine `unadjusted_models_list` and `adjusted_models_list`.
- Continuing the pipe, add an `outcome` column with the value `"got_cvd"`.
- Finally, use `filter()` to keep only conditions where the `predictor` equals `term` and `effect` equals `"fixed"`.

`@hint`
- Filter when predictor and term are the same (`predictor == term`).

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/07241773b32bd45d4d1eb59cb3524e5519e117e7/unadjusted_models_list.rda"))
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/dc0bc2887b3a9f02c65275512b851fd3d0a8ddee/adjusted_models_list.rda"))
library(dplyr)
library(purrr)
unadjusted_models_list <- map(
    unadjusted_models_list,
    ~mutate(.x, predictor = term[2], model = "Unadjusted")
)
adjusted_models_list <- map(
    adjusted_models_list,
    ~mutate(.x, predictor = term[2], model = "Adjusted")
)
```

`@sample_code`
```{r}
all_models <- bind_rows(
  		# Combine the two lists of models 
  		___, 
  		___
	) %>% 
    mutate(outcome = ___) %>% 
	# Keep only predictor rows and fixed effects
    filter(___ == , ___ == ___)

all_models
```

`@solution`
```{r}
all_models <- bind_rows(
  		# Combine the two lists of models 
  		unadjusted_models_list, 
  		adjusted_models_list
	) %>% 
    mutate(outcome = "got_cvd") %>% 
	# Keep only predictor rows and fixed effects
    filter(predictor == term, effect == "fixed")

all_models
```

`@sct`
```{r}
success_msg("Well done! You've now bound all models together and continued wrangling them.")
```

---

## Communicating cohort findings through graphs

```yaml
type: VideoExercise
key: b1fd983ad5
xp: 50
```

`@projector_key`
d22c077f314d124f7ab5f28fa0423465

---

## Plotting model estimate and uncertainty

```yaml
type: TabExercise
key: 69007ab10b
xp: 100
```

Statistical analysis used on cohort data usually output some time of regression estimate along with a measure of uncertainty (e.g. 95% confidence interval). Sometimes it makes sense to present these results in a table, but often the better approach is to create a figure instead. Figures show magnitude, direction, uncertainty, and comparison of results very effectively.

Create a plot of the unadjusted model results that highlights the estimate and uncertainty of the estimate.

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/5a385fa413f9f3cfbe95857e390b53ae9966a62d/all_models.rda"))
library(dplyr)
library(ggplot2)
```

***

```yaml
type: NormalExercise
key: 19bfd714ac
xp: 30
```

`@instructions`
- For this exercise, filter to keep `model` that is equal to `"Unadjusted"`.

`@hint`
- Filter should be `model == "Unadjusted"`.

`@sample_code`
```{r}
# Keep only unadjusted models
unadjusted_results <- all_models %>% 
    filter(___ == ___)

# Check filtered data
unadjusted_results
```

`@solution`
```{r}
# Keep only unadjusted models
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")

# Check filtered data
unadjusted_results
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: 4ffd9e23ed
xp: 35
```

`@instructions`
- Set `y` as `predictor`, `x` as `estimate`, `xmin` as `conf.low`, and `xmax` as `conf.high`.
- Add `geom_point()` and `geom_errorbarh()` layers.

`@hint`
- The `errorbarh` (horizontal) requires `xmin = conf.low, xmax = conf.high`.

`@sample_code`
```{r}
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")

# Create a dot and error bar plot
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = ___, x = ___,
              xmin = ___, xmax = ___)) +
    ___() +
    ___()

# Check the plot
model_plot
```

`@solution`
```{r}
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")

# Create a dot and error bar plot
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = predictor, x = estimate, 
               xmin = conf.low, xmax = conf.high)) +
    geom_point() +
    geom_errorbarh()

# Check the plot
model_plot
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: f5fe48b671
xp: 35
```

`@instructions`
- Use `geom_vline()` to add a vertical line, setting `xintercept` as 1 for the "center line".

`@hint`
- Set `xintercept = 1` for the center line.

`@sample_code`
```{r}
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")

# Create a dot and error bar plot
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = predictor, x = estimate, 
               xmin = conf.low, xmax = conf.high)) +
    geom_point() +
    geom_errorbarh() +
    # Add vertical line
    ___(xintercept = ___)

# Check the plot
model_plot
```

`@solution`
```{r}
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")

# Create a dot and error bar plot
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = predictor, x = estimate, 
               xmin = conf.low, xmax = conf.high)) +
    geom_point() +
    geom_errorbarh() +
    # Add vertical line
    geom_vline(xintercept = 1)

# Check the plot
model_plot
```

`@sct`
```{r}
success_msg("Excellent! See how this graph shows the uncertainty around individual model estimates? This is an effective way of presenting results from models.")
```

---

## Create a more polished plot

```yaml
type: NormalExercise
key: f6857cd149
xp: 100
```

Now that we've created this plot, let's polish it up. We want it to be "publication quality", since we'll eventually present this figure to others.

As with the previous exercise, use the `unadjusted_results` data frame you created to plot the findings. This time, make the plot more polished and presentable.

`@instructions`
- Set the point `size` to 3, the error bar `height` to 0.1, and the `linetype` to `"dotted"`.
- Include appropriate axis labels (`"Predictors"` on the y and `"Odds Ratio (95% CI)"` on the x). Recall that CI means confidence interval.
- Set the theme to `theme_classic()`.

`@hint`
- The labels should be of the form `x = "Axis Label"` (e.g. for the x-axis).

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/5a385fa413f9f3cfbe95857e390b53ae9966a62d/all_models.rda"))
library(dplyr)
library(ggplot2)
unadjusted_results <- all_models %>% 
    filter(model == "Unadjusted")
```

`@sample_code`
```{r}
# Make the plot more polished
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = predictor, x = estimate, xmin = conf.low, xmax = conf.high)) +
    geom_point(size = ___) +
    geom_errorbarh(height = ___) +
    geom_vline(xintercept = 1, linetype = ___) +
    labs(y = ___, x = ___) +
	# Set the theme
    ___()
model_plot
```

`@solution`
```{r}
# Make the plot more polished
model_plot <- unadjusted_results %>% 
    ggplot(aes(y = predictor, x = estimate, xmin = conf.low, xmax = conf.high)) +
    geom_point(size = 3) +
    geom_errorbarh(height = 0.1) +
    geom_vline(xintercept = 1, linetype = "dotted") +
    labs(y = "Predictors", x = "Odds ratio (95% CI)") +
	# Set the theme
    theme_classic()
model_plot
```

`@sct`
```{r}
success_msg("Amazing! You have a very nice figure now that is ready to be presented to others! There are other themes to use if you don't like this one. Check out the package ggthemes for more.")
```

---

## Visualize unadjusted and adjusted model results

```yaml
type: NormalExercise
key: 163d46569b
xp: 100
```

The STROBE best practices indicate to show both "crude" (unadjusted) and adjusted model results. Showing both can be informative and insightful into the research question. Create a plot of your results showing both unadjusted and adjusted models. Do the same steps as in the previous exercise for creating the plot.

`@instructions`
- Use the `all_models` data frame this time.
- Using `facet_grid()` to split the plot by `rows` with the `model` variable (called inside `vars()`).

`@hint`
- The faceting variable should be called as `vars(model)`.

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/5a385fa413f9f3cfbe95857e390b53ae9966a62d/all_models.rda"))
library(ggplot2)
library(magrittr)
```

`@sample_code`
```{r}
# Show results of both adjusted and unadjusted
plot_all_models <- ___ %>% 
    ggplot(aes(y = predictor, x = estimate, xmin = conf.low, xmax = conf.high)) +
    geom_point(size = 3) +
    geom_errorbarh(height = 0.1) +
    geom_vline(xintercept = 1, linetype = "dotted") +
    # Facet plot by model
    ___(rows = vars(___)) +
    labs(y = "Predictors", x = "Odds ratio (95% CI)") +
    theme_classic()
plot_all_models
```

`@solution`
```{r}
# Show results of both adjusted and unadjusted
plot_all_models <- all_models %>% 
    ggplot(aes(y = predictor, x = estimate, xmin = conf.low, xmax = conf.high)) +
    geom_point(size = 3) +
    geom_errorbarh(height = 0.1) +
    geom_vline(xintercept = 1, linetype = "dotted") +
    # Split plot by model
    facet_grid(rows = vars(model)) +
    labs(y = "Predictors", x = "Odds ratio (95% CI)") +
    theme_classic()
plot_all_models
```

`@sct`
```{r}
success_msg("Great job! You can see that there are large differences in some of the results between unadjusted and adjusted models.")
```

---

## Use tables effectively to show your results

```yaml
type: VideoExercise
key: fceefd27c4
xp: 50
```

`@projector_key`
8ecd82ad25933a2add144c7e10605b7d

---

## Present the basic characteristics of the cohort

```yaml
type: TabExercise
key: cb7b2cfe06
xp: 100
```

A classic use for tables is showing the basic characteristics of a cohort dataset, as there are diverse data types and summary statistics that need to be shown. Including a basic participant characteristics table is part of the STROBE best practices. This table can be quite informative for others when they interpret your analysis results.

Using the `carpenter` package, create a table showing summary statistics for each data collection visit.

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/25722a0770c3779d3290fd5628362c56a9d7d21b/tidied_framingham.rda"))
library(carpenter)
library(dplyr)
```

***

```yaml
type: NormalExercise
key: c653bbf2fa
xp: 25
```

`@instructions`
- Set `"followup_visit_number"` for the `header` argument of `outline_table()`.

`@hint`
- Use quotes around `followup_visit_number`.

`@sample_code`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
	# These discrete variables are numeric, but must be factors
	mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
	# Set followup visit number as table column
    outline_table(header = ___) 

# Check the table
characteristics_table
```

`@solution`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
	# These discrete variables are numeric, but must be factors
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
	# Set followup visit number as table column
    outline_table(header = "followup_visit_number") 

# Check the table
characteristics_table
```

`@sct`
```{r}
success_msg("Nice!")
```

***

```yaml
type: NormalExercise
key: 3f50416623
xp: 25
```

`@instructions`
- Add a row for the `"got_cvd"`, `"sex"`, and `"education_combined"` variables, using `stat_nPct` for the `stat` argument.

`@hint`
- The variables should be quoted, e.g. `"sex"`.

`@sample_code`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    # Show n (%) for discrete variables as rows
    add_rows(c(___, ___, ___), stat = ___)

# Check the table
characteristics_table
```

`@solution`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    # Show n (%) for discrete variables as rows
    add_rows(c("got_cvd", "sex", "education_combined"), stat = stat_nPct)

# Check the table
characteristics_table
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: cf370dcbc3
xp: 25
```

`@instructions`
- Add rows for `"total_cholesterol"`, `"body_mass_index"`, and `"participant_age"` using `stat_medianIQR` for the `stat` argument.

`@hint`
- The variables need to be surrounded by quotes, just like the function above.

`@sample_code`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    add_rows(c("got_cvd", "sex", "education_combined"), stat = stat_nPct) %>% 
    # Show median (range) for continuous variables
    add_rows(c(___, ___, ___), stat = ___)

# Check the table
characteristics_table
```

`@solution`
```{r}
# Create a table of summary statistics
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    add_rows(c("got_cvd", "sex", "education_combined"), stat = stat_nPct) %>% 
    # Show median (range) for continuous variables
    add_rows(c("total_cholesterol", "body_mass_index", "participant_age"), stat = stat_medianIQR)

# Check the table
characteristics_table
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: 81f9f129af
xp: 25
```

`@instructions`
- Rename the table `"header"` to `"Measures"`, `"Baseline"`, `"Second followup"`, and `"Third followup"`, then `build_table()` to markdown format.

`@hint`
- The new column headers should be given as a character vector.

`@sample_code`
```{r}
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    add_rows(c("got_cvd", "sex", "education_combined"), stat = stat_nPct) %>% 
    add_rows(c("participant_age", "body_mass_index", "total_cholesterol"), stat = stat_medianIQR) %>% 
    # Rename headers to better titles
    renaming("header", c(___, ___, ___, ___))

# Build the table and convert to markdown form
build_table(___)
```

`@solution`
```{r}
characteristics_table <- tidied_framingham %>% 
    mutate_at(vars(followup_visit_number, got_cvd), as.factor) %>% 
    outline_table(header = "followup_visit_number") %>% 
    add_rows(c("got_cvd", "sex", "education_combined"), stat = stat_nPct) %>% 
    add_rows(c("participant_age", "body_mass_index", "total_cholesterol"), stat = stat_medianIQR) %>% 
    # Rename headers to better titles
    renaming("header", c("Measures", "Baseline", "Second followup", "Third followup"))

# Build the table and convert to markdown form
build_table(characteristics_table)
```

`@sct`
```{r}
success_msg("Nice job! You've gotten the data formatted as a table for easy inclusion in a document or report and have provided basic participant characteristics from each cohort visit.")
```

---

## Supplemental tables of raw numbers for results

```yaml
type: TabExercise
key: e1034d1d86
xp: 100
```

While the main messaging and presentation of results should emphasize figures over tables, often it is useful to other researchers (especially those doing meta-analyses or aggregating results) that the raw model results be given as well. Here we can use tables to give this data, as a supplement to the figure.

Provide the estimates and confidence intervals of the unadjusted and adjusted model results in a table format that you could include in a document or report. The packages `glue`, `stringr`, and `knitr` have been loaded.

`@pre_exercise_code`
```{r}
load(url("https://assets.datacamp.com/production/repositories/2079/datasets/5a385fa413f9f3cfbe95857e390b53ae9966a62d/all_models.rda"))
library(knitr)
library(glue)
library(dplyr)
library(tidyr)
library(stringr)
```

***

```yaml
type: NormalExercise
key: f26d103841
xp: 35
```

`@instructions`
- Round the `estimate`, `conf.low`, and `conf.high` to 2 digits using the function `round` (don't use with `()`).

`@hint`
- `mutate_at()` takes variables (as the first argument) inside `vars()` and applies a function like `round` as the second argument.

`@sample_code`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    # Round values of variables to 3
    mutate_at(vars(___, ___, ___), ___, digits = ___)

# View wrangled data
table_model_results
```

`@solution`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    # Round values of variables to 3
    mutate_at(vars(estimate, conf.low, conf.high), round, digits = 2)

# View wrangled data
table_model_results
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: 3934cab889
xp: 35
```

`@instructions`
- Use `glue()` to create a new variable in the form `estimate (conf.low, conf.high)`, then replace underscores with spaces in `predictor` with `str_replace_all()`.

`@hint`
- The estimate and CI variables should be placed inside the `{}` in `glue()`.

`@sample_code`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    mutate_at(vars(estimate, conf.low, conf.high), round, digits = 2) %>% 
    # Use glue function to combine variables
    mutate(estimate_ci = glue("{___} ({___}, {___})"),
           # Underscores to spaces in predictor
           predictor = str_replace_all(___, "_", " "))

# View wrangled data
table_model_results
```

`@solution`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    mutate_at(vars(estimate, conf.low, conf.high), round, digits = 2) %>% 
    # Use glue function to combine variables
    mutate(estimate_ci = glue("{estimate} ({conf.low}, {conf.high})"),
           # Underscores to spaces in predictor
           predictor = str_replace_all(predictor, "_", " "))

# View wrangled data
table_model_results
```

`@sct`
```{r}
success_msg("Amazing!")
```

***

```yaml
type: NormalExercise
key: f015c03292
xp: 30
```

`@instructions`
- Keeping `model`, `predictor`, and `estimate_ci` variables, use `spread` on `model` and `estimate_ci`.
- Create the formatted table with `kable()`.

`@hint`
- `spread` takes two arguments: 1) the current discrete column (`model`) that will be the new columns names, and 2) the values (`estimate_ci`) that will be in the new columns.

`@sample_code`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    mutate_at(vars(estimate, conf.low, conf.high), round, digits = 2) %>% 
    mutate(estimate_ci = glue("{estimate} ({conf.low}, {conf.high})"),
           predictor = predictor %>% 
               str_remove("_scaled") %>% 
               str_replace_all("_", " ")) %>%
    # Keep then spread variables for final table
    select(___, ___, ___) %>% 
    spread(___, ___)

# Create a Markdown table
kable(___, caption = "Estimates and 95% CI from all models.")
```

`@solution`
```{r}
# Prepare the results for the table
table_model_results <- all_models %>% 
    mutate_at(vars(estimate, conf.low, conf.high), round, digits = 2) %>% 
    mutate(estimate_ci = glue("{estimate} ({conf.low}, {conf.high})"),
           predictor = predictor %>% 
               str_remove("_scaled") %>% 
               str_replace_all("_", " ")) %>%
    # Keep then spread variables for final table
    select(model, predictor, estimate_ci) %>% 
    spread(model, estimate_ci)

# Create a Markdown table
kable(table_model_results, caption = "Estimates and 95% CI from all models.")
```

`@sct`
```{r}
success_msg("Amazing! You've wrangled the data and prepared it to be presented as a table! You can now easily add this to your manuscript (especially easy if you use R Markdown).")
```
