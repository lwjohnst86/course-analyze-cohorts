---
title: 'Exploring, wrangling, and transforming cohort data'
description: 'Before statistically analyzing cohort data, you''ll need to explore and wrangle it into an appropriately analyzable format. You''ll also learn about some common transformations to apply to variables in cohort studies.'
---

## Pre-wrangling exploration

```yaml
type: VideoExercise
key: f20c0dcfb8
xp: 50
```

`@projector_key`
146b85090bb8ab77efbfe45c5c751f5d

---

## Plot univariate distributions

```yaml
type: BulletExercise
key: 78574fab0c
xp: 100
```

Let's get comfortable creating some univariate histograms to start exploring the data. Create several histograms of a couple variables. The `ggplot2` package has been loaded.

`@pre_exercise_code`
```{r}
tidier_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/70cb0511fa2caed6e774bbb26e65ac55660ae8c9/tidier_framingham.rds"))
library(ggplot2)
```

***

```yaml
type: NormalExercise
key: c1d2dff125
xp: 50
```

`@instructions`
- Set `x` to `participant_age` and add a `geom_histogram()` layer.

`@hint`
- In the `aes()`, the argument should be `x = participant_age`.

`@sample_code`
```{r}
# Examine the age histogram
ggplot(tidier_framingham, aes(x = ___)) +
    ___()
```

`@solution`
```{r}
# Examine the age histogram
ggplot(tidier_framingham, aes(x = participant_age)) +
    geom_histogram()
```

`@sct`
```{r}
success_msg("Nice!")
```

***

```yaml
type: NormalExercise
key: ca8520f319
xp: 50
```

`@instructions`
- Do the same thing, but set `x` to `systolic_blood_pressure`.

`@hint`
- The `aes()` should have `x = systolic_blood_pressure`.

`@sample_code`
```{r}
# Examine the systolic blood pressure histogram
ggplot(tidier_framingham, aes(x = ___)) +
    ___()
```

`@solution`
```{r}
# Examine the systolic blood pressure histogram
ggplot(tidier_framingham, aes(x = systolic_blood_pressure)) +
    geom_histogram()
```

`@sct`
```{r}
success_msg("Great job! You've created histograms and examined two variables.")
```

---

## Long data and visualizing variables over time

```yaml
type: TabExercise
key: 6e98f4c451
xp: 100
```

Now that you've learned how to create histograms, let's convert some of the Framingham dataset into the long data format using `gather()`. Then, using the long data form, create histograms for multiple variables simultaneously for each followup visit. This will give us a quick overview of the data and their distribution. Pay attention to how the distribution of each variable looks like.

`@pre_exercise_code`
```{r}
tidier_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/70cb0511fa2caed6e774bbb26e65ac55660ae8c9/tidier_framingham.rds"))
library(dplyr)
library(tidyr)
library(ggplot2)
```

***

```yaml
type: NormalExercise
key: 245888bee5
xp: 25
```

`@instructions`
- Select the variables `total_cholesterol`, `high_density_lipoprotein`, and `low_density_lipoprotein`.
- Using `gather()`, set the two new column names as `variable` and `value`, and then exclude `followup_visit_number` from being "gathered" (using the `-`).

`@hint`
- The `gather()` function should look like `gather(variable, value, -followup_visit_number)`.

`@sample_code`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three cholesterol-based variables
        ___, ___, ___
    ) %>%
    gather(___, ___, -___)
```

`@solution`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three cholesterol-based variables
        total_cholesterol, high_density_lipoprotein, low_density_lipoprotein
    ) %>%
    gather(variable, value, -followup_visit_number)
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: e13f252e66
xp: 25
```

`@instructions`
- `facet_wrap()` by the variables `followup_visit_number` and `variable`. Don't forget to use the `vars()` function.

`@hint`
- The `facet_wrap()` variables need to be within the `vars()` function and separated by a comma.

`@sample_code`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three cholesterol-based variables
        total_cholesterol, high_density_lipoprotein, low_density_lipoprotein
    ) %>%
    gather(variable, value, -followup_visit_number) %>%
    ggplot(aes(x = value)) +
    geom_histogram() +
    # Facet by followup and variables
    ___(___(___, ___), 
        scales = "free")
