Here I collect what I learned during the optional swirl lessons
and did not fit in the other files.

## File management

 * `list.files()` -- `ls`.

   *Fun fact:* `ls()` lists variables defined in the environment.
 * `(file|dir).create()`, `file.copy()`, `file.rename()`, `file.remove()`
    resp. `unlink()`, `(file|dir).exists()`;  
    `touch`, `mkdir`, `cp`, `mv`, `rm` resp. `rmdir`/`unlink`,
    and checks as expected.
 * `file.info()` -- detailed information, similar to what `ls -l` gives.
 * `file.path()` -- path of file relative to current working directory.

    *Note:* This can be used to construct filenames in a platform-independent
    fashion!
    For instance, `file.path("folder1", "folder2", "file")` returns
    a representation of `folder1/folder2/file` appropriate for the current
    system.
 * `Sys.chmod()` -- `chmod`

## Sequences

 * Create basic sequences by `a:b`; non-integer bounds work!
 * For more control, use `seq()`; useful parameters include
   `by` and `length`. Maybe `along.with`.
 * `length()` gives the number of elements in a sequence
    (which is a vector, after all).
 * For repetitions of the same number, use `rep()`.
   Prominent parameters: `times`, `each`.

## Fun with logical values

* `x == NA` gives `NA` -- use `is.na()` to check if some value is `NA`.
* `&&` and `||` differ from `&` and `|` in that they will only check the
  first components of participating vectors, no matter the result.
* Use `isTRUE()` to check booleans if they could be `NA`; evaluating
  `if ( NA) ...` will only get you errors.
* `which()` gives the indices at which the given logical vector is `TRUE`.
  `any()` and `all()` are vector-or and vector-and.

## Looking at data

 * `class()`
 * Data frames: `dim()`, `nrow()`, `ncol()` for dimensions;
   `names()` for a vector of column names.
 * `object.size()`
 * `(head|tail)(x, n)` gives the first|last `n` rows of `x`.
 * `summary()` gives a gross overview over the given data.
 * `table()` shows the occurrence counts of values.
 * `str()` summarizes the structure of data in readable form.

## Random Sampling

 * `sample()` samples from a fixed collection of values.
 * Distribution-specific functions are `r[name]()` (random sample),
   `d[name]()` (density function), `p[name]()` (cumulative distribution
   function), and `q[name]()` (quantiles; the inverse of the CDF).

    Distributions include `binom`, `norm`, `pois`, `exp`, `chisq`,
    `gamma`, ... They take parameters as you would expect from their
    resp. mathematical formulations/parameterizations.
 * `pX()` and `dX()` can do lower and upper tail.
 * For reproducibility, set the seed with `set.seed()`.
 * Use `replicate` for repeating a sampling multiple times (independently).

## Plotting

 * `plot()` creates scatterplots. Note the formula interface.
   Many more options beyond `?plot`; see e.g. `?par`, `?points`.
 * `boxplot()` creates box-and-whisker plots.
 * `hist()` creates histograms.
 * More advanced: `lattice`, `ggplot2`, `ggvis`.

## Assorted

 * `par()` gives lots of graphical parameters.
 * `rnorm()` samples random numbers from a normal distribution.
 * `runif()` samples random numbers from a uniform distribution.
 * `sample()` samples uniformly from vectors.
 * `set.seed()` sets the seed of the used PRNG -- use for reproducibility!
 * > `table()` uses the cross-classifying factors to build a contingency
   >  table of the counts at each combination of factor levels.
 * `sessionInfo()` and `devtools::session_info()`
