---
title: "Fun with R:  Ecological data analysis in R"
author: "Vianney Denis"
date: "2020/09/22"
output:  
  "html_document":
   theme: united
   highlight: tango
   toc: true
   toc_depth: 3
   toc_float: true
   number_sections: false
   code_folding: show
    # ioslides_presentation 
---

# **Topic 3 - Plots**

## Base plotting environment

### Point and line plots

The most commonly used plot function in R is `plot` which generates both points and line plots. For example, to plot length of petal `Petal.Length` vs the width of petal (`Petal.Width`) for each individual measured:


```r
data(iris)
plot(iris$Petal.Length) # index observation for Petal.Length
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-1-1.png" width="672" />

```r
plot(iris$Petal.Width) # index observation for Petal.Width
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-1-2.png" width="672" />

```r
plot(Petal.Length ~ Petal.Width, dat = iris) # pairwise
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-1-3.png" width="672" />


The above plot command takes two arguments: Petal.Length ~ Petal.Width which is to be read as plot variable `Petal.Length` as a function of `Petal.Width`, and `dat = iris` which tells the plot function which data frame to extract the variables from. Another way to call this command is to type:


```r
plot(iris$Petal.Length ~ iris$Petal.Width) # using the $ operator
```


The `plot` function can take on many other arguments to help customize the default plot options. For example, we may want to change the axis labels to something more descriptive than the table variable name:



```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', # add label to x-axis 
     ylab = 'Petal length (cm)', # add label to y-axis 
     main = 'Petal width and length of iris flower') # add title to the plot 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-3-1.png" width="672" />

We can change the symbole type using argument `pch` (see `?pch` for point options). `pch = 19` will produce a solid circle. The argument `cex` will control is size. We can also set its colors to be 90% transparent (10% opaque) using expression `col = rgb (0,0,0,0.10)`. The `rgb` function defines the intensities fro each of the display primary colors (on a scale of 0 to 1). The primary colors are red, green and blue. The forth value is optional and provides the fraction opaqueness with a value of 1 being completely opaque. 



```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     pch = 19, cex=2, 
     col = rgb (0,0,0,0.10)) # set up symbol types and their color  
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     pch = 19, cex=2, 
     col = rgb (1,0,0,0.10)) # set up symbol types and their color  
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-4-2.png" width="672" />

Now it is time to combine with this what we learned in the previous class. How to add different colors for the three different species. Remember the `ifelse`?


```r
col.iris<-ifelse(iris$Species=='setosa','purple',ifelse(iris$Species=='versicolor','blue','pink')) # create a vector of character with the name of the color we wanna use
col.iris
```

```
##   [1] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##   [9] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##  [17] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##  [25] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##  [33] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##  [41] "purple" "purple" "purple" "purple" "purple" "purple" "purple" "purple"
##  [49] "purple" "purple" "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [57] "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [65] "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [73] "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [81] "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [89] "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"   "blue"  
##  [97] "blue"   "blue"   "blue"   "blue"   "pink"   "pink"   "pink"   "pink"  
## [105] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"  
## [113] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"  
## [121] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"  
## [129] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"  
## [137] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"   "pink"  
## [145] "pink"   "pink"   "pink"   "pink"   "pink"   "pink"
```

You can insert this color vector in the argument `col` and controlling its opacity with the `alpha` function from the package `scales`. Accordingly, you can guess the argument `col` can accept a vector of character with just the name of color instead of a rgb code.


```r
library(scales)
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     pch = 19, cex=2, 
     col = alpha(col.iris, 0.2)) # set up symbol types and their color 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-6-1.png" width="672" />
 
We now must add a legend to our plot. Observe the conversion of the color list into a factor, and the extraction of levels in the `col` argument.


```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     pch = 19, cex=2, 
     col = alpha(col.iris, 0.2))

legend(x="bottomright", pch= 19, cex=1.5, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-7-1.png" width="672" />

The argument `las=1` will rotate y-axis labels. This is often a requirement for publication quality plots. 


```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     pch = 19, cex=2, las=1,
     col = alpha(col.iris, 0.2)) # set up symbol types and their color 

