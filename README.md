# DissertationHighLevel
---
title: "Jags-Ymet-XmetSsubj-MrobustHierDisExample"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Need to create a version of the data that secondary with 1's and 2's instead of one's and zeros
```{r}
setwd("~/Desktop/QualData")
selRegressBayesSmallSample = read.csv("selRegressBayesSmallSample.csv", header = TRUE)

Secondary = selRegressBayesSmallSample$Secondary
Secondary = as.data.frame(Secondary)
Secondary = apply(Secondary, 2, function(x){ifelse(x == 1, 2, 1)})
Secondary = as.data.frame(Secondary)

selRegressBayesSmallSample = selRegressBayesSmallSample[c("Men", "Experience", "AfricanAmerican", "SELSVScore")]

selRegressBayesSmallSampleDis = cbind(selRegressBayesSmallSample, Secondary)

head(selRegressBayesSmallSampleDis)

write.csv(selRegressBayesSmallSampleDis, "selRegressBayesSmallSampleDis.csv")

```


Here is the version for my dissertation
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHierDis.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Desktop/QualData")
myData = read.csv( file="selRegressBayesSmallSampleDis.csv" )
xName = "Men" ; x2Name = "Experience"; x3Name = "AfricanAmerican"; yName = "SELSVScore" ; sName="Secondary"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
source("Jags-Ymet-XmetSsubj-Dicot-MrobustHierExtendDis.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)


```