```

`@solution`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three cholesterol-based variables
        total_cholesterol, high_density_lipoprotein, low_density_lipoprotein
    ) %>%
    gather(variable, value, -followup_visit_number) %>%
    ggplot(aes(x = value)) +
    geom_histogram() +
    # Facet by followup and variables
    facet_wrap(vars(followup_visit_number, variable), 
               scales = "free")
```

`@sct`
```{r}
success_msg("Great!")
```

***

```yaml
type: NormalExercise
key: 378ed55252
xp: 25
```

`@instructions`
- Select new variables `participant_age`, `body_mass_index`, and `cigarettes_per_day`, then run the plot again.

`@hint`
- Put the variables in the `select()` function.

`@sample_code`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three charactistics
        ___, ___, ___
    ) %>%
    gather(variable, value, -followup_visit_number) %>%
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(followup_visit_number, variable), 
               scales = "free")
```

`@solution`
```{r}
tidier_framingham %>%
    select(
        followup_visit_number,
        # Select the three charactistics
        body_mass_index, participant_age, cigarettes_per_day
    ) %>%
    gather(variable, value, -followup_visit_number) %>%
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(followup_visit_number, variable), 
               scales = "free")
```

`@sct`
```{r}
success_msg("Amazing!")
```

***

```yaml
type: MultipleChoiceExercise
key: cf90249310
xp: 25
```

`@question`
There were several things to observe from the distributions of the variables and some things to consider for later analyses. Did you notice a few of them?

Which of the answers below describes some observations about the data.

`@possible_answers`
- The lipoprotein data was not available at visits 1 and 2.
- Most people smoked zero cigarettes per day.
- The participants' age had a "jagged", uneven distribution.
- [All of the above.]
- None of the above.

`@hint`
- Run the code again and check the histogram plots.

`@sct`
```{r}
success_msg("Great job! These types of observations are important to consider and examine, as they can profoundly influence later inferential analyses.")
```

---

## Visually examine the outcomes with the exposures

```yaml
type: NormalExercise
key: 1100af6e1e
xp: 100
```

Boxplots are great for showing a distribution by a grouping variable (e.g. sex or disease status). Create multiple boxplots of several exposure variables with the outcome variable (CVD) by combining what we learned previously about converting to long form and using faceting. Since we want to plot CVD status on the x-axis, we'll need to exclude it from being "gathered".

`@instructions`
- Select the variables `got_cvd`, `total_cholesterol`, `participant_age`, and `body_mass_index`.
- Also exclude `got_cvd` from the `gather()` function and set `value` for the y-axis in `aes()`.
- Add a `geom_boxplots()` layer.
- Lastly, flip the plot using `coord_flip()`.

`@hint`
- The initial `ggplot2` setup should be `ggplot(aes(x = value, y = variable))`.
- Include `-got_cvd` after `-followup_visit_number` in `gather()`.

`@pre_exercise_code`
```{r}
tidier_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/70cb0511fa2caed6e774bbb26e65ac55660ae8c9/tidier_framingham.rds"))
library(dplyr)
library(tidyr)
library(ggplot2)
tidier_framingham <- tidier_framingham %>% 
    mutate(got_cvd = as.character(got_cvd))
```

`@sample_code`
```{r}
tidier_framingham %>% 
    select(followup_visit_number,
           # Select the disease and the three continuous variables
           ___, ___,
           ___, ___) %>% 
    # Exclude also the disease
    gather(variable, value, -followup_visit_number, -___) %>% 
    ggplot(aes(y = ___, x = variable)) +
    # Plot boxplots
    ___() +
    facet_wrap(vars(followup_visit_number), ncol = 1) +
    # Flip the plot
    ___()
