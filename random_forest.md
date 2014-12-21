# load data
data=read.csv("pml-training.csv",header=TRUE)
# delete all columns where the first value is NA
data=data[grep(FALSE,is.na(data[1,]))]
# delete all columns where the first value is empty
data=data[grep(FALSE,data[1,]=="")]
# load and clean test data in the similar way
test=read.csv("pml-testing.csv",header=TRUE)
test=test[grep(FALSE,is.na(test[1,]))]
test=test[grep(FALSE,test[1,]=="")]
# 
# check if all features in training data are contained in testing data
# 
testnames = names(test)
datanames = names(data)
datanames %in% testnames
# load caret library
library(caret)
# divide dataset into training (70%) and testing (30%)
inTrain <- createDataPartition(y=data$classe,p=0.7,list=FALSE)
training <- data[inTrain,]
testing <- data[-inTrain,]
# train the model, but skip 1st column, since it contains row number
model <- train(classe~., data = training[,-1], method="rf",prox=FALSE)
# confusion matrix
confusionMatrix(testing$classe,predict(model,testing))
# prediction
predict(model,testing)
