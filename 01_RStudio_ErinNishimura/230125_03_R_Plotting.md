

# Part 3 - Functions, Import/Export, & Plotting

January 25, 2023

## Lessons for today

  * [Functions - the verbs of the R language](#functions---the-verbs-of-the-r-language)
  * [Importing data](#import-and-export-of-data)
  * [Plotting](#plotting)
  * [Packages](#Packages)

## Learning Goals

  * Students will execute a few basic R functions
  * Students will learn to import and export data
  * Students will gain experience in basic plotting
  
## Useful references

 * [Base R cheat sheet](https://iqss.github.io/dss-workshops/R/Rintro/base-r-cheat-sheet.pdf)
 * [Basic lessons on R & R plotting](https://www.w3schools.com/r/r_graph_plot.asp)
 * [The R Graph Gallery](https://www.r-graph-gallery.com/index.html)
 * [Learn more with swirl](https://swirlstats.com/)

	
----

# Functions - the verbs of the R language

For this lesson, let's start with our same vectors as before. If you don't have them in your environment already, load them like so...

```r
organism <- c("human", "mouse", "worm", "yeast", "maize")
kingdom <- c("animalia", "animalia", "animalia", "fungi", "plantae")
kingdom <- as.factor(kingdom)
chromosomes <- c(23, 20, 5, 16, 10)
haploid <- c(FALSE, FALSE, FALSE, TRUE, FALSE)
model_systems <- data.frame(organism, chromosomes, kingdom, haploid)
str(model_systems)
```

By now, you are already experienced in calling R functions. Here are some examples:

```r

date()

dim(model_systems)

mean(chromosomes)

```

Functions typically involve parentheses. Sometimes these parentheses are empty and sometimes we type something in them. Think of this similar to how some verbs take direct objects (I threw the ball) and some don't (I run). 

Two different types of information can go in the parentheses: **arguments** and **options**.

  * **ARGUMENTS**: These are object names that are added inside the parentheses. They are what the function will operate on. They are analogous to direct objects in English. 
  * **OPTIONS**: Think of these as adverbs. These are optional content you can add that changes **how** the function will operate.

The `help()` function gives us information about how a function operates. The help page will tell us
   * What a function does
   * Whether it requires an argument
   * what object classes are allowed as arguments
   * The potential list of options
   * The default values associated with each option

➡️ Try the help function

```r
help(dim)
```

  * The `x` in the example tells you that this function takes an argument. If we read under **ARGUMENTS** we can learn whith object classes are allowed.

➡️ Let's look at the help menu for `mean`.

```r
help(mean)
```

This help menu also specifies the **options** that the mean function takes and their **default** values. As a default, trim is set to 0. In other words, all the values are used to calculate the mean. However, this value can be changed to 0.2, in which case, the most extreme 20 % of all datapoints will be removed before the mean is calculated. 

➡️ Give it a try:

```r
mean(chromosomes, trim = 0.2)
```

⚠️ **GRAPHICAL SUMMARY** 

<img src="webContent/WebContent_Powerpoint_functionGrammar.jpg" width="600">


# Importing Data - Acquiring Data

So far, we've created objects by assignment expressions that directly specify their values. Next, we'll learn how to **import** data into R through an special assignment expression.

First, let's download a dataset to import. 

❗**EXERCISE** Download a data file

  * Go to [US_COVID_Vacc_by_StateTerr.csv](https://drive.google.com/file/d/1_0z0Bi3cg5GjWpz6FV23NMFw8V0kzQm_/view?usp=sharing)
  * ➡️ Download the file by clicking the down arrow in the top right set of icons.
  * ➡️ Save the file somewhere in your Documents directory/folder that makes sense. Consider adding it to your folder called "CMB580" that contains this code. 
  * ➡️ Ensure the file name saved is "US_COVID_Vacc_by_StateTerr.csv" by viewing it in Finder or explorer.

# Importing Data - Setting the working directory

To import the file into R, we first need to sync up where R **thinks** it is working on your computer with the folder that contains a document you want to import. This is a bit tricky and will require some knowledge of **paths**.

**Paths** - a path describes the location of a folder or file on your computer. Because folders are nested on a computer, the path will start on the left with the "topmost" or "outer most" directory, and then progressively list different sub-directories. 

In R, folder names are separated by a "/" character.

To determine where R "thinks it is" on your computer, use the command `getwd()` for **get working directory**. **directory** is a more computer science-y term for "folder".

```r
getwd()
```

The output is listed as a path. Notice how the output to getwd() matches with the folder names at the bottom of the "Finder" window and with the different folder icons.

<img src="webContent/Screen Shot 2023-01-23 at 9.06.12 AM.png" width="600">

:heavy_exclamation_mark: MAC tip: If you don't see you path in the Finder, pull down the View menu and select Show Path Bar.

:heavy_exclamation_mark: PC tip: If you don't see your path in the Explorer, follow [these directions](https://pureinfotech.com/show-full-path-file-explorer-windows-10/)



❗**EXERCISE** Setting the working directory

To import a file, we need to set R's working directory to match the directory where our file lives.

  * ➡️ Go to the **Files** Panel of RStudio.
  * ➡️ Navigate to the location containing the downloaded dataset (may take some sleuthing)
  * ➡️ Change the working directory by going to the **Files Menu Banner**, selecting **More**, and selecting **Set As Working Directory**
  * ➡️ For posterity, copy and paste the command line that appears on the console that looks like `setwd(/directory/directory/)` into your .R script for next time

# Importing Data - Reading

❗**EXERCISE** Together, let's write the code to read the data file into R.

We will use the command `read.table()` to import the dataset

```r
# Check we're in the right place
getwd() 

# Check how read.table is used
help(read.table)

# Look at the data using read.table
read.table("US_COVID_Vacc_by_StateTerr.csv", sep = ",", header = TRUE)

# Actually, I don't like those number row names
read.table("US_COVID_Vacc_by_StateTerr.csv", sep = ",", header = TRUE, row.names = "location")

# That only printed out the data from the file, it didn't capture it.
# To capture the data, use an assignment expression:
VaxByState <- read.table("US_COVID_Vacc_by_StateTerr.csv", sep = ",", header = TRUE, row.names = "location")

:+1: Use help(read.table) to learn how you can also use read.csv or read.csv2 to upload comma separated content, also! There are many ways to do the same thing in R.


```

# Importing Data - EDA (Exploratory Data Analysis)

⚠️ **BEST PRACTICES** It is a wise idea to inspect your data once you have read it into R.

➡️ Look at what you have acquired and make sure everything looks good!

```r
dim(VaxByState)
str(VaxByState)
class(VaxByState)
```

## Obtaining, Cleaning, Wrangling, & Munging

I obtained the data from [Our World In Data](https://ourworldindata.org/). This is a great resource for worldwide statistics. I use this site because their data is **clean**. What do I mean by that?

  * headers don't contain spaces
  * no blank fields. Missing fields are labeled "NA"
  * no weird characters

One thing you will discover is that most datasets are NOT clean. It takes A LOT of ground work to make your data nice and neat and tidy. This ground work is called either **cleaning**, **wrangling**, or **munging** data, depending on your frustration level. 

I had to clean up this data quite a bit to make the neat and tidy file you just imported.
 
  * filtered for the most recent dates
  * removed superfluous columns
  * re-arranged the columns
  * removed data for US federal prisons, Defense Dept., and Veteran's hospitals because some of their data was missing.



## Plotting

  * Plotting and graphics are really what make R special. R graphics can handle a large amount of data and packages extend base R into capabilities that are very impressive.
  * [The R Graph Gallery](https://www.r-graph-gallery.com/index.html) has some impressive examples.
  * To be honest, the graphics in base R are pretty clunky and tricky to modify. For today, I'll show you how to make a simple R plot using the scottish_towns dataset.

**X Y SCATTER PLOT** The function `plot()` uses the following syntax to plot x and y values into a scatter plot:

<img src="webContent/WebContent_Powerpoint_plotting.jpg" width="800">

The result looks like this:

<img src="webContent/Rplot.jpeg" width="500">

❗ **EXERCISE: Basic R Plot

➡️ Let's try it! Say we want to see whether there is a relationship between the overall vaccination rates and the booster rates for different states. From first principles, we would predict that states with overall high vaccination rates also likely had high booster rates and vice versa. However, there may be interesting places where the initial rates of vaccination were high, but then the population did not boost at high levels. It would be interesting to find those states/territories.

```r
# We can use integer or numeric or integer classes as input
str(VaxByState)

# Use colnames to see the options for data we have available:
colnames(VaxByState)

# We'll use these x-values...
VaxByState$people_vaccinated_per_hundred

# ... and these y-values:
VaxByState$total_boosters_per_hundred

# Plot
plot(VaxByState$people_vaccinated_per_hundred, VaxByState$total_boosters_per_hundred)

# Let's add some labels
plot(VaxByState$people_vaccinated_per_hundred, VaxByState$total_boosters_per_hundred)
text(vax_vector, boost_vector, rownames(VaxByState),col='darkblue', pos = 4, cex = 0.8)
```

<img src="webContent/Screen Shot 2023-01-25 at 8.53.36 AM.png" width="500">



# Ready to get fancy? 

The following is just a demonstration of what you can do to add some details to the plot function. 

```r
# this is just a demonstration
plot(vax_vector,
     boost_vector,
     col = "lightblue",
     pch = 19,
     xlim = c(20,130),
     ylim = c(20,130),
     main = "Vaccination rates and boost rates per state or territory",
     ylab = "Boosters administered per hundred people",
     xlab = "Vaccinations administered per hundred people",
     )

# Label the states and territories
text(vax_vector, boost_vector, rownames(VaxByState),col='darkblue', pos = 4, cex = 0.8)

# Add a linear regression trendline
lm(boost_vector ~ vax_vector)
abline(lm(boost_vector ~ vax_vector ), col = "pink")
text(120,95, "linear regression", col = "pink")


# Fit the data using local smoothed fit (Lowess)
lowess(boost_vector ~ vax_vector)
lines(lowess(boost_vector ~ vax_vector), col = "purple")
text(115,78, "local fit", col = "purple")
```
<img src="webContent/Screen Shot 2023-01-25 at 8.45.09 AM.png" width="600">

## Saving plots

To save a plot, we can simply use the **Export** menu item on the top of the Plots panel. 

➡️ Export your plot as a .pdf...

<img src="webContent/Screen Shot 2023-01-25 at 8.48.24 AM.png" width="500">

➡️ Specify the width and height of the output image. Give it a name. Save it in a location that makes sense. You may need to play around with the width/height until the plot looks nice...

<img src="webContent/Screen Shot 2023-01-25 at 8.48.40 AM.png" width="500">

----

# Shameless plug

  * Please consider joining me in the Fall semester for a 3-module course on Computational Biology:
    * DSCI510 - Linux as a computational platform (no pre-requisites)
    * DSCI511 - Python Programming (DSCI510 or previous experience in Linux is a pre-requisite)
    * DSCI512 - RNA sequencing data analysis (DSCI510 or previous experience in Linux is a pre-requisite)


# Thanks, everyone! And keep coding!

▶️ [Part 4 - Bonus Content](230125_04_R_BonusContent.md)