```

`@solution`
```{r}
tidier_framingham %>% 
    select(followup_visit_number,
           # Select the disease and the three continuous variables
           got_cvd, total_cholesterol,
           participant_age, body_mass_index) %>% 
    # Exclude also the disease
    gather(variable, value, -followup_visit_number, -got_cvd) %>% 
    ggplot(aes(y = value, x = variable)) +
    # Plot boxplots
    geom_boxplot() +
    facet_wrap(vars(followup_visit_number), ncol = 1) +
    # Flip the plot
    coord_flip()
```

`@sct`
```{r}
success_msg("Excellent! You quickly created a figure showing several continuous variables by the outcome, and over time! Notice how some variables are a bit higher in the `got_cvd` group and that over time these differences decreased? Also notice the problem of showing multiple variables that have vastly different values such as between body mass and cholesterol.")
```

---

## Discrete data and tidying it for later analysis

```yaml
type: VideoExercise
key: 3d338af036
xp: 50
```

`@projector_key`
4e1f8ff56b37d8caee655cf2b0b4639d

---

## Make discrete variables human-readable

```yaml
type: BulletExercise
key: e916c33326
xp: 100
```

As you may have noticed, there are several discrete variables with ambiguous values. For instance, sex has the values as either 1 or 2, but what do those numbers mean? Often, you will encounter discrete data as integers rather than descriptive strings when working with cohort datasets. With data like this, you need to have a data dictionary to know what the numbers mean. Let's fix this problem and tidy up the data so it is more intuitive and descriptive.

`@pre_exercise_code`
```{r}
tidier_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/70cb0511fa2caed6e774bbb26e65ac55660ae8c9/tidier_framingham.rds"))
library(dplyr)
```

***

```yaml
type: NormalExercise
key: 556d51535a
xp: 50
```

`@instructions`
- Tidy education up with `case_when()` to: 1 = "0-11 years"; 2 = "High School"; 3 = "Vocational"; 4 = "College".

`@hint`
- The form for the `case_when()` should look like `education == 1 ~ "0-11 years"`, for each number-string pairing.

`@sample_code`
```{r}
tidier2_framingham <- tidier_framingham %>% 
    mutate(education = case_when(
      # Use the format: variable == number ~ "string"
      education == ___ ~ ___,
      education == ___ ~ ___,
      education == ___ ~ ___,
      education == ___ ~ ___,
      TRUE ~ NA_character_))

# Check changed education
count(tidier2_framingham, education)
```

`@solution`
```{r}
tidier2_framingham <- tidier_framingham %>% 
    mutate(education = case_when(
      # Use the format: variable == number ~ "string"
      education == 1 ~ "0-11 years",
      education == 2 ~ "High School",
      education == 3 ~ "Vocational",
      education == 4 ~ "College",
      TRUE ~ NA_character_))

# Check changed education
count(tidier2_framingham, education)
```

`@sct`
```{r}
success_msg("Excellent!")
```

***

```yaml
type: NormalExercise
key: 57c4db5e65
xp: 50
```

`@instructions`
- Do the same thing for the `sex` variable, to: 1 = "Man"; 2 = "Woman".

`@hint`
- The form for the `case_when()` should look like `sex == 1 ~ "Man"`, for each number-string pairing.

`@sample_code`
```{r}
tidier2_framingham <- tidier_framingham %>% 
    mutate(sex = case_when(
      # Use the format: variable == number ~ "string"
      sex == ___ ~ ___,
      sex == ___ ~ ___,
      TRUE ~ NA_character_))
    
# Check changed education
count(tidier2_framingham, sex)
```

`@solution`
```{r}
tidier2_framingham <- tidier_framingham %>% 
    mutate(sex = case_when(
      # Use the format: variable == number ~ "string"
      sex == 1 ~ "Man",
      sex == 2 ~ "Woman",
      TRUE ~ NA_character_))
    
