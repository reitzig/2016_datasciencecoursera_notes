## `str()` -- structure

 * Alternative to `summary()` -- more about structure (rather than
   statistical information).
 * Compact structure information of data objects.
 * Answers "what's in this object?" with (ideally) one line per basic object.
 * Applies to functions as well!
 * Does all kinds of neat things on the different object types, trying
   to guess what you would be most interested in.
 * Nifty: combine with `split()` like e.g.

   ```R
   str(split(airquality, airquality$Month))
   ```

   This gives you a `str`-summary of the data per month.
   (That's probably also a useful combination with `summary()`.

## Random Sampling

### Simple distributions

See [Swirl notes](lessons-swirl.md) which I augmented with some things
from the lecture.

### More complicated models

 * Suppose a more complex model, e.g. y ~ a + b x + e$

   where x,e are random and a,b are constants.
 * Solution: sample vectors of realizations of x and e
   separately; combine by vector operations.

   ```R
   x <- rX(100, ...)
   e <- rY(100, ...)
   y <- b0 + b1 * x + e
   ```

   Creates a vector with 100 random realizations according to y.
 * Scales to more complicated models, e.g. Y ~ Poisson(µ) where
   parameter µ is noisy, defined by log µ ~ a + b x with normal x.
   Just sample inside-out, doing the arithmetics along the way.


## Profiling

 * *Problem:* program is too slow.
 * *Goal:* find out where most time is spent and optimize there.

   That is, use *data* to steer your improvement efforts.
 * Avoid premature optimization.

### Tools

 * `system.time()` -- like UNIX `time`, that is gives you the time
   it took to evaluate the given expression.

   *Note:* user/CPU time U vs elapsed/wall-clock E time.
 * U < E can happen if the CPU waits a lot, e.g. on I/O,
   or other (background) tasks were run as well.
 * E < U can happen if multiple cores were used (U is added up
   across CPUs).

   R links some parallel-enabled libraries, e.g. BLAS libraries.
   There is also the `parallel` package for parallel and distributed
   programming.

But in order to do this you need to know where to look already!
What if you do not?

 * `Rprof()` -- starts the profiler (needs to be compiled into R).
 * `summaryRprof()` summarizes the results.
 * Don't use together with `system.time()`.
