### Reading from MySQL

Use package `RMySQL`. Tables become data frames.

 * `dbConnect(MySQL(), ...)` gives you a connection `c` 
      (specify `db=` to get a connection to a specific database on 
      the given server).
 * `dbGetQuery(q, "query";)` executes SQL queries and returns the result.
 
    *Alternative:* `q <- dbSendQuery(...)` + `fetch(q)`. Allows you to
      fetch only a few rows of the result.  
      Use `dbClearResult(q)` to clear the query on the server.
 * `dbDisconnect(c)` closes a connection.
 
More:

 * `dbListTables(c)` lists all tables in the database.
 * `dbListFields(c, "table")` lists all fields of the specified table.
 * `dbReadTable(c, "table")` gets you the whole table in a data frame. 

More documentation on 
  [CRAN](https://cran.r-project.org/web/packages/RMySQL/RMySQL.pdf)
and 
  [here](http://www.r-bloggers.com/mysql-and-r/).
  
*Side note:* Package `sqldf` lets you run SQL queries on data frames!


### Reading from HDF5

  [HDF5](http://www.hdfgroup.org/HDF5) 
is used for storing large, structured data sets.
Data are organized in hierarchical groups and stored as multidimensional arrays
with metadata (similar to data frames).

Use the `rhdf5` package. Install from Bioconductor, e.g. like so:

```R
source("http://bioconductor.org/biocLite.R")
biocLite("rhdf5")
```

Relevant functions include:

 * `h5createFile()`
 * `h5createGroup()`
 * `h5ls()`
 * `h5write()` -- writes arrays, matrices, data frames, ...
 * `h5read()`
 
*Note:* You can read and write in chunks; check out the `index` parameter.

Find more information [in the documentation](http://bioconductor.org/packages/release/bioc/vignettes/rhdf5/inst/doc/rhdf5.pdf).


### Reading from the web

 * Web-scraping: extract data from websites. (Observe ToS!)
 * You can get sources of websites by
 
    ```R
    con  <- url("...")
    html <- readLines(con)
    close(con)
    ```
    
    Gives you a flat string.
 * Better: `htmlTreeParse()` from package `XML`.
 * Also: `GET()` + `content()` + `htmlParse()` from package `httr` like so:
 
    ```R
    html <- GET(url)
    str  <- content(html, as="text")
    tree <- htmlParse(str, asText=T)
    ```
    
    *Advantage:* You can authenticate. (Use handles!)

Recall `xpathSApply` et al.!


### Reading from APIs

Use the `httr` package. Relevant functions include:

 * `app <- oauth_app()`
 * `sig <- sign_oauth1.0(app, ...)`
 * `GET("query url", sig)`

Process the obtained `content()` using 
  [appropriate functions](week1.md) 
depending on the data format.

`httr` also allows `POST`, `PUT` and `DELETE` as well as other authentication
methods.

See [here](https://github.com/hadley/httr/blob/master/demo/oauth2-github.r) 
for a full example with Github.


### Reading from other sources

> There's a package for everything. 
> Google [data storage method] R package.

Some ways of interacting with files:

 * Built-in: `file()`, `url()`, `gzfile()`, `bzfile()`.
 
    More: `?connections`
 
 * Package `foreign`: several `read.X` functions, e.g. Octave and SPSS. 
 * More database wrappers: `RPostgreSQL`, `RODBC`, `RMongo`.
 * Read images with packages `jpeg`, `readbitmap`, `png`, `EBImage`.
 * Read GIS data with packages `rdgal`, `rgeos`, `raster`.
 * Read music with packages `tuneR` and `seewave`.   
