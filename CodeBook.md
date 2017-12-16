library(dplyr)
y_train <- read.table("train/y_train.txt", quote="\"")
y_test <- read.table("test/y_test.txt", quote="\"")

features <- read.table("features.txt", quote="\"")
activity_labels <- read.table("activity_labels.txt", quote="\"")

subject_train <- read.table("train/subject_train.txt", quote="\"")
subject_test <- read.table("test/subject_test.txt", quote="\"")

X_train <- read.table("train/X_train.txt", quote="\"")
X_test <- read.table("test/X_test.txt", quote="\"")


colnames(activity_labels)<- c("V1","Activity")

subject<- rename(subject_train, subject=V1)
train<- cbind(y_train,subject)
trainmrg<- merge(train,activity_labels, by=("V1"))

colnames(X_train)<- features[,2]

NTrain <- cbind(trainmrg,X_train)
NNTrain<- NTrain[,-1]

NNNTrain <- select(NNTrain,contains("subject"), contains("Activity"), contains("mean"), contains("std"))

colnames(activity_labels)<- c("V1","Activity")

subject1<- rename(subject_test, subject=V1)
testd <- cbind(y_test,subject1)
testd1 <- merge(testd,activity_labels, by=("V1"))

colnames(X_test)<- features[,2]

testd2<- cbind(testd1,X_test)

testd3 <- testd2[,-1]

testd4 <- select(testd3,contains("subject"), contains("Activity"), contains("mean"), contains("std"))

run_analysis <- rbind(NNNTrain,testd4)


run_analysis1 <- (run_analysis%>%
                        group_by(subject,Activity) %>%
                        summarise_all(funs( mean)))

write.table(run_analysis1,"run_analysis.txt", row.name= FALSE)