# Check changed education
count(tidier2_framingham, sex)
```

`@sct`
```{r}
success_msg("Awesome! You've tidied up discrete values to be understandable to humans!")
```

---

## Merge factor categories together

```yaml
type: NormalExercise
key: 62bcf49a5e
xp: 100
```

Sometimes, categorical variables (as factors or characters) have many levels but only a few observations in one or more of the levels. It might make sense to combine categories together for some analyses or particular questions.

The `forcats` package has been preloaded as well as the previous `tidier2_framingham` dataset you tidied.

`@instructions`
- Recode the levels of `"Vocational"` and `"College"` education so both are named `"Post-Secondary"` using `fct_recode()`.
- Confirm the education levels have been correctly recoded using `count()`.
- You'll get a warning message about `NA` values. Ignore it as it doesn't matter.

`@hint`
- `fct_recode()` recoding should be in the form `"new name" = "old name"`, for example: `"Post-Secondary" = "College"`.

`@pre_exercise_code`
```{r}
tidier2_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/16a8a17e784e845c75eb7fe15899683684e89a22/tidier2_framingham.rds"))
library(forcats)
library(dplyr)
tidier2_framingham$education_combined <- NULL
```

`@sample_code`
```{r}
tidier2_framingham <- tidier2_framingham %>% 
    mutate(education_combined = ___(
        # Merge college and vocational levels
        education, 
        # Form is: "new" = "old"
        ___ = ___,
        ___ = ___))

# Confirm changes to variable
count(tidier2_framingham, ___)
```

`@solution`
```{r}
tidier2_framingham <- tidier2_framingham %>% 
    mutate(education_combined = fct_recode(
        # Merge college and vocational levels
        education, 
        # Form is: "new" = "old"
        "Post-Secondary" = "College",
        "Post-Secondary" = "Vocational"))

# Confirm changes to variable
count(tidier2_framingham, education_combined)
```

`@sct`
```{r}
success_msg("Great! You've combined two factor levels together into a new level.")
```

---

## Variable transformations

```yaml
type: VideoExercise
key: bfcfbe9aa2
xp: 50
```

`@projector_key`
5d026dadac109f3540f3c1f59a6f96ea

---

## Apply variable transformations

```yaml
type: NormalExercise
key: c812627e90
xp: 100
```

There are several types of transformations you can choose from. Which one you choose depends on the question, the type of data and their values (e.g. discrete vs continuous), the statistical method you will use, and how you want your results to be interpreted.

Recall the form for `mutate_at()` is:

```{r}
mutate_at(
    # List variables in here:
    vars(...), 
    # List functions in here, with name-function pair:
    list(name = function, ...)
)
```

`@instructions`
- In `vars()`, add `body_mass_index` and `cigarettes_per_day`.
- In `list()`, add `log`, `sqrt`, and `invert` (this function is loaded).
- Check how these variables changed by selecting the two original variables names using the `contains()` function and piping to `summary()`.

`@hint`
- The `select()` function form should look like `contains("body_mass_index")`.

`@pre_exercise_code`
```{r}
tidier2_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/16a8a17e784e845c75eb7fe15899683684e89a22/tidier2_framingham.rds"))
library(dplyr)
invert <- function(x) 1 / x
```

`@sample_code`
```{r}
# Use three transformations on body mass index
transformed_framingham <- tidier2_framingham %>% 
    mutate_at(vars(___, ___), 
              list(___ = ___, ___ = ___, ___ = ___))

# Check the created variable summaries
transformed_framingham %>% 
    select(contains(___), 
           contains(___)) %>% 
    summary()
```

`@solution`
```{r}
# Use three transformations on body mass index
transformed_framingham <- tidier2_framingham %>% 
    mutate_at(vars(body_mass_index, cigarettes_per_day), 
              list(log = log, sqrt = sqrt, invert = invert))

# Check the created variable summaries
transformed_framingham %>% 
    select(contains("body_mass_index"), 
           contains("cigarettes_per_day")) %>% 
    summary()
