## Motivation

 * Data comes in raw form; need to tidy it up before analysis.
 * Want: tables with one observation per row, one attribute per column.
 * Get: Some standardized (JSON, XML, ...) or custom text file format or even
        text in natural language.
 * Sources: files, databases, API, websites
 * We want to write scripts that take raw data as input and produce tidy data.
 
## Raw and processed data

> Data are values of qualitative or quantitative variables, 
> belonging to a set of items.

"Raw" data is the original source (we have access to)
which may not be useful for analysis.

"Processed" data is ready for analysis.

*Remember:* What we use as "raw" data may already have been processed a lot!
  Think about sensor data, for instance.
  
## The components of tidy data

Four things to have:

 1. Raw data.
 
     * You ran no software on it.
     * You did not manipulate any values.
     * You did not remove any data.
     * You did not summarize the data in any way.
     
 2. Tidy data.
      
     * One table and file for each "kind" of variable.
     * Each variable in its own column.
     * Each observation in its own row.
     * Link multiple table together with IDs/keys.
     
     *Tip:* Include human-readable column headers.
     
 3. Code book (meta data) -- describes each variable and their values 
    in the *tidy* data set, e.g. units, scales, ....
    
     * Information about the variables (not in the tidy data set), 
        e.g. units.
     * Information about summary choices, 
        e.g. mean vs median.
     * Information about the experimental study design, 
        e.g. extraction vs random sampling or sanitization.
     
 4. A script ("explicit and exact recipe") that transforms 1 into 2+3.
 
     * No parameters.
     * If more convenient, exact and explicit instructions for humans are
        okay as well.
    
## Getting Data
        
### Downloading files

 * Can be part of the processing script!
 * In R: `download.file()` with parameters `url`, `destfile`, `method`.
 
    HTTPS links may require `curl` as method.
 * Store the date and time you downloaded using `date()`.
 
### Reading local flat files

See [R Programming](../02 R Programming/README.md).
Also, see `?connections`.

### Reading Excel files

Use `read.xlsx()` or `read.xlsx2()` from the `xlsx` package.
There is also `write.xlsx()`.

More powerful: XLConnect package.

### Reading XML files

Use `xmlTreeParse()` or `xmlParse()` (also `htmlTreeParse()`) from the `XML`
package to get the tree in an R representation. Functions on XML trees:

 * `xmlRoot(doc)`
 * `xmlName(node)`
 * `xmlValue(node)`
 * `names(node)`
 * `node[[i]]` gives the i-th child
 * `xmlSApply(node, fct)`
 * Can use XPath with e.g. `xpathSApply(node, query, fct)` or 
   `getNodeSet(node, query)`.
 
### Reading JSON files

Use `fromJSON()` from the `jsonlite` package. There is also `toJSON()`.

### Using data.table

Function `data.table()` from the `data.table` package is similar to `data.frame`
but much more efficient.

Further functions:

 * `tables()`
 * Subsetting is different from data frames. 
    
    * Subsetting with a single parameter subsets rows.
    * Subsetting columns is more tricky; 
       you pass (lists of) *expressions* after the comma.
       
       Examples:
       
        * `DT[, list(mean(x), sum(z))]`
        * `DT[, table(y)]`
        * `DT[, w := z^2]` -- adds a new column.
        * `DT[, { tmp <- x+z; log2(tmp + 5) }]`
        * `DT[, a := x > 0]`
        * `DT[, b := mean(x+w), by = a]` --  groups by `a`; all rows in one group
            get the same value of `b`.
        
 * Other than with data frames, copies are not automatically created.
 * Special variables include `.N` -- the number of rows in a group. Example: `DT[, .N, by=x]`
 * You can set a key with `setkey(DT, x)`. 
    Benefits: More convenient (`DT[val]`) and efficient subsetting by values
    of the key; joins with `merge()`
 * Substitute `fread()` of `read.table()` is a lot faster.
 
Find more differences [here](http://stackoverflow.com/questions/13618488/what-you-can-do-with-data-frame-that-you-cant-in-data-table).
