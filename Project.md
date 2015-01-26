###Practical Machine Learning Project

###Summary - 

#####This is a class project for Practical Machine Learning.  We are trying to classify the correctness of barbell excercise technique using data from fitness gadgets.  The response variable is ordinal in nature with 5 levels (A-E).  A training data set with approx 20,000 records will be used to build a final prediction model to be applied to a data set with 20 records.

#####NOTE: due to lengthy processing times the models did not complete, code is below in comments though

####Load Libraries

```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 2.15.3
```

```
## Loading required package: lattice
```

```
## Warning: package 'lattice' was built under R version 2.15.3
```

```
## Loading required package: ggplot2
```

```r
library(lattice)
library(rattle)
```

```
## Warning: package 'rattle' was built under R version 2.15.3
```

```
## Rattle: A free graphical interface for data mining with R.
## Version 2.6.26 r77 Copyright (c) 2006-2013 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
```

```r
library(C50)
```

```
## Warning: package 'C50' was built under R version 2.15.3
```


####Load data sets

```r
training1 <- read.csv("pml-training.csv", na.strings = c("NA", ""), header = TRUE)
testing1 <- read.csv("pml-testing.csv", na.strings = c("NA", ""), header = TRUE)
```


####Data Cleaning
######Remove the X (ID), username and TimeStamp variables

```r
training2 <- training1[, -1:-5]
testing2 <- testing1[, -1:-5]
```


#####Rename "problem_id"" in testing data frame to "classe".

```r
rename(testing2, c(problem_id = "classe"))
```

```
## Error: could not find function "rename"
```


#####Ientify of variables with NAs (missing) >= 60%

```r
columns_ge_60pct_NA <- vector()
vectorCounter <- 0
for (i in 1:ncol(training2)) {
    if ((sum(is.na(training2[, i]))/nrow(training2)) >= 0.6) {
        vectorCounter <- vectorCounter + 1
        columns_ge_60pct_NA[vectorCounter] <- i
    }
}
```


#####Remove variables with NAs (missing) >= 60%

```r
training3 <- training2[, -columns_ge_60pct_NA]
testing3 <- testing2[, -columns_ge_60pct_NA]
```


#####Identify variables with Near Zero Variance

```r
nsvColumns <- nearZeroVar(training3)
```


#####Remove variables with Near Zero Variance.

```r
training <- training3[, -nsvColumns]
testing <- testing3[, -nsvColumns]
```


#####The Training data frame will get split further (70/30) because the final Testing data frame will have only 20 records.  20 records would be insufficient for preventing the over-fitting of the models.  Final data frames will be:
######1 - train - 70% of training data set
######2 - test - 30% of training data set
######3 - testing - 20 record final testing data set

```r
inTrain <- createDataPartition(training$class, p = 0.7, list = FALSE)
train <- training[inTrain, ]
test <- training[-inTrain, ]
```


#####Cleanup RAM

```r
rm(testing1)
rm(testing2)
rm(testing3)
rm(training)
rm(training1)
rm(training2)
rm(training3)
```


####Model Development
#####Tree - rf (Random Forest)

```r
# mod_rf <- train(classe~.,data=train,method='rf') print(mod_rf)
# plot(mod_rf)
```


#####Tree - C5

```r
# mod_c5 = readRDS('mod_c5.RDS') mod_c5 <-
# train(classe~.,data=train,method='C5.0') print(mod_c5) plot(mod_c5)
```


#####Tree - rpart (CART)

```r
# mod_rpart = readRDS('mod_rpart.RDS') mod_rpart <-
# train(classe~.,data=train,method='rpart') print(mod_rpart)
# fancyRpartPlot(mod_rpart)
```


#####Neural Net - nnet

```r
# mod_nnet <- train(classe~.,data=train,method='nnet') print(nnet)
```


#####Save models to disk to save rebuilding

```r
# saveRDS(mod_rf, 'mod_rf.RDS') saveRDS(mod_c5, 'mod_c5.RDS')
# saveRDS(mod_rpart, 'mod_rpart.RDS') saveRDS(mod_nnet, 'mod_rf.RDS')

```



####Model Evalution
#####Out of Sample Error:  Calculate Accuracy Ratio on train and test

```r
# mean(predict(model_rf, train) == train$classe) * 100
# mean(predict(model_rf, test) == test$classe) * 100

# mean(predict(model_c5, train) == train$classe) * 100
# mean(predict(model_c5, test) == test$classe) * 100

# mean(predict(model_rpart, train) == train$classe) * 100
# mean(predict(model_rpart, test) == test$classe) * 100

# mean(predict(model_nnet, train) == train$classe) * 100
# mean(predict(model_nnet, test) == test$classe) * 100
```




###Conclusion
#####The models did not complete but the code above will build 4 models where a champion can be selected using Accuracy Ratio.

####Confusion Matrix

```r
# predictions <- predict(mod_rpart,newdata=test)
# print(confusionMatrix(predictions,test$classe))
```



####Submission Results:

```r
# print(predict(mod_rpart,newdata=testing))
```



