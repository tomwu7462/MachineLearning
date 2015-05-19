---
title: "MachingLearning"
author: "Sang Hua"
date: "Monday, May 18, 2015"
output: html_document
---

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```{r}
library(caret)
setwd("~/MachineLearning")
#pml.training <- read.csv("~/MachineLearning/pml_training.csv",header=TRUE, sep=",")
#pml.testing <- read.csv("~/MachineLearning/pml_testing.csv",header=TRUE, sep=",")
inTrain <- createDataPartition(y=pml_training$classe, p=0.60, list=FALSE)
training <- pml_training[inTrain,]
testing <- pml_training[-inTrain,]

train_num1 <- subset(training,select=c(classe,magnet_belt_y,magnet_belt_z,pitch_arm,total_accel_arm,accel_arm_x,magnet_arm_x,magnet_arm_y,
                                      magnet_arm_z,accel_dumbbell_x,magnet_dumbbell_z,pitch_forearm,total_accel_forearm,accel_forearm_x,
                                      magnet_forearm_x,magnet_forearm_y))

modFit <- train(classe ~ ., method="rpart",data=train_num1)
print(modFit$finalModel)

plot(modFit$finalModel, uniform=TRUE, main="Classification Tree")
text(modFit$finalModel, use.n=TRUE, all=TRUE, cex=0.8)

predictA <- predict(modFit, newdata=testing)
table(predictA,testing$classe)

predictTesting <- predict(modFit,newdata="~/MachineLearning/pml_testing"")


```

You can also embed plots, for example:

```{r, echo=FALSE}
plot(cars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
