## Cleaning Data

### Subsetting and Sorting

See [R Programming, week 2](../02 R Programming/week1.md).

Pointers: library `plyr`.


### Summarizing Data

For inspecting data, use `head()`/`tail()`, `summary()`, `str()`, `quantile()`.
For the size of an object in memory, use `print(object.size(o), units="??")`.

Functions `table()` with `useNA`, `sum()`, `any()`, `all()`, `colSums()`
and combinations thereof are useful for sanity checks.

User `xtabs()` to create cross-tables; `ftable()` can flatten the result if it
has more than two dimensions.


### Creating New Variables

You may want new variables for things like, for example, indicating missing
or wrong values or factor versions of quantitative variables.

 * For creating *indices* (unique identifiers), use `seq()`.
 * For breaking up variables into quantiles, use `cut()` or 
    `cut2()` from library `Hmisc`.
 * Transform variables into factors with `factor()`.
 * `mutate()` from library `plyr()` creates copies with the updates.

### Reshaping Data

Library `reshape2` provides several functions.

 * `melt()` separates identifiers and measurements ("variable").
 * `dcast()` can then cast/organize the resulting data frames 
    into different forms. 
    
    Similar, for arrays: `acast()`.
    
Library `plyr` also has some interesting things; for instance, 
`ddply` does something similar to `tapply()`.

*Honorable mention:* `arrange` is faster than `order()`.

### Managing Data Frames with dplyr

*Assumption:* Tidy Data in data frames (and other types) 
that are fully annotated.

An improved version of library `plyr`, `dplyr` simplifies existing functionality
of R and makes it more efficient.

Different implementations for representing data frames exist; 
default is standard R.

Common format: parameters are data frame + actions; refer to columns directly by their name. Result is a (new) data frame.

 * `tbl_df()` -- wraps a regular data frame for nicer printing; you only
    get as much as fits the terminal.
    
    Use `View()` for a complete, formatted view.

 * **select** -- extract subsets of columns
 
    * `select(df, colD, colA, colC)` -- selects the specified columns and puts
       them in the given order.
    * `select(df, colA:colD)` -- selects columns `colA` through `colB` in `df`.
    * `select(df, -(colA:colD))` -- selects all columns *but* 
       `colA` through `colB` in `df`.
       
    Check the documentation of `select` for special predicates/functions.
       
 * **filter** -- extract subsets of rows
    
    * `filter(cf, colA > 10)`
    * `filter(cf, colA > 10 & coldB < 20)`
    
 * **arrange** -- reorder rows
 
    * `arrange(df, colA)` -- sorts `df` by the values in `colA` 
        in ascending order.
    * `arrange(df, desc(colA))` -- sorts `df` by the values in `colA`
        in descending order.
    * `arrange(df, colA, colB)` -- sorts first by `colA`, then by `colB`.
    
 * **rename** -- change variable/column names
 
    * `rename(df, newName = colA, coolname = colB)`
    
 * **mutate** -- add new variables/columns or transform existing ones
 
    * `mutate(df, colA_centered = colA - mean(colA, na.rm = TRUE))` --
       introduces a new column according to the provided rule.
    * `mutate(df, myCategory = factor(1 * (colA > 10), labels = c("low", "high")))`
       -- introduces a new factor variable according to the specified rule,
          with labels!
    * `mutate(df, x = ..., y = 2^x)` -- variable definitions can use
       new variables defined "earlier".
       
 * **group_by** -- arranges rows according to values of variables
    or factors. It amounts to a "behind-the-scenes split".
 
    * `group_by(df, colA)`
    * `group_by(df, myCategory)`
       
 * **summarize** -- generate summary statistics
 
    * `summarize(df, meanA = mean(colA)`
    * `summarize(groupedDf, meanA = mean(colA), maxB = max(colB), medC = median(colC))`
      -- creates a new data frame with one row of summary statistics per group.
      
      Add `na.rm = TRUE` to `mean()` et al. to ignore NAs.


*Special feature:* pipeline operator `%>%` allows to skip temporary variables.
Example:

```R
df %>% mutate(myCategory = ...)
   %>% group_by(myCategory)
   %>% summarize(colA = mean(colA), ...)
```

Other backends include `data.table` (for fast, large tables) and 
relational databases (via SQL interface of the DBI package).

Read more on `dplyr` [here](https://github.com/hadley/dplyr),
and on its precursor `plyr` [here](http://plyr.had.co.nz/).


### Merging Data

You may want to *merge* several tables into one; this is what (plain) *joins* 
do in relational database. 

Use `merge()`, e.g. like so: `merge(x, y, by.x="nameX", by.y="nameY", all=T)`.
Parameter `all` controls what should happen with rows in one data frame 
that do not have matches in the other.  
Default is to merge w.r.t. all common column names.

Use `join()` from `dplyr`, e.g. like so: `join(x, y)`. Scales more nicely to
more than two data frames:

```R
dfList = list(df1, df2, df3)
join_all(dfList)
```

There is also `bind_rows()` in `dplyr` for merging two tables that contain
observations of the same observational unit.


### Tidying Data

Use package `tidyr`.

 * **gather** -- collapses columns into key-value pairs of header and contained 
    value. 
    Use to get rid of columns that are not variables.
    
 * **separate** -- splits columns. 
    Use to get rid of columns that contain more than one variable.
 
 * **spread** -- expand a key-value column into multiple columns.
    Use to get rid of observations spread across multiple rows.
    
 * **extract_numeric** -- use to clean up numerics.
