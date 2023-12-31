Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

-   Making summary tables and graphs
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(here)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  How is tree diameter affected by the date planted?
2.  How does the road side affect tree growth (this is in
    `street_side_name`)?
3.  How is tree growth affected by distance to the coast?
4.  Given the above factors, which tree species grows the largest?
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

#### Question 1: age and diameter

To start, I’ll turn age into a categorical variable, splitting into
“young,” “middle” and “old,” to better understand the lifecycle of the
trees, first removing the rows where the date is not available.

``` r
trees_age <-
  vancouver_trees %>%
  filter(!is.na(date_planted)) %>%
  mutate(age_class = factor(case_when(
                               date_planted < "1990-01-01" ~ "very old",
                               date_planted <  "1995-01-01" ~ "old",
                               date_planted < "2005-01-01" ~ "middle",
                               date_planted < "2015-01-01" ~ "young",
                               .default = "very young"),c("very young", "young","middle","old", "very old")))

head(trees_age)
```

    ## # A tibble: 6 × 21
    ##   tree_id civic_number std_street    genus_name species_name cultivar_name  
    ##     <dbl>        <dbl> <chr>         <chr>      <chr>        <chr>          
    ## 1  149556          494 W 58TH AV     ULMUS      AMERICANA    BRANDON        
    ## 2  149563          450 W 58TH AV     ZELKOVA    SERRATA      <NA>           
    ## 3  149579         4994 WINDSOR ST    STYRAX     JAPONICA     <NA>           
    ## 4  149590          858 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## 5  149604         5032 WINDSOR ST    ACER       CAMPESTRE    <NA>           
    ## 6  149617         4909 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ## # ℹ 15 more variables: common_name <chr>, assigned <chr>, root_barrier <chr>,
    ## #   plant_area <chr>, on_street_block <dbl>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, height_range_id <dbl>,
    ## #   diameter <dbl>, curb <chr>, date_planted <date>, longitude <dbl>,
    ## #   latitude <dbl>, age_class <fct>

Now, for fun, we can get the counts of the ages:

``` r
trees_age %>%
  group_by(age_class) %>%
  summarize(count = n())
```

    ## # A tibble: 5 × 2
    ##   age_class  count
    ##   <fct>      <int>
    ## 1 very young  5606
    ## 2 young      26675
    ## 3 middle     29992
    ## 4 old         7490
    ## 5 very old     300

This will be helpful in the plotting page, but also tells as that most
trees are not very old, which lets us know that we can only understand
this relationship with high confidence for the earlier part of the tree
lifecycle.

Next, we can plot the age in our new categories against the diameter. We
will overlay a line, to make the trend clearer.

``` r
trees_age %>% 
  group_by(age_class) %>%
  summarize(mean_diameter = mean(diameter)) %>%
  ggplot(aes(x = age_class, y = mean_diameter)) +
  geom_col()+
  geom_line(aes(x = as.numeric(age_class)))+
  labs(x = "Tree age", y = "Tree diameter (m)")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

There is a shockingly linear relationship between age and diameter,
although it appears slower between “very young” and “young.” This is
likely because trees are not planted as saplings, so using “date
planted” to mean “age” is not quite accurate

### Question 2: Road side and tree diameter

For this one, we will compute the mean, range, standard deviation, and
max height for each road side.

``` r
road_side_summary <- vancouver_trees %>%
  filter(!is.na(diameter)) %>%
  filter(!is.na(street_side_name)) %>%
  group_by(street_side_name) %>%
  summarise(mean_diam = mean(diameter), diam_range = max(diameter) - min(diameter), diam_standard_dev = sd(diameter), count = n())
  
road_side_summary
```

    ## # A tibble: 6 × 5
    ##   street_side_name mean_diam diam_range diam_standard_dev count
    ##   <chr>                <dbl>      <dbl>             <dbl> <int>
    ## 1 BIKE MED              3.01        0.5            0.0811    38
    ## 2 EVEN                 11.5       305              9.13   71753
    ## 3 GREENWAY             16.4        25             10.6        5
    ## 4 MED                   8.08       98.5            9.03    3297
    ## 5 ODD                  11.7       435              9.27   71374
    ## 6 PARK                  3.19       27              2.25     144

This gives us a sense of what data we have available, and a course look
at the effect of interest.

Let’s visualize this graphically, plotting the mean, with the standard
deviation overlaid in red

``` r
road_side_summary %>% ggplot(aes(x = street_side_name))+
  geom_col(aes(y = mean_diam, fill = "Mean Diameter")) +
  geom_col(aes(y = diam_standard_dev, fill = "Standard Deviation in Diameter"), alpha = 0.5)+
  labs(x = "Street side", y = "Mean diameter/standard deviation")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

