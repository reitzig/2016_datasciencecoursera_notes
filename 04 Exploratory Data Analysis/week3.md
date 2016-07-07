## Clustering

Organize things into groups that are *close*.

 1. How do we define "close"?
 2. How do we compute the grouping?
 3. How do we *visualize* the grouping?
 4. And how do we interpret it?
 
Google "Cluster Analysis" -- it's widely used and useful!

The choice of distance metric is *essential* -- garbage in, garbage out!
Popular choices include Euclidian, Manhattan
Make sure the distance you choose makes sense for your problem!

### Hierarchical Clustering

*Idea of hierarchical clustering:* 
Iteratively merge the two *closest* (groups of) things.
(This results in a tree of merge operations.)

How do we define the distance of *clusters* of points? Options include

 * complete linkage (maximum pairwise distance),
 * average linkage (distance of means/centers of mass).
 
While the algorithm is deterministic, the outcome differs a lot with above
choices, plus where to cut off the tree. Good for exploratory analyses, but
maybe less so for getting "definitive" answers.


So how to do this in R?

 1. Compute all pairwise distances with `dist()`.
 2. Compute the clustering with `hclust()` (pass it the distance matrix).
 3. Plot the dendrogram of the result with `plot(_)` or
    `plot(as.dendrogram(_))`.
 
*Note:* For slightly fancier dendrograms, download `myplclust` from the
course repository. More examples are on 
  [R Enthusiasts](http://gallery.r-enthusiasts/RGraphGallery.php).
  
*Note:* Function `heatmap()` uses hierarchical clustering; it can be useful
for visualizing high-dimensional data to unearth patterns.



### K-means Clustering

Here, we start with the number of clusters we imagine the data should contain,
and guesses for cluster *centroids*.

*Idea:* 

 1. Assign points to closest centroid.
 2. Recompute centroids.
 3. Iterate.
 
In the end, we get estimates for the cluster centroids, and an induced 
assignment of points to clusters.

In R: `kmeans(df, centers = k)`; returns a rather complex object `o`.
If you just want the final cluster assignments, those are in `o$cluster`.

Visualizing the result is easy; plot points as usual, group by `o$cluster`,
and add `o$centers` if desired.

*Note:* Aside from guessing/eyeballing, you can use cross-validation,
information theory or other methods to estimate the number of clusters.

*Warning:* Depending on the implementation how you pick initial centroids,
k-means may not be deterministic! Therefore, you should look at the results
of several runs before making up your mind; it may very well be that you get 
very different result every time, in which case there is probably little
clustering in your data.




## Dimension Reduction

Assume we have many multivariate variables. There are two goals one might have:

 1. *Compression:* Find the best matrix with *fewer variables* (i.e. of lower rank)
    that explains the original data.

 2. *Statistical:* Find a new set of *uncorrelated* set of multivariate
     variables that explain as much variance of the original data as possible.

    
*Question:* What do "explain" and "best" mean here?

Solutions include:

 1. *Singular value decomposition*:
 
      If $X$ is a matrix with each variable in a column and each observation
      in a row, then the SVD is a *matrix decomposition*
      $X = UDV^T$
      where the columns of $U$ are orthogonal (left-singular vectors), 
      the columns of $V$ are orthogonal (right-singular vectors), and
      $D$ is a diagonal matrix (singular values).
     
      The idea is that if you use only the first few rows/columns of the
      three matrices, you get an approximation of the data. So, the first
      few rows/matrices of $U$ and $V$ (rescaled according to $D$) can serve
      as a more compact stand-in for the real data.
     
      Use `svd(matrix)` to get it.
     
      The entries in $D$ correspond to the percentage of the overall variation
      explained by that component ("Variance explained") after squaring
      and dividing by the sum of squares of the values, that is
      `svd$d^2/sum(svd$d^2)`.
      That is, they indicate which principle component influences the data by how
      much.
     

 2. *Principle Components Analysis*
  
      The *principle components* are equal to $V$, i.e. the right-singular values 
      in the SVD, if you first scale (subtract the mean, divide by the standard 
      deviation) the variables.
  
      Get it by `svd(scale(matrix))$v` or `prcomp(scale(matrix))`.
      
      These vectors expose patterns in the data, but may mix them up.
      
      Read more in 
        [A Tutorial on Principal Component Analysis](http://arxiv.org/pdf/1404.1100.pdf)
      by Jonathon Shlens.

      
**Problem:** What about `NA`s? There are many possibilities.
On possibility:
Library `impute` offers `impute.knn()` which replaces each `NA` with the 
average values over a few "closest" (w.r.t. k-nearest neighbours) rows.

*Alternatives:* factor analysis, independent components analysis, 
latent semantic analysis.

*Note to self:* Further reading needed.