```

`@sct`
```{r}
success_msg("Excellent! You've transformed two variables into several forms.")
```

---

## Compare the different transformations

```yaml
type: BulletExercise
key: a4867af13b
xp: 100
```

Visualize how each transformation influences the distribution of the data. Graphing these transformations can provide insight into helping you choose a transformation for the variable.

Since we have several transformations, we want to plot them all on one plot. As we've done several times throughout the course, we need to use a long data format combined with facets to achieve this.

The `transformed_framingham` dataset you previously wrangled has been loaded.

`@pre_exercise_code`
```{r}
transformed_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/f6e38ca6a70fe7f5e38d234d11d42fd19603a37f/transformed_framingham.rds"))
library(tidyr)
library(dplyr)
library(ggplot2)
```

***

```yaml
type: NormalExercise
key: e0ccd581d4
xp: 50
```

`@instructions`
- Pipe `transformed_framingham` into `select()` and use `contains()` to keep variables with `body_mass_index` in the name.

`@hint`
- Select the variables with `contains("body_mass_index")`.

`@sample_code`
```{r}
# Plot a histogram of body mass transforms
bmi_transforms_plot <- ___ %>% 
	# Keep variables with string in variable name
    select(contains(___)) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")

bmi_transforms_plot
```

`@solution`
```{r}
# Plot a histogram of body mass transforms
bmi_transforms_plot <- transformed_framingham %>% 
	# Keep variables with string in variable name
    select(contains("body_mass_index")) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")

bmi_transforms_plot
```

`@sct`
```{r}
success_msg("Amazing!")
```

***

```yaml
type: NormalExercise
key: 8588b94514
xp: 50
```

`@instructions`
- Now do the same thing for `cigarettes_per_day`.

`@hint`
- Use `contains("cigarettes_per_day")`.

`@sample_code`
```{r}
# Plot a histogram of cigarettes per day transforms
cpd_transforms_plot <- transformed_framingham %>% 
	# Keep variables with string in variable name
    select(contains("___")) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")

cpd_transforms_plot
```

`@solution`
```{r}
# Plot a histogram of cigarettes per day transforms
cpd_transforms_plot <- transformed_framingham %>% 
	# Keep variables with string in variable name
    select(contains("cigarettes_per_day")) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")

cpd_transforms_plot
```

`@sct`
```{r}
success_msg("Great! Check out how each transformation influences the distribution of body mass index and of the number of cigarettes smoked.")
```

---

## How does the distribution change?

```yaml
type: MultipleChoiceExercise
key: ca708dca27
xp: 50
```

Understanding how each transformation influences the units and the distribution of the data is an important step in properly applying these transformations. Try answering these questions about the shape of the data after each transformation.

Both `bmi_transforms_plot` and `cpd_transforms_plot` are loaded for you to examine. Looking at the graphs, observe how each transformation influences the distribution of body mass index or cigarettes per day and think about how these new distributions might influence later analyses. 

Which statement is true?

`@possible_answers`
- Square root and scale don't change the distribution but do change the unit.
- Logarithm changes the distribution and unit.
- Body mass already has a good distribution and has the original unit.
- Scale can make interpreting easier as 1 unit = 1 standard deviation of the original unit.
- All of the above.

`@hint`
- Run `bmi_transforms_plot` and `cpd_transforms_plot` in the console to look at the distributions.

`@pre_exercise_code`
```{r}
transformed_framingham <- readRDS(url("https://assets.datacamp.com/production/repositories/2079/datasets/f6e38ca6a70fe7f5e38d234d11d42fd19603a37f/transformed_framingham.rds"))
library(tidyr)
library(dplyr)
library(ggplot2)

bmi_transforms_plot <- transformed_framingham %>% 
    select(contains("body_mass_index")) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")

cpd_transforms_plot <- transformed_framingham %>% 
    select(contains("cigarettes_per_day")) %>% 
    gather(variable, value) %>% 
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free")
```

`@sct`
```{r}
msg1 <- "Almost. While this is true, it's not the only true answer."
msg2 <- "Almost. While this is true, it's not the only true answer."
msg3 <- "Almost. While this is true, it's not the only true answer."
msg4 <- "Almost. While this is true, it's not the only true answer."
msg5 <- "Yes! Which type of and when you might transform really depends on the research question, the data values, and how you will want the results from your analyses to be interpreted. This means you need to carefully think about and have justifications for what you do to the data."
ex() %>% check_mc(5, feedback_msgs = c(msg1, msg2, msg3, msg4, msg5))
```
