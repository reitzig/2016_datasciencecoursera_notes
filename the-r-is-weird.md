## Weirdness in R

Some things I found confusing, hilarious and/or enraging:

### UI

 * The CLI binary is called `R` and the file type is `.R`.
   Who uses capitals?!
 * The console in RStudio breaks GNU/Linux conventions:

    * CTRL+C does not do anything.
    * Select + Mouse3 does not copy-paste.

### Language Design

 * `<-` is a pain to type (on American layouts, anyway).
 * The paradigm is neither functional nor object-oriented,
   but uses elements of both (awkwardly).
 * All libraries dump their functions into the global namespace.

### API

 * There does not seem to be any agreement on function name conventions:
   underscores, dots and camel case all exist.
 * `c(1,2,3)` and `c(1:3)` are different things.
 * Subsetting with `[...]` works differently on matrices than everywhere else.
 * `x + y` *does* something on vectors whose dimensions do not match.
 * `[1,2]` and `[[1,2]]` have completely different behaviour.
 * `length()` behaves differently on matrices (number of entries)
   and data frames (number of columns).

   *Solution:* Use `nrow()` and `ncol()`.

Pull requests with rational explanations
(target group: computer scientists, programmer)
are appreciated.
