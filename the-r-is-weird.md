## Weirdness in R

Some things I found confusing, hilarious and/or enraging:

### UI

 * The CLI binary is called `R` and the file type is `.R`.
   Who uses capitals?!
 * The console in RStudio breaks GNU/Linux conventions:

    * CTRL+C does not do anything.
    * Select + Mouse3 does not copy-paste.
 * Under GNU/Linux, programs and libraries are usually not installed into
   userspace. R does not deal with this gracefully. At all.
   And then, you change the path where it installs libraries.

   Additional fun: Running swirl courses when you installed the library
   to global space (`sudo R` -- hrmpf!) is impossible since it won't environment
   try and create your user profile in userspace!

### Language Design

 * `<-` is a pain to type (on American layouts, anyway).
 * You assign by `<-` but set default parameters with `=`.
 * The paradigm is neither functional nor object-oriented,
   but uses elements of both (awkwardly).
 * All libraries dump their functions into the global namespace.
 * If you set the dimension of a vector, you convert it into a matrix,
   class and everything.
 * `&&` and `||` do not mean the same as everywhere (?) else.
 * Objects can have multiple classes, e.g. `class(Sys.time())`.

### API

 * There does not seem to be any agreement on function name conventions:
   underscores, dots and camel case all exist.
 * `c(1,2,3)` and `c(1:3)` are different things.
 * Subsetting with `[...]` works differently on matrices than everywhere else.
 * `x + y` *does* something on vectors whose dimensions do not match.

   *Answer:* This is because there is no scalar type! `c(1,2) + 1` means
   `c(1,2) + c(1)`, so we need shorter vectors to be "recycled" just to
   get basic scalar operations to work!
 * `[1,2]` and `[[1,2]]` have completely different behaviour.
 * `length()` behaves differently on matrices (number of entries)
   and data frames (number of columns).

   *Solution:* Use `nrow()` and `ncol()`.

Pull requests with rational explanations
(target group: computer scientists, programmer)
are appreciated.
