## Control Structures

 * `if`, `else`, `while`, `break`, `return` -- business as usual.
    Syntax is just like in Java.

    `if` can be used as expression (just like in Scala or Ruby), so

    ```R
    x <- if ( P ) { 1 } else { 2 }
    ```

    works.

 * `for` loops are written in bash-style:

    ```R
    for ( i in S ) { print(i) }
    ```

    where `S` is a sequence or vector.

    Use `seq_along(x)`to get the indices of a vector or sequence
    (which you can again iterate over).  
    `seq_len(x)` may also be useful in this context; it's much the same
    as `1:x`.

    **Note:** the loop variable does *not* remain local!
    After the loop is done with, the loop variable remains visible with
    the value of the last iteration *that got executed*.

 * `next` -- skips to the next iteration of a loop (cf. `continue` in Java).
 * `repeat { ... }` -- infinite loop; terminate with `break`.

*Note:* The above are mostly useful in scripts. For interactive sessions,
  there are the `*apply` functions. More later.

## Functions

### Defining Functions

 * Define (anonymous) functions by `function(params) { ... }`.
 * Since functions are values, you "name" them just like any other value:
    `foo <- function(...) {...}`.

 * Using special names of the form `"%name%"` you can define binary/infix
   operators.
 * Default values are specified as usual, e.g.
    `function(x, y=10) { ... }`.

   You can use `NULL` as default value if there is no good default value.
 * Last expression is return value; or whatever we `return` explicitly.

   Return `invisible(x)` to prevent auto-printing.
 * There is special argument name `...` which collects an arbitrary number of
   arguments. So, for instance:

   ```R
   mymean <- function(v, ...) {
     mean(v[c(...)])
   }
   ```

   computes the mean of any subset of the specified vector, e.g.
   `mymean(1:5, 1,3,5)` returns `3`.

   This is of particular use for defining *generic* functions; more later.

   *Note:* `...` does not have to appear at the end of the argument list!
     Positional and partial matching will be turned off after `...`, but
     in combination with named arguments `...` can occur anywhere.
     Unnamed or partially named parameters will simply be swallowed by `...`.
     Check out `paste` and `cat`, for instance.

    Unpack `...` by `list(...)`; parameter names carry over.

### Using functions

 * All arguments are *named*.
 * Specified parameters are matched to formal arguments by position and/or name.
   Unnamed parameters are assigned left-to-right to so far unmatched arguments.

   Weird things work; best stick to names unless specifying a prefix
   of all arguments (by position).
 * Partial matching (cf. column indices) works for argument names;
    priorities are (in decreasing order) exact match, partial match, position.
 * Get a list of (formal) arguments with `formals(f)`.
   This literally gives you a `list`; for human-readable information,
   use `args(f)`.
 * Function parameters are accessed/evaluated *lazily*.
 * As a consequence, Arguments can be *missing*. So, for instance the function

    ```R
    f <- function(a,b) { if (a < 5) { b } else { a+1 }}
    ```

    can be called as `f(6)` -- parameter `b` is not needed so we don't get
    punished for its absence. `f(2)` will fail, though, because `b` is needed
    but there is no default.

*Notes to self:*

 * Get the source code of any function by typing its name (like a variable).
   `args()` and `?` get more structured information.
 * In code that is to be read by others,
   use named arguments for function calls whenever there can be *any* unclarity.
 * Avoid using missing parameters as a feature. Specify defaults if you want
   calls to "legally" leave out parameters.
 * Don't use partial matches! We have tab-completion.


## Scoping

### Namespaces

 * When trying to bind a value, R searches *environments*.
   Run `searches()` to see which and in which order.
 * The order can be defined (by the user).
 * Load packages with `library` to get it on the list (at position 2, so
   packages loaded later take precedence).
 * Defining something in the interactive prompt puts in in the `.GlobalEnv`
    environment (user workspace) -- the first one searched, so these take precedence.
 * Functions and values have different namespaces.

    *Note to self:* Apparently, functions are not *completely* first-class
    objects?

### Scoping Rules

 * R uses *static/lexical* scoping.
 * There are function arguments, local variables and *free* variables.
 * Values for free variables are searched for in the environment
   in which the function was *defined* (not the one it was called in!).

   A function together with its environment is a *(function) closure*.
 * Environments are collections of symbol-value pairs.
 * There is a tree of environments, usually rooted with the global environment
   or package environment.
 * Values are searched from the function's parent environment upwards
   towards the root, then down the search list.
 * Changes of the values in the resp. environment between function calls
   *will be reflected in future calls*.
   Also, if the variable is newly defined in a higher-priority environment,
   it will take precedence in future calls!

   *Question:* Okay, so we do they call it *static* binding?  
   *Answer:* Because *dynamic* scoping searches from the call environment
     (called *parent frame* in R).  
   *Note to self:* Better stick with *lexical* scoping.


 * Things start to be interesting when functions return functions
   that have free variables that bind to values defined in the body of
   the outer function!

   Because then calling environment is not the same as the
   parent/definition environment.

   *Note to self:* Parent environment != parent frame. Good job.
 * Access the environment of a function with `environment(f)`;
   list the variables with `ls(environment(f))`.
   Access their values with `get("name", environment(f))`.

