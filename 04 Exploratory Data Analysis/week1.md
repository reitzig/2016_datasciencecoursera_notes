## Principles of Analytic Graphics

Follows Edward Tufte's book [Beautiful Evidence](https://www.edwardtufte.com/tufte/books_be).

 1. **Show comparisons**. Evidence *compared to what*? 
    Need to show control!
    
 2. **Show causality/mechanism/explanation/systematic nature**.
    What supports your idea of the underlying mechanism? 
    
 3. **Show multivariate data**. Do not hide potential confounding factors!
 
      Example: [Simpson's paradox](https://en.wikipedia.org/wiki/Simpson's_paradox).
    
 4. **Integration of evidence**. Integrate words, numbers, images, diagrams.
    
 5. **Describe and document the evidence** with appropriate labels, scales, sources, ...
 
 6. **Content is king**. Story before presentation.
 
 
 
## Exploratory Graphs

*Goal:* 
Summarize data, highlight broad features; 
explore basic questions and hypotheses; 
suggest modeling strategies.
 
Graphs can be useful for

 * understanding properties of and
 * finding patterns in data,
 * suggesting modeling strategies and
 * "debugging" analyses.
 
 *Exploratory* graphs are usually
 
  * quickly made,
  * legion,
  * designed for personal understanding (as opposed to communication).
  
  
Simple, **one-dimensional summaries**:

 * Six-number `summary()`: min, 1st quartile, median, mean, 3rd quartile, max.
   Can already answer some questions.
   
   Can plot as `boxplot()` (maybe plus `abline()`).
 * Histogram: information about the distribution of values.
 
   *Tip:* `hist()` + `rug()` + `abline()`
   
 * Barlots: for categorical variables.
 
 
**Two-dimensional** plots:

 * Multiple/overlayed 1D plots.
 * scatterplots or
 * smooth scatterplots.
 
**Multi-dimensional** plots can be

 * multiple/overlayed 2D plots or coplots,
 * 2D plots with color, size or shape adding dimensions,
 * spinning plots or
 * actual 3D plots (rarely useful).
 
 
 
## Plotting Systems in R
 
Three systems: 

 * base ("artist's palette" model)
 * lattice (single call for each plot)
 
    *Example:* `xyplot(A ~ B | C, data = df, layout = ...)` for a panel plot.
 * ggplot2 (graphical grammar)
 
 
### Devices

Plots are created on *graphics devices* which includes the screen or 
files of different types (PDF, PNG, SVG, PS, ...).
Use `?Devices` to see a list; more are available on CRAN.

Example:

```R
pdf(file = "plot.pdf")
[plotting code]
dev.off()
```

`dev.cur()` returns the number of the current device; `dev.set()` moves you
to the specified device.

Use `dev.copy()` (or special functions like `dev.copy2pdf()`) to copy the current
plot to another device.

Example:

```R
[plotting code]
dev.copy(png, file = "plot.png")
dev.off()
```

*Special mention:* Package `tikzDevice` provides a device that generates
TikZ code!
 
### Base Plotting

Packages: `graphics`, `grDevices`.

Two phases: initialize, annotate.

Use `example()` for checking some parameters, e.g. `example(points)`.

**Initialisation**

Use e.g. `plot()` (generic, many parameters!), `boxplot()` and `hist()` 
to start a graphics device (if there is not already one) and draw a new plot.
Use `type = "n"` to not plot any date, for instance if you want to 
add points in subsets later.

*Key parameters:* 

 * `pch` -- plotting symbol 
 * `lty` -- line type
 * `lwd` -- line width
 * `col` -- color
 * `xlab` -- label of x-axis
 * `ylab` -- label of y-axis
 
Global parameters can be set by `par()`:

 * `las` -- orientation of axis labels
 * `bg` -- background color
 * `mar` -- margin size
 * `oma` -- outer margin
 * `mfrow` -- number of plots per row/columns (plots filled row-wise)
 * `mfcol` -- number of plots per row/columns (plots filled column-wise)
 
Use `par("name")` to see the default of parameter `name`.

Create multiple plots by just calling initializers multiple times; 
`mfrow` and `mfcol` control the layout.
The *outer* margin surrounds all of them!

**Annotation**

 * `lines` -- add lines
 * `points` -- add points
 * `text` -- add text labels within the plot
 * `title` -- annotations of axes, title and subtitle, margin
 * `mtext` -- add text to the margins
 * `axis` -- add axis ticks/labels
 * `abline` -- adds a line that is an output of e.g. `lm()`.
 * `legend` -- adds a legend.

### Lattice

Packages: `lattice`, `grid`.