The standard deviation of the diameter is very large throughout, other
than the bike median category. This suggests that we need to use caution
and large sample sizes when working with this data.

### Question 3: Tree diameter and distance to coast

In a perfect world, I would have liked to have created a definition of
the coast line. It should be feasible to define the boundaries of tree
locations, and we know that the north and south boundaries are right
next to the water, as well as the area surrounding downtown. As a
first-pass approximation, I am going to simply use the longitude, as
this should roughly approximate the distance to the open ocean - we are
ignoring the effects of False Creek, the Fraser River, etc.

``` r
trees_long <-
  vancouver_trees %>%
  filter(!is.na(longitude)) %>%
  filter(!is.na(diameter)) %>%
  mutate(westness = factor(case_when(
                               longitude < -123.18 ~ "far west",
                               longitude <  -123.14 ~ "west",
                               longitude < -123.10 ~ "middle",
                               longitude < -123.06 ~ "east",
                               .default = "far east"),c("far west", "west","middle","east", "far east")))

head(trees_age)
```

    ## # A tibble: 6 × 21
    ##   tree_id civic_number std_street    genus_name species_name cultivar_name  
    ##     <dbl>        <dbl> <chr>         <chr>      <chr>        <chr>          
    ## 1  149556          494 W 58TH AV     ULMUS      AMERICANA    BRANDON        
    ## 2  149563          450 W 58TH AV     ZELKOVA    SERRATA      <NA>           
    ## 3  149579         4994 WINDSOR ST    STYRAX     JAPONICA     <NA>           
    ## 4  149590          858 E 39TH AV     FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## 5  149604         5032 WINDSOR ST    ACER       CAMPESTRE    <NA>           
    ## 6  149617         4909 SHERBROOKE ST ACER       PLATANOIDES  COLUMNARE      
    ## # ℹ 15 more variables: common_name <chr>, assigned <chr>, root_barrier <chr>,
    ## #   plant_area <chr>, on_street_block <dbl>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, height_range_id <dbl>,
    ## #   diameter <dbl>, curb <chr>, date_planted <date>, longitude <dbl>,
    ## #   latitude <dbl>, age_class <fct>

We can again check the counts to make sure this is reasonable.

``` r
trees_long %>%
  group_by(westness) %>%
  summarize(count = n())
```

    ## # A tibble: 5 × 2
    ##   westness count
    ##   <fct>    <int>
    ## 1 far west 10590
    ## 2 west     24618
    ## 3 middle   30270
    ## 4 east     30204
    ## 5 far east 28158

This again gives us a sense of the location of trees. We see that it
skews a little towards the middle of town. However, each bin is
sufficently large that we don’t need to worry about variance from small
sample sizes

``` r
trees_long %>% 
  group_by(westness) %>%
  summarize(mean_diameter = mean(diameter)) %>%
  ggplot(aes(x = westness, y = mean_diameter)) +
  geom_col()+
  geom_line(aes(x = as.numeric(westness)))+
  labs(x = "Distance west in Vancouver", y = "Tree diameter (m)")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

With a stronger effect than I expected, trees do appear to be shorter
the further east we go. However, it is entirely unclear whether this is
due to distance to the coast, or some other factor such as the relative
wealth, densities and ages of these neighbourhoods.

### Question 4

My intention with this question was to control for the effects I had
found above, but to start, let me just compare the diameters outright.
As well, to make it more manageable, I will look at the genus rather
than the species

First, let me summarize some key statistics of each genus.

``` r
genus_summary <- vancouver_trees %>%
  filter(!is.na(diameter)) %>%
  filter(!is.na(genus_name)) %>%
  group_by(genus_name) %>%
  summarise(mean = mean(diameter), range = max(diameter) - min(diameter), standard_dev = sd(diameter), count = n(), mean_height = mean(height_range_id))
  
