# R Programming

This course covered the the basics of R programming.

Notes are organised by week:

 * [Week 1](week1.md): History, ecosphere and basic data types.
 * [Week 2](week2.md): Control structures, functions.
 
## Installing recent R and swirl on Ubuntu

I had installed R from the Ubuntu repositories (14.04 LTS) which was
some version 3.0.x. It was not possible to install swirl on that.
I followed 
  [this guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04)
to get swirl installed. In a nutshell:

 1. Add a CRAN deb repository for `apt`.
 2. Install the latest R.
 3. Install some dev libraries (using `apt`).
 4. `install.packages("swirl")` in R.
