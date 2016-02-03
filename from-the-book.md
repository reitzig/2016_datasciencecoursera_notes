Here I collect notes I took reading books associated with the course.

## J. Leek: The Elements of Data Analytic Style
*Version from 2015-03-02*

The book is mostly a big checklist you can validate your analysis against.
Many things are (academia-)common sense, but useful advice anyway.
I took note of things I did not know before, statements a particularly
agree with, and such that I think warrant discussion.

### Quotes

 * > Prediction is about tradeoffs

 * > If you reproduce someone else's analysis and identify a problem, bug
   > or mistake you should contact them and try to help them resolve them
   > problem rather that [sic] pointing the problem out publicly or
   > humiliating them.

   I think errors in published work should *always* be made public as fast as
   possible. The tradeoff between speed and involvement of the authors depends
   on the case at hand. Has it already been peer reviewed? What's the potential
   damage? But, eventually, mistakes *have* to be fixed.

   That said, humiliation should not have any part in it either way.
   Reporting the facts of the situation is quite enough.

 * > The goal for figures is that when viewer with an appropriately detailed
   > caption, [figures] can stand alone without any further explanation as
   > a unit of information.

   I don't think I agree with this as a general rule. If you want to support the
   "abstract -- figures -- conclusion" mode of reading, sure.
   Beyond that, I think figures should *support the story*. They do not always
   have to make sense in isolation.
   That said, if you can do both, do so!

 * > Throughout your written analysis you should focus on how each element:
   > text, figures, equations, and code contribute to or detract from the story
   > you are trying to tell.

 * > In the R programming language be sure to use `useNA` argument to
   > highlight missing values `table(x, useNA="ifany")`.

 * > 70% in training and 30% in validation

 * > One really common choice for correcting for multiple testing is to use
   > the false discovery rate to control the rate at which things you call
   > significant are false discoveries.

 * > **Overfitting** -- interpreting an exploratory analysis as predictive.

   Yet another definition of the term!

 * > Github is an example of a distributed version control system where a
   > version of the files is on your local computer and also available at
   > a central server [...] Multiple people can then work on the analysis
   > code and push changes to the central server.

   While I certainly recommend using Git and maybe Github -- a free alternative
   may be more suitable for research, e.g. a university-hosted
   [Gitlab](https://about.gitlab.com/) -- this paragraph does not do either
   justice. You have *all* versions on your computer and the server
   (provided everybody pushes all their branches and commits).

   Be aware that Github supports
   [making code revisions citable](https://guides.github.com/activities/citable-code/).
   Also, it may not be the best place to store data; see e.g.
   [here](http://academia.stackexchange.com/q/987/1419)
   for some pointers.

### Pointers

 * [Heteroscedasticity](https://en.wikipedia.org/wiki/Heteroscedasticity)

   > A collection of random variables is heteroscedastic if there are sub-populations that have different variabilities from others.

   Opposite: [Homoscedasticity](https://en.wikipedia.org/wiki/Homoscedasticity)

 * Compare two measurements of the same value using
   [Bland-Altman_plot](https://en.wikipedia.org/wiki/Bland-Altman) aka
   Tukey mean-difference plots.

 * Common smoothers:
   [smoothing splines](https://en.wikipedia.org/wiki/Smoothing_splines);
   [moving averages](https://en.wikipedia.org/wiki/Moving_average);
   [local regression](https://en.wikipedia.org/wiki/Local_regression) (LOESS).

 * [Random forests](https://en.wikipedia.org/wiki/Random_forests)
   are a successful prediction technique.

 * [Familywise error rate](https://en.wikipedia.org/wiki/Family_wise_error_rate)
   is the probability of making one or more false discoveries, or type I errors,
   among all the hypotheses when performing multiple hypotheses tests.

 * [R CMD check](http://r-pkgs.had.co.nz/check.html) checks your code
   for common problems.

### Further Reading

 * Karl Browman: [How to display data badly](https://github.com/kbroman/Talk_Graphs/tree/iowastate2013)
 * William S. Cleveland & Robert McGill: [Graphical Perception: Theory, Experimentation, and Application to the Development of Graphical Methods](http://dx.doi.org/10.1080/01621459.1984.10478080)
    [[no paywall](http://info.slis.indiana.edu/~katy/S637-S11/cleveland84.pdf)]
 * Willian Strunk Jr: [The Elements of Style](https://en.wikipedia.org/wiki/The_Elements_of_Style)
 * Hilary Mason: Entertain, don't teach. (no longer available?)
