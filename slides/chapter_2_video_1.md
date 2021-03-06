---
title: Exploring before wrangling
key: 146b85090bb8ab77efbfe45c5c751f5d

---
## Pre-wrangling exploration

```yaml
type: "TitleSlide"
key: "f836c98410"
```

`@lower_third`

name: Luke Johnston
title: Diabetes epidemiologist


`@script`
We've gone over the basics of cohorts and some initial explorations. In this chapter we'll cover common wrangling tasks and visually exploring cohort data.


---
## Univariate ("one variable") visualizations

```yaml
type: "TwoColumns"
key: "09f0b70d13"
```

`@part1`
```{r}
library(ggplot2)
# Histogram of body mass
ggplot(tidier_framingham,
	   # Aesthetic mapping
       aes(x = body_mass_index)) +
    geom_histogram()
```
{{1}}


`@part2`
![histogram](https://assets.datacamp.com/production/repositories/2079/datasets/adcd763375b8bcba52f68c14e34c7ef2c6c2cd33/ch2-v1-histogram.png) {{1}}


`@script`
A useful way to look at data is with univariate distributions using histograms, which count the number of times a given value occurs. We'll create histograms using ggplot2. We first set up the plot using the ggplot function. This takes two arguments, the dataset and the aesthetic mapping or aes function. Aes indicates where to map variables to plot aspects like the x-axis. We want a univariate plot, so we'll set x to a continuous variable like body mass. The histogram layer is added with the plus sign followed by geom-underscore-histogram.


---
## Visualize many variables with "longer" data

```yaml
type: "FullSlide"
key: "6f3273e660"
```

`@part1`
```{r}
library(dplyr)
wide_form <- tidier_framingham %>%
    select(subject_id,
           body_mass_index,
           participant_age)
head(wide_form, 4)
```
{{1}}

```
# A tibble: 4 x 3
  subject_id body_mass_index participant_age
       <dbl>           <dbl>           <dbl>
1       2448            27.0              39
2       2448            NA                52
3       6238            28.7              46
4       6238            29.4              52
```
{{2}}


`@script`
If we want to visualize several variables, plotting them one by one is less than ideal. There's an easier way though, by using ggplot facets. We'll go over facets soon, but to optimally use facets we'll need the data in the long form.

Our data right now is in wide form. To illustrate, I'm only using a couple variables. Wider data has variables as columns and those variables' values are the individual rows.


---
## Convert to "longer" form using gather from tidyr

```yaml
type: "FullSlide"
key: "71c8f11816"
```

`@part1`
```{r}
library(tidyr)
long_form <- wide_form %>%
    head(4) %>%
    gather(variable, value, -subject_id)
long_form
```
{{1}}

```
# A tibble: 8 x 3
  subject_id variable        value
       <dbl> <chr>           <dbl>
1       2448 body_mass_index  27.0
2       2448 body_mass_index  NA  
3       6238 body_mass_index  28.7
4       6238 body_mass_index  29.4
5       2448 participant_age  39  
6       2448 participant_age  52  
7       6238 participant_age  46  
8       6238 participant_age  52  
``` 
{{2}}


`@script`
We use the gather function from tidyr to convert to long form. For illustration I'll only show four rows using head. Gather takes four arguments: data, key, value, and the variable selection. Data is already given by the pipe. The key is the name to give the new column that will contain the original variable names. The value is the name to give the new column that will contain the variable values. We'll simply call the new columns variable and value. The last optional argument is the one or more variables we want to include or exclude from gathering. Here we want to exclude subject ID by using minus.

Longer data is in more of a stacked form, with variable names as data in a column here called variable and their associated value in a new column, again, here called value.


---
## Visually explore multiple variables

```yaml
type: "FullSlide"
key: "1355d59abd"
```

`@part1`
```{r}
wide_form %>%
    gather(variable, value, -subject_id) %>%
    ggplot(aes(x = value)) +
    geom_histogram() +
    facet_wrap(vars(variable), scale = "free", nrow = 1)
``` 
{{1}}

![facetted histograms](http://assets.datacamp.com/production/repositories/2079/datasets/2acc30ca29c22a921db27611193a3a0363d075fa/ch2-v1-two-histograms.png) {{1}}


`@script`
Let's now use this long data by piping it into ggplot. We then set x to the value column with the original data. Then we add the histogram geom and to facet use the facet-underscore-wrap function. This function takes several arguments, but the first is the variables you want to facet. The one or more variables to facet need to be wrapped in the vars function. We're setting scale to free as the variables' values have different ranges. Setting nrow to one puts all the facets in one row.

Now we have a single plot with multiple histograms! You can very quickly explore more of the data. Longer data can also be leveraged for many other analysis tasks too, which we'll cover in later chapters.


---
## Visually explore the exposure by outcome

```yaml
type: "TwoColumns"
key: "b4c84874a5"
```

`@part1`
```{r}
tidier_framingham %>%
    mutate(got_cvd = 
    	as.character(got_cvd)) %>%
    ggplot(aes(
    	x = got_cvd,
		y = body_mass_index)) +
    geom_boxplot()
```
{{1}}

- `coord_flip()` for horizontal boxplots {{2}}


`@part2`
![boxplot](http://assets.datacamp.com/production/repositories/2079/datasets/1a49396f8978317db2445c06cebd1ebdf75f6223/ch2-v1-boxplot.png) {{1}}


`@script`
Univariate visualizations are great but let's explore how the exposures look in relation to the outcome. For categorical outcomes like CVD, boxplots are great for showing the data's distribution using the median, the twenty-fifth and seventy-fifth percentile, and a measure of slightly more extreme values.

Since CVD's values are either zero or one, we'll need to convert it to categorical with mutate and as-dot-character. Then we set x to be CVD and y needs to be a continuous variable. Add the geom-underscore-boxplot layer and we get a boxplot figure!

To make the boxplots horizontal instead of vertical, you'll need to use coord-underscore-flip.


---
## Exploring time!

```yaml
type: "FinalSlide"
key: "6b31cf3945"
```

`@script`
Alright, let's visualize some variables!