legend(x="bottomright", pch= 19, cex=1.5, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-8-1.png" width="672" />

The size of the main title, axis, labels is also control with `cex` arguments 


```r
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     cex.axis=1.0, cex.lab=1.5, cex.main=1.5,
     pch = 19, cex=2, las=1,
     col = alpha(col.iris, 0.2))

legend(x="bottomright", pch= 19, cex=1.5, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-9-1.png" width="672" />

The size of the points can be set as a proportion of a given continuous variable



```r
ratio<-iris$Petal.Length/iris$Sepal.Width  # ratio between the length of petal and the width of Sepal
plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     cex.axis=1.0, cex.lab=1.5, cex.main=1.5,
     pch = 19, las=1, cex= ratio * 2, # incude this ration in cex, multiply x2 to make it more visual
     col = alpha(col.iris, 0.2))
# looks like Sepal.Width is poorly informative in disriminating species

legend(x="bottomright", pch= 19, cex=1.5, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-10-1.png" width="672" />

The function `pairs` allows a quick examination of the relationship among variables of interest: a scatterplot matrix. 


```r
pairs(iris[1:4], pch=19, col = alpha(col.iris, 0.2))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-11-1.png" width="672" />

A conversion to a line plot is easily made by specifying the `type` of plot. Y


```r
iris<-iris[order(iris$Petal.Width),] # first order
blossom<-NULL
blossom$year <- 2010:2019                                               # 
blossom$count.alaska <- c(3, 1, 5, 2, 3, 8, 4, 7, 6, 9)
blossom$count.canada <- c(4, 6, 5, 7, 10, 8, 10, 11, 15, 17)
as.data.frame(blossom)
```

```
##    year count.alaska count.canada
## 1  2010            3            4
## 2  2011            1            6
## 3  2012            5            5
## 4  2013            2            7
## 5  2014            3           10
## 6  2015            8            8
## 7  2016            4           10
## 8  2017            7           11
## 9  2018            6           15
## 10 2019            9           17
```

```r
plot(count.alaska ~ year,dat = blossom, type='l',
      ylab = "No. of flower blossoming") 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-12-1.png" width="672" />

To plot both points and line, set the type parameter to "b" (for both). We’ll also set the point symbol to 20.


```r
plot(count.alaska ~ year,dat = blossom, type='b', pch=20,
      ylab = "No. of flower blossoming") 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-13-1.png" width="672" />

We can rotate the axis, increase the width of the line, change its type and color:  


```r
plot(count.alaska ~ year,dat = blossom, type='b', pch=20,
     lty=2, lwd=0.5, col='red',
     ylab = "No. of flower blossoming") 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-14-1.png" width="672" />


We can add the other variable by adding a line of another type and color:  

```r
plot(count.alaska ~ year,dat = blossom, type='b', pch=20,
     lty=2, lwd=0.5, col='red',
     ylab = "No. of flower blossoming") 
lines(count.canada ~ year,dat = blossom, type='b', pch=20,
     lty=3, lwd=0.5, col='blue')
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-15-1.png" width="672" />

The plot is a static object meaning that we need to define the axes limits before calling the original plot function. Both axes limits can be set using the `xlim` and `ylim` parameters. We don’t need to set the x-axis range since both variables cover the same year range. We will therefore only focus on the y-axis limits. We can grab both the minimum and maximum values for the variables `count.alaska` and `count.canada` using the `range` function, then pass the range to the `ylim` parameter in the call to plot.


```r
y.rng<-range(c(blossom$count.alaska, blossom$count.canada))

plot(count.alaska ~ year,dat = blossom, type='l', ylim = y.rng,
     lty=2, lwd=1, col='red',
     ylab = "No. of flower blossoming") 
lines(count.canada ~ year,dat = blossom,
     lty=1, lwd=1, col='blue')
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-16-1.png" width="672" />


Point plots from different variables can also be combined into a single plot using the `points` function in lieu of the `lines` function. Let's get back to our iris dataset and build our plot sequentially for two species.



```r
iris.ver<- subset(iris, Species == "versicolor")
iris.vir<- subset(iris, Species == "virginica")

y.rng <- range( c(iris.ver$Petal.Length, iris.vir$Petal.Length) , na.rm = TRUE) 
x.rng <- range( c(iris.ver$Petal.Width, iris.vir$Petal.width) , na.rm = TRUE) 

# Plot an empty plot

plot(Petal.Length ~ Petal.Width, dat = iris.ver,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     cex.axis=1.0, cex.lab=1.5, cex.main=1.5, type='n',
     xlim=x.rng,  ylim=y.rng)

# Add points for versicolor
points(Petal.Length ~ Petal.Width, dat = iris.ver, pch = 20,cex=2, 
       col = rgb(0,0,1,0.10))
       
# Add points for versicolor
points(Petal.Length ~ Petal.Width, dat = iris.vir, pch = 20,cex=2, 
      col =  alpha('#fc03c6', 0.2))

# Add legend
legend("topleft", c("versicolor", "virginica"), pch = 19, cex=1.2,
       col = c(rgb(0,0,1,0.10), alpha('#fc03c6', 0.2)))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-17-1.png" width="672" />


Note 1: that `na.rm=T` is added in the `range` function to prevent `NA` value in the data from returning an `NA` value in the range.

Note 2: You can define the color using the `rgb` function, or by color name such as `col = "red"` or `col = "bisque"`. For a full list of color names, type colors() at a command prompt. You can also the hexadecimal code of the color. Google "color picker". Color gradient can easily be created and personnalized [here](https://www.webfx.com/web-design/color-picker/).


### Boxplots

A boxplot is one of many graphical tools used to summarize the distribution of a data batch. The graphic consists of a “box” that depicts the range covered by 50% of the data (aka the interquartile range, IQR), a horizontal line that displays the median, and “whiskers” that depict 1.5 times the IQR or the largest (for the top half) or smallest (for the bottom half) values.

For example, we can summarize the width of the sepal for all individuals in our iris dataset:


```r
boxplot(iris$Sepal.Width, na.rm = TRUE)
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-18-1.png" width="672" />

Several variables can be summarized on the same plot.


```r
boxplot(iris$Sepal.Width,iris$Sepal.Length, iris$Petal.Width,iris$Petal.Length, names = c("Sepal.Width", "Sepal.Length", "Petal.Length","Petal.Width"), main = "Iris flower traits")
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-19-1.png" width="672" />

The `names` argument labels the x-axis and the `main` argument labels the plot title. The outliers can be removed from the plot, if desired, by setting the `outline` parameter to FALSE. The boxplot graph can also be plotted horizontally by setting the `horizontal` parameter to `TRUE`:


```r
boxplot(iris$Sepal.Width,iris$Sepal.Length, iris$Petal.Width,iris$Petal.Length, names = c("Sepal.Width", "Sepal.Length", "Petal.Length","Petal.Width"), main = "Iris flower traits",outline = FALSE, horizontal = TRUE )
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-20-1.png" width="672" />

Using a long version of a table (`pivot_longer`), the variable can be summarized by the levels of a factor, here the `Species` levels


```r
boxplot(Sepal.Width ~ Species,iris) 
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-21-1.png" width="672" />

We can `reorder` the boxplots based on the median value. By default, `boxplot` will order the boxplots following the factor’s level order.
        
 
 ```r
 iris$Species.ord <- reorder(iris$Species,iris$Sepal.Width, median)
 levels(iris$Species.ord)
 ```
 
 ```
 ## [1] "versicolor" "virginica"  "setosa"
 ```
 
 ```r
 boxplot(Sepal.Width ~ Species.ord, iris)
 ```
 
 <img src="Topic_3_plots_files/figure-html/unnamed-chunk-22-1.png" width="672" />
        
### Histograms

The histogram is another form of data distribution visualization. It consists of partitioning a batch of values into intervals of equal length then tallying their count in each interval. The height of each bar represents these counts. For example, we can plot the histogram of the width od sepal using the `hist` function as follows:
        

```r
hist(iris$Sepal.Width, xlab = "Width of Sepal", main = NA)
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-23-1.png" width="672" />

To control the number of bins add the `breaks` argument. The `breaks` argument can be passed different types of values. The simplest value is the desired number of bins. Note, however, that you might not necessarily get the number of bins defined with the `breaks` argument.


```r
hist(iris$Sepal.Width, xlab = "Width of Sepal", main = NA, breaks=10)
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-24-1.png" width="672" />

The documentation states that the breaks value “**is a suggestion only as the breakpoints will be set to `pretty` values**”. `pretty` refers to a function that rounds values to powers of 1, 2 or 5 times a power of 10.

If you want total control of the bin numbers, manually create the breaks as follows:     


```r
n <- 10  # Define the number of bin
minx <- min(iris$Sepal.Width, na.rm = TRUE)
maxx <- max(iris$Sepal.Width, na.rm = TRUE)
bins <- seq(minx, maxx, length.out = n +1)
hist(iris$Sepal.Width, xlab = "Width of Sepal", main = NA, breaks = bins)
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-25-1.png" width="672" />

### Density plot

Histograms have their pitfalls. For example, the number of bins can drastically affect the appearance of a distribution. One alternative is the density plot which, for a series of x-values, computes the density of observations at each x-value. This generates a “smoothed” distribution of values.

Unlike the other plotting functions, `density` does not generate a plot but a list object instead. But the output of `density` can be wrapped with a `plot` function to generate the plot.



```r
dens <- density(iris$Sepal.Width)
plot(dens, main = "Density distribution of the width of sepal")
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-26-1.png" width="672" />

You can control the bandwidth using the bw argument. For example:


```r
dens <- density(iris$Sepal.Width, bw=0.05)
plot(dens, main = "Density distribution of the width of sepal")
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-27-1.png" width="672" />

The bandwidth parameter adopts the variable’s units.

### QQ plot

This is an important plot called by the function `qqnorm` to examine the normality of a variable. A `qqline` can be added to represent a “theoretical”, by default normal, quantile-quantile plot.  


```r
qqnorm(iris$Sepal.Width)
qqline(iris$Sepal.Width)
```

> *<span style="color: green">**RP6**: Using our previous `rairuho` dataset, examine the relationship between the length of the plant at `day3` and the length at `day7`: create a scatterplot. Examine if the length at `day7` is normal: you will use `hist`, a `density plot and `qqnorm`. How `treatment`affect the length at day7: make a `boxplot`. Make a comparison of the lenght of the plant between each pair of days using `pairs`.</span>*.


```{.r .fold-hide}
rairuoho<-read.table('Data/rairuoho.txt',header=T, sep="\t", dec=".")
plot(day3 ~ day7,dat=rairuoho,
    xlab = 'Length at day 3', 
    ylab = 'Length at day 7', 
    main = 'Realtionship between the length at day 3 and day 7')
hist(rairuoho$day7)
dens.rai <- density(rairuoho$day7, bw=6)
plot(dens.rai, main = "Density distribution of thelength at Day 7")
qqnorm(rairuoho$day7)
qqline(rairuoho$day7)
boxplot(day7~treatment, data=rairuoho) 
pairs(rairuoho[,1:6])
```

## Graphical options

See the current graphical settings using:


```{.r .fold-show}
par() # graphical options
# also use change the current settings
```

The `par` function allows you to set up graphical parameters (for all coming plots)


```r
plot(Petal.Length ~ Petal.Width, dat = iris,las=1)
# `las` can be applied to all following plots by setting `par`

par(las=1) 
plot(Petal.Length ~ Petal.Width, dat = iris)
# it will apply to all following plots unless say otherwise.
```


**Some useful settings control by the function `par`:**

*Text and symbol size*

`cex` point and text size 

`cex.axis`  axis tick label size 

`cex.lab` axis label size

`cex.main` title size

`cex.sub` subtitle size 

*Plotting symbol (e.g., for scatter plots)*

`pch` plotting symbol (`pch=19` or `pch="*"`) 

*Lines (e.g., for line plots)*

`lty` line type (1=solid,2.6=dashed or dotted)
	
`lwd` line width 

*Margins (have to be setup before plotting)*

`mar` margins (in order: bottom, left, top, right)

*Panels (have to be setup before plotting)*

`mfrow` no. of rows and column, e.g. `par(mfrow=c(2,2))`


```r
par(mfrow=c(1,2))# define graphical parameter: 1 row, 2 columns
plot (1, 1, cex=15, pch=15) # 1st plot
plot (1, 1, cex=15, pch=19) # 2nd plot
```

*Axis range*

`xlim` min, max

`ylim` min, max


```r
par(bg="#FCE8C5", mar=c(4,4,4,4), pch = 19, las=1, cex=1.2, cex.main=1.2, cex.axis=1,cex.lab=1)

plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     col = alpha(col.iris, 0.2)) # set up symbol types and their color 

