## Coding Standards in R

*Goal:* Make your code more readable for others.

 1. Use text files.
 2. Use indentation.
 3. Limit the width.
 4. Limit the length of functions.
 
*Question:* What about naming conventions?



## R Markdown

Integrate R and Markdown (`.Rmd`) as a means of *literate statistical programming*.

Any blocks of the form

    ```{r}
    [code]
    ```
    
Will be executed as R code; there are options to be added to `{r}` such 
as chunk names, whether code or results are shown, and more.
Code of the form `\`r var\`` can be used inline to use results in text.

Package `knitr` converts R Markdown into markdown, 
`markdown` converts markdown into HTML.
If you want slides, you can use `slidify`.

Also, package [`rmarkdown`](https://github.com/rstudio/rmarkdown) 
(based on `pandoc`) contains function `render()` which directly converts 
R Markdown to HTML. You can even call it from the shell:

```bash
Rscript -e "rmarkdown::render('my.Rmd')"
```

### Advantages

 * Data analysis code, results and companion text appear side-by-side in a 
    meaningful way.
 * Code is live -- automatic updates and regression tests.
 * Plain text format (accessible, VCS-able).
 * No redundant output files around.
 * Code from all steps can be included.
 
 
### Disadvantages

 * Can be hard to read.
 * Can be slow to process.
 
 
### Knitr

Knitr is nice because it allows multiple code, documentation and output 
languages/formats. It's also built right into RStudio.

It's good for manuals, tutorials, reports, not-too-long technical documents
and documentations of data preprocessing.

It's less good for long research articles (which is easier to manage if it's
spread across multiple files),
time-consuming computations (which you do not not want to re-do every time
you compile the document),
and anything that requires precise formatting.

Knitr is integrated into RStudio but can also be used like this:

```R
library(knitr)
knit2html("my.Rmd")
```

### Tips and Tricks

 * Use package `xtable` to create nice tables. For example:
 
    ```R
    library(xtable)
    xt <- xtable(summary(data))
    print(xt, type = "html")
    ```
    
 * Use `opts_chunk$set()` to set chunk options globally. Commonly used options
    include `results` (`"asis"` or `"hide"`), `echo` (boolean) and 
    `fig.(height|width)` (numeric).
    
 * Use `cache=T` to store results of expensive chunks. Observe caveats.
