### Editing Text Variables

String manipulation functions:

 * `tolower()` -- lower-case all letters
 * `toupper()` -- upper-case all letters
 * `strsplit()` -- splits strings by a separator
 * `sub()` -- substitute first substring by something
 * `gsub()` -- substitutes *all* substrings
 * `grep()`, `grepl()` -- finds occurrences of strings

In library `stringr`:

 * `nchar()` -- string length
 * `substr()` -- returns the specified substring
 * `paste()` -- joins strings together
 * `paste0()` -- joins with empty separator
 * `str_trim()` -- trims excess whitespace from beginning and end

Regular expressions as usual; use with `grep(l)`, `(g)sub` and others.


### Working with Dates

Package `lubridate` has lots of nice stuff, e.g. quick date creators
`ymd()`, `mdy()`, `dmy()`, and so on.

Add durations to date(-time)s like `date + days(2) + hours(3)`.
Compute differences with `interval()` and print `as.period()`.
Change the time zone using `with_tz()`.

Because dates are intricate, `lubridate` distinguishes instants, intervals,
durations, and periods. See 
  [Dates and Times Made Easy with lubridate](http://dx.doi.org/10.18637/jss.v040.i03)
by G. Grolemund and H. Wickham (2011).


### Some data resources

 * Open government sites, e.g. `data.un.org`, `govdata.de` or `data.gov`.
 * `gapminder.org`
 * `asdfree.com`
 * `infochimps.com/marketplace`
 * `kaggle.com`
 * Several datasets curated by data scientists
 * ... many more ...
 
Plus, many web apps have APIs, some with dedicated R packages!