**Consequences**

 * All objects have to be in memory, otherwise it's hard to maintain the
    (inter-)linked structure.
 * All functions must carry a pointer to their definition environment
    (which we have to keep around!).

### Application Example: Optimization

 * Functions like e.g. `optim`, `nlm`, `optimize` take function parameters!
   These can depend on all kinds of things, with all kinds of names
   -- so we would not want some variable of the same name inside the
   implementation of, say, `optim` overshadow the value we set outside!
 * *Pattern:* Constructor function

    ```R
    constructor <- function(data, fixed=c(FALSE, ..., FALSE)) {
      params <- fixed
      function(p) {
        params[!fixed] <- p
        ... do stuff ...
      }
    }
    ```

    This guy now creates functions (to be optimized) that depend on the
    data and may have some of the model values fixed; the others get to be
    parameters.

    For instance, `constructor(data, c(0, FALSE))` creates a function that
    takes one parameter; `params` will end up being `c(0, p)`.
 * This can be used to optimize one parameter of the model with the others
   fixed, or plotting the values of the function in one parameter with the
   others fixed -- all using the same constructor function, i.e. you
   implement the model only once using *all* parameters, and decide at
   call-site which parameters to fix to which values.
 * Also, all the used data is "carried with" the thus constructed function.
   This saves parameters.
 * We get cleaner code -- yay!

*Reference:* [Lexical Scope and Statistical Computing](http://dx.doi.org/10.1080/10618600.2000.10474895) by R. Gentleman and R. Ihaka (2000)

---

*Note to self:* Yes, the column-means example function is unnecessarily ugly:

```R
sapply(1:ncol(mat), function(i) { mean(mat[,i]) })
```

*Note:* `sapply` may give you lists when you don't want them. `unlist()` to
the rescue!

Now if only there was nicer syntax for anonymous functions...

---

## Dates and times

 * `Date` for dates (#days since 1970-1-1),
   `POSIXct` (just an integer) and `POSIXlt` (a list with all kinds of information)
    for times (#seconds since 1970-1-1).
 * Create dates with `as.Date("YYYY-mm-dd")`.
 * `Sys.date` and `Sys.time()` get you the current date and time (as `POSIXct`),
    respectively.
 * Use `strptime()` to create times from (almost) arbitrary strings;
   it's `printf`-style, see `?strptime` for details.
 * `as.POSIXlt()` gets you the list version of a time, `as.POSIXct()` the int one.
 * Get internal representation by `unclass()`.
   `str(unclass())` can give a more readable output.
 * Access details with e.g. `weekdays()`, `months()`, `quarters()`.
 * You can use some operators with dates/times, e.g. `+`, `-`, `==`, `<=`.
 * Differences can also be computed using `difftime()` which allows to
   set the unit of the result.
 * Plotting functions treat dates/times specially.
 * When using dates often, package `lubridate` may be worth checking out.

## Strings

 * `sprintf` is a neat way to construct strings from multiple values.
   It works just as in other languages and seems to be the only feasible
   way of dealing with strings.
 * `paste()` is what other languages call `join`.

   Distinguish parameters `collapse` and `sep` when passing multiple vectors!
   `sep` controls with what to separate components, `collapse` if and how
   the resulting vector is flattened. For instance:

   ```R
   > paste(1:3, c(4:6), sep=" ", collapse=" -- ")
   [1] "1 4 -- 2 5 -- 3 6"
   > paste(1:3, c(4:6), sep=" ")
   [1] "1 4" "2 5" "3 6"
   ```
 * Nice to know: `LETTERS` and `letters` contain `A..Z` and `a..z`,
   respectively.

## Getting all file names in a directory

*Note to self:* It's probably better to have a regular naming scheme and
compute file names, as opposed to trying to make sense out of haphazardly
named files programmatically.

Still, from [here](https://www.coursera.org/learn/r-programming/discussions/mTtS2Lm8EeWfsw41r6ikDw):

```R
id <- c(1,10,100)
directory <- "specdata"

list.files(directory, full.names=TRUE)[id]
## [1] "specdata/001.csv" "specdata/010.csv" "specdata/100.csv"
```

*Note:* `list.files()` automatically deals with numbered files!

## Coding Standards

 * Usual coding standards apply (indent, break lines, modularize, ...).
 * Be aware of [roxygen](http://roxygen.org/), a neat way to create comments
   and documentation for functions at the same time.
 * *Question:* Are there *any* standards regarding function naming?
   It sure doesn't seem so.
