# R Programming

This course covered the the basics of R programming.

Notes are organised by week:

 * [Week 1](week1.md): History, ecosphere and basic data types.
 * [Week 2](week2.md): Control structures, functions, dates and times.
 * [Week 3](week3.md): Loop functions (aka "attempts at functional programming")
                       and debugging.
 * [Week 4](week4.md): Data inspection, random sampling and profiling.

I also did the (optional) swirl lessons, which I can only recommend.
Ideally, do them *before* watching the videos!
I put what I learned where it fit best in the above files;
things that did not fit anywhere went
  [to their own file](lessons-swirl.md).

## Installing recent R and swirl on Ubuntu

I had installed R from the Ubuntu repositories (14.04 LTS) which was
some version 3.0.x. It was not possible to install swirl on that.
I followed
  [this guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04).
In a nutshell:

 1. Add a CRAN deb repository for `apt`.
 2. Install the latest R.
 3. Install some dev libraries (using `apt`).
 4. `install.packages("swirl")` in R.

    **Important:** Make sure to install swirl in userspace!
    The library stores your progress which it can not do if it can not
    write to it's own location! (D'oh, I know.)