head(genus_summary)
```

    ## # A tibble: 6 × 6
    ##   genus_name  mean range standard_dev count mean_height
    ##   <chr>      <dbl> <dbl>        <dbl> <int>       <dbl>
    ## 1 ABIES       12.9  41.5         9.71   190        3.21
    ## 2 ACER        10.6 317           8.76 36062        2.73
    ## 3 AESCULUS    23.7  64           9.57  2570        4.60
    ## 4 AILANTHUS   15.9  18.5         8.64     4        3.5 
    ## 5 ALBIZIA      6     0          NA        1        2   
    ## 6 ALNUS       17.5  40           8.94    74        3.99

Already, we can see that the diameter ranges substantially between
genera, and could by hand pick out a few trees that seem to grow well.

To get a better sense of this, I’ll plot the height and diameter of each
genus, with the size of the points scaled by the count of trees in that
genus. The points will need some transparency so we can see when they
overlap I’ll label the top few points on the plot.

``` r
genus_summary %>%
  ggplot(aes(x=mean, y= mean_height)) +
  geom_point(aes(size = count, color = genus_name), alpha = 0.5) +
  geom_text(aes(label = ifelse(mean > 22,as.character(genus_name),''), hjust = -0.1, vjust =0)) +
  labs(x = "Mean diameter (m)", y = "Mean height index") +
  scale_color_discrete(guide="none") +
  xlim(0,55)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Unfortunately, “castanea” and “salix” overlap, but otherwise, we can see
the tallest and widest few genera and get a sense of their scale
relative to other trees.
<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

I have made some progress towards answering all 4 of my research
questions, although my approach is fairly coarse and qualitative in all
cases. I know that trees grow taller roughly linearly as they age, and
that the further east you go, the smaller trees tend to (although, as
noted above, it’s not clear if this has much to do with the distance
from the shore). For a more meaningful analysis of both of these
questions, I really should be limiting myself to a specific species of
tree, and perhaps in a specific area.

For question 2, I see that even and odd sides of the road are very
similar, although really I was hoping to get at east/west/north/south
differences, which has not been elucidated from this example.

For question 4, I have a few genera that grow taller and wider on
average, but this did not include any controlling for other impacts, of
which there are many. For instance, it may be that the genera that
appear to “grow best” are just ones that happened to only have been
planted a long time ago, and have thus had lots of time to grow.

A more refined and realizable set of questions might be:

1.  What quantitative relationship exists between the date planted and
    tree diameter?
2.  How does the cardinal direction of a road affect the growth of trees
    lining the street? Can we predict this effect numerically?
3.  What relationship does geographic location have on the growth of
    given species of trees?
4.  For trees grown in similar conditions and planted at similar times,
    what species or genus grows to the largest diameter?
    <!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

This data is mostly tidy.

The goal is to study trees, each tree occupies a row, and can be seen as
one observation of a tree. Each column is a variable relating to that
tree. To discuss the first 8 columns: the tree’s ID is a unique
identifier. The civic number appears to correspond to a house number. In
untidy data, this might be combined with the following column, the
street name, however separating it like this is much nicer, as these are
really two separate variables, and this allows for analysis per street.
Likewise for genus and species. The cultivar is yet another independent
property. Arguably, the inclusion of the common name is untidy, as this
is entirely redundant with the genus and species. It would be better to
separately store a lookup table for this information. The “assigned”
column has an unclear meaning, which is also bad practice (although this
is not *necessarily* untidy).

Looking closely at a few further columns, on_street_block also contains
strictly less data than civic_number. on_street and std_street do appear
to vary from one another, which likely corresponds to two different
systems for representing the streets location. This is also potentially
confusing, a better option would be to just pick one system to prevent
anyone from accidentally using a different system than expected.

However, in terms of the overall structure, the data is tidy.

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

Although I mentioned a few non-ideal things, it would be pretty trivial
to remove and add those columns back, so I’ll untidy the data further.

To untidy it, I’ll do something that is truly a little insane, and
simply give each numerical variable its own row, such that each row is a
tree-numerical value combo

