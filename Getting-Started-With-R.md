Getting Started With R
================

## A few notes and tips

##### R is a base language which allows you to load in a variety of packages that have different functions. For instance, if I needed to load in an SPSS (.sav) dataset, I would need to load in a package called foreign so that R will be able to read the .spv file. There are hundreds of packages, but for each class example or lab that is covered I will list the packages that are required for performing certain functions.

##### Packages are loaded using this code format:

> library(packagename)

##### If you are just starting out with R, you may need to install a lot of these packages - but as soon as you install them once you will always be able to load them in without reinstalling. You will always need to load in packages for use in different R files, though. Install packages by using this code format:

> install.packages(packagename)

##### I prefer to use R Markdown rather than R script when I use R. Play around with the different types of R files you can make and see what works for you!

##### While you’re starting out I do highly recommend R Markdown because you can create separate ‘chunks’ of code and run one or more at a time. (create a chunk by pressing command + option + i)

## Setting up your working directory

##### R needs to know where to look for datasets that you may be loading in - when you specify your working directory you are telling it where to look for your files!

##### I like to have one folder on my computer where I drop all of my data files so I don’t need to worry about all of the various different directories I would have. For example, my typical working directory is “/Users/samdutton/Desktop/7010Data/” which means I have a folder on my desktop called 7010Data where R can then look for the data files I ask it to retrieve.

##### Setup note: include this code in a chunk while your R file is under “markdown”, changing the \*\*\* to your working directory

> knitr::opts\_chunk$set(echo = TRUE)

> knitr::opts\_knit$set(root.dir = ’\*\*\*’)

## Accessing data files

##### To access SPSS files (.sav), use this code:

> library(foreign)

> SAVdataset &lt;- read.spss(“Lab1Data1sample.sav”, to.data.frame = T)

##### To access CSV files (.csv), use this code:

> CSVdataset &lt;- read.csv(“Lab1Data1sample.csv”, to.data.frame = T)

##### Once R loads your dataset it will appear as an *object* in your *environment*. This is typically located in the top right corner of the Rstudio window. If you click on that object in your environment R will open your dataset so you can take a peek at it! Looking at the code just above, SAVdataset and CSVdataset would be objects because you use this symbol " &lt;- " to *assign* " read.csv(“Lab1Data1sample.csv”, to.data.frame = T) " to that name. For more on this, you can email me or use an online resource to find examples of how you can play with objects! One of my favorite resources is <https://bookdown.org/ndphillips/YaRrr/>
