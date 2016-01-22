Have R and RStudio installed. Okay, after course 1 we've got that.

---

This week covers:

 * basic datatypes and functions
 * basic syntax
 * profiling, debugging
 
## Overview and History of R

Find resources on [CRAN](http://cran.r-project.org).

Springer has a book series called *Use R!*.
Other books are listed on [r-project.org/doc/bib/R-books](http://r-project.org/doc/bib/R-books.html).
 
### History of R

 * R is a dialect of S
 * S (Bell labs)
 
    * 1976: Fortran libraries for statistics
    * 1988: C rewrite
    * 1998: version 4 (basically current version)
   
   Commercially owned (TIBCO; today S-PLUS).
   
   *Key idea:* 
   interactive environment; 
   should not feel like programming (in the beginning);
   focus on doing statistical analysis;
   ease user into programming
  
 * R: started 1991, announced 1993
   
   * 1995: GPL
   * 1997: R Core Group formed (controls primary source code)
   * 2000: v1.0.0
   * 2013: v3.0.2
   * ongoing development
   
### Features of R

 * Syntax: similar to S
 * Semantics: look similar, but different
 * Runs "everywhere".
 * Frequent releases, very active development.
 * Core R is lean; many modular packages
 * Lots of graphical functionality 
 * Smooth transition from user to programmer
 * Active user community (mailing lists, SO)
 * Free -- no cost *and* [Stallman-free](http://fsf.org)!
 
### Drawbacks of R

 * Based on 40-year-old technology.
 * Little support for dynamic or 3D graphics. Packages may help.
 * Functionality is based on consumer demand and user contribution.
   No "feature request hotline".
    (Just like any software that is not owned by a company, duh.)
 * Object size limited by physical memory (solutions may be upcoming).
 * Not ideal for *everything* -- of course not!
 
### Design of the R system

Base system (`base` plus some other packages, plus some recommended packages), 
then everything else. 
Find all the stuff on CRAN (4000+ packages) and bioconductor.org.
More things exist off these platforms.

## R Basics

 * There are five atomic classes: 
   `character`, `numeric` (real numbers), `integer`, `complex` and `logical` (boolean).
 * Next most basic object: `vector` of things of the same class.
 * `list` is like a vector, but can contain objects of different classes.
 * `1` --> `numeric`; `1L` --> `integer`
 * Special numbers are `Inf`, `-Inf`, `NaN` (not a number; missing values).
 * Objects can *have attributes* such as names, dimensions, class, length, ...
   Access via `attributes()`.
