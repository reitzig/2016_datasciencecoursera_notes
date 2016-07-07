## Case Study: Analyzing Human Activity with Smartphones

We have acceleration and gyro data collected using smartphones.
Training data has measurements associated with one of six
activities: lying, sitting, standing, walking up, walking down, walking.

*Goal:* Predict the current activity using data from the carried smartphone.

Here are some exploration strategies to try:

 1. Plot individual values by index, grouped by activity. 
     Are there obvious patterns?
     
 2. Cluster the data hierarchically according to a subset of the measurements 
      (e.g. triples that belong together, that is x, y and z components of the 
      same quantity) and group by activity. Draw the dendrogram.
      
      Do the data cluster nicely by activity?
      
 3. Perform SVD and inspect the first few singular vectors (plot values by index, 
      colored by activity). Are there patterns?
      
 4. Using the SVD, determine the feature that contributes the most variation
      to the data (`which.max(svd$v[,i])`).
      Add such maximum contributers to the columns to cluster with.
      
 5. Use k-means clustering with `centers=6` on the whole data sets (minus subject,
      activity labels). Repeat a couple of times.
      
Things we find:

 1. Max acceleration (three components) distinguishes active from passive activities.
 
 2. The maximum contributor of `i=2` is the z-component mean body acceleration 
     in the frequency domain; adding it to clustering nicely separates the three 
     moving activities from each other.
     
 3. K-means can separate the moving activities pretty well, the passive ones
      remain mushed together.
      
More work needed to separate passive activities!




## Case Study: Air Pollution

*Question:* Has the clean air act worked, that is are air pollution levels 
lower now (2012) than they were before (1999)?

*Are 11% reso. 5% missing values a problem?* -- depends on the question.
It probably pays to look for patterns.

*We note 2% of negative values which should not be there* -- we may want to
check that out. They may just be measurement errors due to low values;
we *do* not that negative values appear more outside of the hot summer months
during which pollution is often high.

Here is what we tried:

 1. Global summary (median, mean) and (log) boxplot of all measurements.
 
      Median and mean both went down by about 30%, but maximum increased by
      600%! The "spread" of the data increased.
      
 2. We check how measuring stations changed (in one county); we have some
      that were active in both years, which allows us a fairer comparison.
      
 3. Plot values for single stations over time.
 
      We note (for one station) that there is not even data for the first half 
      of 1999, and only a few months in 2012. There is no overlap in months!
      That maybe problematic for intepreting the data.
  
 4. We look at aggregates for states and compare state means for the two years
      by plotting means by year and connecting the points for each state
      (using `segments()`).
      
      We note that the mean goes down for most states.