``` r
vancouver_trees_untidy <- vancouver_trees %>%
  pivot_longer(cols = where(is.numeric) & -tree_id, names_to = "Property", values_to = "Value") %>%
  relocate(c("Property", "Value"), .after = tree_id) #ensure the new columns are near the start

head(vancouver_trees_untidy, n= 15)
```

    ## # A tibble: 15 × 16
    ##    tree_id Property       Value std_street genus_name species_name cultivar_name
    ##      <dbl> <chr>          <dbl> <chr>      <chr>      <chr>        <chr>        
    ##  1  149556 civic_number   494   W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  2  149556 on_street_bl…  400   W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  3  149556 height_range…    2   W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  4  149556 diameter        10   W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  5  149556 longitude     -123.  W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  6  149556 latitude        49.2 W 58TH AV  ULMUS      AMERICANA    BRANDON      
    ##  7  149563 civic_number   450   W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ##  8  149563 on_street_bl…  400   W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ##  9  149563 height_range…    4   W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ## 10  149563 diameter        10   W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ## 11  149563 longitude     -123.  W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ## 12  149563 latitude        49.2 W 58TH AV  ZELKOVA    SERRATA      <NA>         
    ## 13  149579 civic_number  4994   WINDSOR ST STYRAX     JAPONICA     <NA>         
    ## 14  149579 on_street_bl… 4900   WINDSOR ST STYRAX     JAPONICA     <NA>         
    ## 15  149579 height_range…    3   WINDSOR ST STYRAX     JAPONICA     <NA>         
    ## # ℹ 9 more variables: common_name <chr>, assigned <chr>, root_barrier <chr>,
    ## #   plant_area <chr>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, curb <chr>, date_planted <date>

Now to clean it back up:

``` r
vancouver_trees_retidy <- vancouver_trees_untidy %>%
  pivot_wider(names_from = Property, values_from = Value) %>%
  relocate(where(is.numeric), .after = tree_id) #to illustrate

head(vancouver_trees_retidy)
```

    ## # A tibble: 6 × 20
    ##   tree_id civic_number on_street_block height_range_id diameter longitude
    ##     <dbl>        <dbl>           <dbl>           <dbl>    <dbl>     <dbl>
    ## 1  149556          494             400               2       10     -123.
    ## 2  149563          450             400               4       10     -123.
    ## 3  149579         4994            4900               3        4     -123.
    ## 4  149590          858             800               4       18     -123.
    ## 5  149604         5032            5000               2        9     -123.
    ## 6  149616          585             500               2        5     -123.
    ## # ℹ 14 more variables: latitude <dbl>, std_street <chr>, genus_name <chr>,
    ## #   species_name <chr>, cultivar_name <chr>, common_name <chr>, assigned <chr>,
    ## #   root_barrier <chr>, plant_area <chr>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, curb <chr>,
    ## #   date_planted <date>

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  What quantitative relationship exists between the date planted and
    tree diameter? Can we distinguish or control for the age of a tree
    when it was planted?
2.  How does the cardinal direction of a road affect the growth of trees
    lining the street? Can we predict this effect numerically?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

I felt that these two questions were some of the most practical to
answer, without pulling in an abundance of outside data. As well, both
seemed to have some promising early results that we could dig deeper
into.

It also appeals to me that the answers to both of these questions have
practical implications for the purposes of city planning.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

I’m going to do some basic clean up that will help with both analyses,
then eventually split into two, one tibble for each

``` r
vancouver_trees_clean <- vancouver_trees %>%
  select(c(tree_id, std_street, genus_name, species_name, street_side_name, diameter,
           date_planted, longitude, latitude)) %>%
  na.exclude() %>% #remove rows with NAs
  mutate(nominal_age = date("2023-10-24") - date_planted) %>% #use the more direct variable of age
  relocate(nominal_age, .after = tree_id) %>% #Rearrange the date to put the interesting start at the front
  relocate(diameter, .after = nominal_age) %>%
  select(-date_planted) #get rid of the residual variable
head(vancouver_trees_clean)
```

    ## # A tibble: 6 × 9
    ##   tree_id nominal_age diameter std_street    genus_name species_name
    ##     <dbl> <drtn>         <dbl> <chr>         <chr>      <chr>       
    ## 1  149556  9050 days        10 W 58TH AV     ULMUS      AMERICANA   
    ## 2  149563 10007 days        10 W 58TH AV     ZELKOVA    SERRATA     
    ## 3  149579 10928 days         4 WINDSOR ST    STYRAX     JAPONICA    
    ## 4  149590 10039 days        18 E 39TH AV     FRAXINUS   AMERICANA   
    ## 5  149604 10903 days         9 WINDSOR ST    ACER       CAMPESTRE   
    ## 6  149617 10904 days        15 SHERBROOKE ST ACER       PLATANOIDES 
    ## # ℹ 3 more variables: street_side_name <chr>, longitude <dbl>, latitude <dbl>

