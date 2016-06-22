### Lattice

Package: `lattice` (based on `grid`).

Useful for high-dimensional data and creating many plots at once, 
particular panel plots.

Main functions (which return objects of class `trellis`):

 * `xyplot()` -- scatterplots
 * `bwplot()` -- box-and-whiskers plots
 * `histogram()` -- histograms
 * `stripplot()` -- like a boxplot but with points
 * `dotplot()` -- dots on "violin strings"
 * `splom()` -- scatterplot matrix (similar to `pairs()` in base)
 * `levelplot()`, `contourplot()` -- plotting "image" data
 
And more!

**Note:** On the shell, `lattice` plots get auto-printed, but in scripts
you have to use a print method explicitly!

```R
plot <- xyplot(...)
print(p)
```
 
Typically, pass a formula as the first argument:

```R
xyplot(y ~ x | f * g, data = df)
```

If there is no meaningful `y`, as is the case e.g. for histograms and boxplots,
just leave it out. In all functions by `xyplot()`, if you leave out `y` the
default is `names(x)` if `x` is named, and a factor with a single level
otherwise.

Here, `f` and `g` are *conditioning* variables with `*` a particular interaction.
Condition variables cause one plot per category of the condition factors or
shingles being created. `f * g` (or `f + g`) just means to create one plot
per combination of categories.
 
Note also the `groups` parameter. Pass it a factor and have points be grouped
by it, using color and other point styling. This can also be achieved
by using `y1 + y2 ~ x` if you have the values in separate vectors.

*Note:* Plotting the sum of two variables is done by `I(y1 + y2) ~ x`.

Customization of all plots in at once happens with *panel functions*:

```R
xyplot(y ~ c | f, panel = function(x, y, ...) {
  panel.xyplot(x, y, ...)      # defaults for xyplot
  panel.abline(h = median(y))  # horizontal line
  panel.lmline(x, y, col=2)    # linear regression line
})
```

Check out all the `panel._()` functions that come with `lattice`!




### ggplot2

Package `ggplot2`,
an implementation of *Grammar of Graphics* by Leland Wilkinson.

Plots are made up of *aesthetics* (size, shape, color) and *geoms* (points, lines).
Data has to come organized in data frames (or the parent environment);
factors are important for indicating subsets and should be labeled.

`ggplot2()` is the core function; `qplot()` (for "quick plot") is a shortcut
that suffices in many cases.


**qplot**

The most basic plot is:

```R
qplot(x, y, data = df)
```

where `x` and `y` are column names in `df`.
You can do things like `log(x)`, obviously.

Adding a parameter `color = factor` (which is an aesthetic) groups points.

You can control how the data is shown by adding a parameter `geom = c(...)`
with values like `"point"` (the individual values), `"smooth"` 
(a smoothed line through the line with a confidence interval) or `"density"`
(a smoothed line for histograms).

A longer version for adding geoms is by *adding* them. For instance:

```R
qplot(...)  +  geom_smooth(method = "lm")
```

This adds a smooth with linear regression *per group/panel* to the plot.


Histograms are easy if you pass just one variable:

```R
qplot(x, data = df, fill = f)
```

Note the additional aesthetic that fills the bars according to the categories
of the given factor `f`.

In line plots, you can use `shape = f` and `color = f` for separating groups.

Panel plots are created using *facets*:

```R
qplot(x, y, data = df, facets = . ~ f)
```

Using `f ~ g` specifies that `f` should be used for rows, and `g` for columns;
a `.` means that there is only one row resp. column.

*Note:* Modifying the design of `qplot()` plots is tricky; 
use full `ggplot2()` then.


**ggplot**

The basic components are:

 * A *data frame*.
 * *Aesthetic mappings* that descripe how data maps to color, size, ...
 * *Geom*etric objects, e.g. points, lines, shapes.
 * *Facets* for conditional plots.
 * *Stat*istical transformations, e.g. binning, quantiles, smoothing.
 * *Scales* for aesthetic maps.
 * A *coordinate system*.
 
Plots are built up in layers:

 1. Plot the data with basic aesthetics.
 2. Overlay a summary.
 3. Add metadata and annotation.
 
*Example:*
 
 ```R
  ggplot(df, aes(x,y))       # Basic plot setup
+ geom_point()               # Draw points
+ facet_grid(. ~ f_)         # Split into plots w.r.t factor f
+ geom_smooth(method = "lm") # Overlay with a smooth line
```

The order of the components does not matter (unless they are contradictory); 
ggplot2 collects everything first, then prints.

*Note:* `ggplot()` returns an object that you can call 
 `summary()` and `print()` on!
 
There are many ways to annotate:

 * Functions like `xlab()`, `ylab()`, `labs()` and `ggtitle()`.
 * The `geom_X()` functions have options.
 * `theme()` controls global options.
 * There are two standard themes, `theme_gray()` and `theme_bw()`.
 
For instance, you can do:

```R
# Plain modification
geom_point(color = "steelblue", size = 4, alpha = 1/2)

# Grouping!
geom_point(aes(color = f), size = 4, alpha =  1/2)

# Dottet, thicker smooth line
geom_smooth(size = 2, linetype = 3, method = "lm", se = F)
```

*Note:* When changing plot limits, there are two ways:

```R
# This *drops* non-confirming points from the data set *before plotting*.
g + geom_line() + ylim(a,b)

# All points are used for plotting but the plot area is clipped
g + geom_line() + coord_cartesian(ylim = c(a,b))
```

### Colors

Color palettes in the `grDevices` package include `heat.colors()` and `topo.colors()`.
Function `colors()` lists all colors that can be used with plotting functions.
Package `RColorBrewer` has some more palettes for sequential, divergent and
qualitative data; `brewer.pal()` gets you sequences that can be used with
the functions below.

Function `colorRamp()` can be used to create functions that model gradients.
For instance:

```R
grad <- colorRamp(c("red", "blue"))
grad(0)              # red
grad(1)              # blue
grad(0.5)            # purple
grad(seq(0,1,len=6)) # six colors spread evenly between red and blue
```

Function `colorRampPalette()` gives you a function that does the last directly,
i.e. gives you a number of colors spanning the spectrum. Results of this function
can be passed to plotting functions like `image()`.

Function `rgb(r,g,b,a)` lets you create arbitrary RPG colors with optional alpha.
