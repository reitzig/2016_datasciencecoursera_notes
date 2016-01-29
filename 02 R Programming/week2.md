## Control Structures

 * `if`, `else`, `while`, `break`, `return` -- business as usual.
    Syntax is just like in Java.
  
    `if` can be used as expression (just like in Scala or Ruby), so
    
    ```R
    x <- if ( P ) { 1 } else { 2 }
    ```
    
    works.
    
 * `for` loops are written in bash-style:
 
    ```R
    for ( i in S ) { print(i) }
    ```
    
    where `S` is a sequence or vector.
    
    Use `seq_along(x)`to get the indices of a vector or sequence 
    (which you can again iterate over).  
    `seq_len(x)` may also be useful in this context; it's much the same
    as `1:x`.
    
    **Note:** the loop variable does *not* remain local!
    After the loop is done with, the loop variable remains visible with
    the value of the last iteration *that got executed*.
    
 * `next` -- skips to the next iteration of a loop (cf. `continue` in Java).
 * `repeat { ... }` -- infinite loop; terminate with `break`.

*Note:* The above are mostly useful in scripts. For interactive sessions,
  there are the `*apply` functions. More later.
  
## Functions

### Defining Functions

 * Define (anonymous) functions by `function(params) { ... }`.
  * Since functions are values, you "name" them just like any other value:
    `foo <- function(...) {...}`.
 * Default values are specified as usual, e.g.
    `function(x, y=10) { ... }`.
    
   You can use `NULL` as default value if there is no good default value.
 * Last expression is return value; or whatever we `return` explicitly.
 * There is special argument name `...` which collects an arbitrary number of
   arguments. So, for instance:
   
   ```R
   mymean <- function(v, ...) {
     mean(v[c(...)])
   }
   ```
   
   computes the mean of any subset of the specified vector, e.g. 
   `mymean(1:5, 1,3,5)` returns `3`.
   
   This is of particular use for defining *generic* functions; more later.
   
   *Note:* `...` does not have to appear at the end of the argument list!
     Positional and partial matching will be turned off after `...`, but
     in combination with named arguments `...` can occur anywhere.
     Unnamed or partially named parameters will simply be swallowed by `...`.
     Check out `paste` and `cat`, for instance.
    
### Using functions

 * All arguments are *named*.
 * Specified parameters are matched to formal arguments by position and/or name.
   Unnamed parameters are assigned left-to-right to so far unmatched arguments.
   
   Weird things work; best stick to names unless specifying a prefix
   of all arguments (by position).
 * Partial matching (cf. column indices) works for argument names;
    priorities are (in decreasing order) exact match, partial match, position.
 * Get a list of (formal) arguments with `formals(f)`.
   This literally gives you a `list`; for human-readable information,
   use `args(f)`.
 * Function parameters are accessed evaluated *lazily*.
 * As a consequence, Arguments can be *missing*. So, for instance the function
 
    ```R
    f <- function(a,b) { if (a < 5) { b } else { a+1 }}
    ```
    
    can be called as `f(6)` -- parameter `b` is not needed so we don't get
    punished for its absence. `f(2)` will fail, though, because `b` is needed
    but there is no default.

*Notes to self:*

 * In code that is to be read by others,
   use named arguments for function calls whenever there can be *any* unclarity.
 * Avoid using missing parameters as a feature. Specify defaults if you want
   calls to "legally" leave out parameters.
 * Don't use partial matches! We have tab-completion.

---
 
*Note to self:* Yes, the column-means example function is unnecessarily ugly:
 
```R
sapply(1:ncol(mat), function(i) { mean(mat[,i]) })
```
    
Now if only there were nicer syntax for anonymous functions...