legend(x="bottomright", pch= 19, cex=0.8, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))
```

<img src="Topic_3_plots_files/figure-html/unnamed-chunk-33-1.png" width="672" />

We reinitialize the graphical parameters using `dev.off()'


```r
dev.off()
```

```
## null device 
##           1
```

Other elements to add in a plot:


- Title and labels`title`  


```r
title (main='title', ylab='y-axis title", xlab'x-axis title')
```


- Text annotation `text` or in the margine using `mtext`


```r
text (x=1, y=1,'text')
mtext ('text', side=1, line=1)
```


- Horizontal or vertical line `abline`


```r
abline (h=1)
abline (v=1)
```

`line`, `points`, `legend`, etc. already mentioned



## Exporting plot

You might need to export your plots as standalone image files for publications. R will export to many different raster image file formats such as jpg, png, gif and tiff, and several vector file formats such as PostScript, svg and PDF. You can specify the image resolution (in dpi), the image height and width, and the size of the margins.

The following example saves the last plot as an uncompressed tiff file with a 5“x6” dimension and a resolution of 300 dpi. This is accomplished by simply book-ending the plotting routines between the `tiff` and `dev.off`functions:



```r
tiff(filename = "Output/iris_plot.tif", width = 5, height = 6, units = "in", compression = "none", res = 300)

par(bg="#FCE8C5", mar=c(4,4,4,4), pch = 19, las=1, cex=1.2, cex.main=1.2, cex.axis=1,cex.lab=1)

plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     col = alpha(col.iris, 0.2)) # set up symbol types and their color 

legend(x="bottomright", pch= 19, cex=0.8, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))

dev.off()
```

```
## png 
##   2
```


To save the same plot to a pdf file format, simply substitute `tiff` with `pdf` and adjust the parameters as needed:


```r
pdf(file= "Output/iris_plot.pdf", width = 5, height = 6)

par(bg="#FCE8C5", mar=c(4,4,4,4), pch = 19, las=1, cex=1.2, cex.main=1.2, cex.axis=1,cex.lab=1)

plot(Petal.Length ~ Petal.Width, dat = iris,
     xlab = 'Petal width (cm)', 
     ylab = 'Petal length (cm)', 
     main = 'Petal width and length of iris flower',
     col = alpha(col.iris, 0.2)) # set up symbol types and their color 

legend(x="bottomright", pch= 19, cex=0.8, legend= c("versicolor","setosa", "virginica"), col=levels(as.factor(alpha(col.iris, 0.2))))

dev.off()
```

```
## png 
##   2
```


> *<span style="color: green">**RP7**: Take back the previous scatterplot produced on the rairuoho dataset. Make it informative by adding as many elements you think it need and customizing graphical options. Make the y-axis staring at **exactly** 0 and and end at 130. Save the plot as a `pdf` ready for publication.</span>* 

## The `lattice` package

*pending*

## The `ggplot` package

*pending*