Now I’ll make a version for my question 1, which only requires removing
a few unnecessary variables. Realistically, we would also eventually
want to filter to one specific species, but some more work is required
to pick a species that has a large number of species planted over a wide
range of dates.

``` r
vancouver_trees_q1 <- vancouver_trees_clean %>%
  select(c(tree_id, nominal_age, diameter, longitude, latitude))
```

For question 2, I want to create a categorical variable that says
whether a tree is on the north, south, east or west side of the street.
Luckily, in Vancouver, the odd side of the road is north on an avenue
and west on a street, and the opposite for the even. We can work from
this.

``` r
vancouver_trees_q2 <- vancouver_trees_clean %>%
  filter(street_side_name %in% c("EVEN", "ODD")) %>% #We can only work with even or odd
  filter(str_ends(std_street, "AV") | str_ends(std_street, "ST")) %>%
  mutate(street_side = case_when(
    str_ends(std_street, "AV") & street_side_name == "ODD" ~ "north",
    str_ends(std_street, "AV") & street_side_name == "EVEN" ~ "south",
    str_ends(std_street, "ST") & street_side_name == "ODD" ~ "east",
    str_ends(std_street, "ST") & street_side_name == "EVEN" ~ "west",
  )) %>%
  relocate(street_side, .after = tree_id)

head(vancouver_trees_q2)
```

    ## # A tibble: 6 × 10
    ##   tree_id street_side nominal_age diameter std_street    genus_name species_name
    ##     <dbl> <chr>       <drtn>         <dbl> <chr>         <chr>      <chr>       
    ## 1  149556 south        9050 days        10 W 58TH AV     ULMUS      AMERICANA   
    ## 2  149563 south       10007 days        10 W 58TH AV     ZELKOVA    SERRATA     
    ## 3  149579 west        10928 days         4 WINDSOR ST    STYRAX     JAPONICA    
    ## 4  149590 south       10039 days        18 E 39TH AV     FRAXINUS   AMERICANA   
    ## 5  149604 west        10903 days         9 WINDSOR ST    ACER       CAMPESTRE   
    ## 6  149617 east        10904 days        15 SHERBROOKE ST ACER       PLATANOIDES 
    ## # ℹ 3 more variables: street_side_name <chr>, longitude <dbl>, latitude <dbl>

Of course, this won’t be perfect, because not all streets and avenues
are perfectly parallel, and I can’t guarantee that the naming and
numbering scheme are 100% consistent, but this should split things up
reasonably

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: What quantitative relationship exists between the
date planted and tree diameter?

**Variable of interest**: Tree diameter

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression
        coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
diam_v_age <- lm(diameter ~ nominal_age, vancouver_trees_q2)

diam_v_age
```

    ## 
    ## Call:
    ## lm(formula = diameter ~ nominal_age, data = vancouver_trees_q2)
    ## 
    ## Coefficients:
    ## (Intercept)  nominal_age  
    ##  -0.3414655    0.0008618

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I will take a look at the p value, to see how well these correlate

``` r
lm_tidy <- broom::tidy(diam_v_age)

print(lm_tidy %>% select(c(term, p.value)))
```

    ## # A tibble: 2 × 2
    ##   term             p.value
    ##   <chr>              <dbl>
    ## 1 (Intercept) 0.0000000126
    ## 2 nominal_age 0

Here, the value I’m mainly interested is the bottom right of this table.
In fact, it would appear that the association is so significant, that
the p value has rounded all the way down to zero. This perhaps isn’t too
surprising given what we saw in section 1.2!

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
if(!dir.exists(here("output"))){
  dir.create(here("output"))
}
write_csv(road_side_summary, here("output", "road_size_summary.csv"))
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 4.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
if(!dir.exists(here("output"))){
  dir.create(here("output"))
}
rds_path = here("output", "diam_v_age.rds")

saveRDS(diam_v_age, rds_path)

new_diam_v_age <- readRDS(rds_path)

new_diam_v_age
```

    ## 
    ## Call:
    ## lm(formula = diameter ~ nominal_age, data = vancouver_trees_q2)
    ## 
    ## Coefficients:
    ## (Intercept)  nominal_age  
    ##  -0.3414655    0.0008618

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output md files.
-   All knitted md files are viewable without errors on Github. Examples
    of errors: Missing plots, “Sorry about that, but we can’t show files
    that are this big right now” messages, error messages from broken R
    code
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
