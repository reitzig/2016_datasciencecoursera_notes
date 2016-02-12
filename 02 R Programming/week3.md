## Loop functions

Loop functions (`*apply`) implement the
  "[split-apply-combine](http://econpapers.repec.org/RePEc:jss:jstsof:40:i01)"
strategy.

*Reference:* [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets.html)

*Note to self:* Is this just `map` in functional programming,
or "map-reduce" in more "modern" terms?

### `lapply`

**Input:** A list `[l1,...,ln]` and a function `f`.  
**Output:** A list of the same length: `[f(l1), ..., f(ln)]`.

Remarks:

 * Inputs that are not a list are coerced into one;
   you get an error if that is not possible.
 * You pass either an anonymous function, or the *name* of a function
   (no parentheses).
 * Names for elements in the input carry over to the output.
 * Since a data frame is a list of vectors, pass a data frame to apply
   a function to all its columns.
 * Additional (named) parameters are passed (via `...`) to every call of `f`.
   Example:

    ```R
    lapply(1:4, runif, min=0, max=10)
    ```

### `sapply`

Calls `lapply` and then attempts simplification (to a vector).

 * If every element is of length one, the output is a vector.
 * If every element has the same amount of elements $>1$, the output is a matrix.
 * Else, the list is returned without simplification.

`sapply(x, f, simplify = FALSE, USE.NAMES = FALSE)`
is the same as `lapply(x, f)`.

### `mapply`

The multivariate version of `lapply`/`sapply`, i.e. the function takes
multiple parameters taken from different lists.
Example:

```R
mapply(rep, 1:4, 4:1)
mapply(rnorm, mean=1:5, sd=1:5, 2)
```

Notes:
 * Naming the input lists matches their elements with the correspondingly
   named parameters of the given function. Additional parameters can still
   match to unnamed parameters, according to the matching rules for function
   calls.

*Note to self:* Can't we just combine the lists and then use `lapply`?

### `vapply`

Similar to `sapply` but allows you to specify the target type!
If the list resulting from `lapply` does not match the target, an error is
thrown. That makes for safer scripts!

*Note:* Fix the dimension of the result elements by `basetype(dim)`.

### `tapply`

Applies functions over subsets of a given vector; returns an array.
That is, when you want to split your data into groups according to some variable
before applying a function, you can use `tapply`:
`tapply(data, index, fun)` applies `fun` to the subset(s) distinguished
by factor(s) `index`.
Example:

```R
tapply(1:10, 1:10 %% 2 == 0, sum)

x <- c(rnorm(10), runif(10), rnorm(10,2))
levels(x) <- factor(c(rep("norm", 10), rep("unif", 10), rep("norm2",10)))
tapply(x, levels(x), mean)
tapply(x, levels(x), range)
```

You can also pass a list/vector of factors/indices.

Notes:

 * `tapply(df$col1, df$col2, f)` groups/factors the rows
   in `df` by different values in `df$col2`, and applied `f` to each of the
   resulting subvectors of `df$col1`.
 * `tapply` tries to simplify like `sapply` by default; can override.

### `apply`

Apply a function over the margins of an array (read: vector with dimension),
e.g. the columns or rows of a matrix. Example:

```R
column_means <- apply(matrix, 2, mean)
row_means <- apply(matrix, 1, mean)
```

That is, the second parameter are the dimensions that are to be kept.
Note the differences of the calls in the following example:

```R
a <- array(rnorm(40), c(2,2,10))
apply(a, c(1), mean)
apply(a, c(2), mean)
apply(a, c(3), mean)
apply(a, c(1,2), mean)
apply(a, c(2,3), mean)
apply(a, c(1,3), mean)
apply(a, c(1,2,3), mean)
```

`apply` creates a matrix if the result of the function is a vector.
Example:

```R
x <- matrix(rnorm(200), 20, 10)
apply(x, 1, quantile, probs = c(0.25,0.75))
```

The result has one column per for, and each column contains the two quantiles
computed from the corresponding row.

*Note:* There are (more efficient) shortcuts for often used combinations,
e.g. `(row|col)(Sums|Means)`.

### `split`

Not a looping function, but often useful in combination with those.
Essentially, it does what `tapply` does before doing `lapply`:
split a vector according to the (levels of the) given factor:
`lapply(split(x, factor), fun)` is essentially the same as
`tapply(x, factor, fun)`.

Notable option: `drop = TRUE` drops empty levels.

*Note to self:* One should explain `lapply`, then `split`, and *then* present
`tapply` as a combination.

## Debugging

Pre-defined *conditions*:

 * `message` produces notification messages.
 * `warning` generates warning (unexpected, but not fatal)
   *after* the function completes.

   *Example:* `log(-1)`
 * `stop` generates an error message; aborts execution.

   *Example:* `if ( NA > 1 ) ...`

You define your own conditions, i.e. messages of some type.

Basic rule of debugging:
compare expectation to output;
compare expectation to documentation;
reproduce;
narrow down to MWE.

### Tools

 * `traceback()` -- prints stack trace if there was an error.
                    You can call this from the prompt immediately after seeing
                    the error.
 * `debug()` -- enters debug/browsing mode for this function; can step through.
 * `browser()` -- suspends at call site and enters debug mode.

    *Note to self:* Is this what IDEs call a break point?
 * `trace()` -- inserts debugging code without entering the function.
 * `recover()` -- allows you to define a new error handler.
                  Execution stops at an error, gives you the current call stack
                  and dig around in it.
 * And, of course: print-line debugging. D'uh.

Setting `options(error = recover)` puts you into recovery mode right when
and where an error occurs.

In browsing mode, enter `help` to see the commands.
