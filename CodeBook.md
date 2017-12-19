library(dplyr)
y_train <- read.table("train/y_train.txt", quote="\"")
y_test <- read.table("test/y_test.txt", quote="\"")

features <- read.table("features.txt", quote="\"")
activity_labels <- read.table("activity_labels.txt", quote="\"")

subject_train <- read.table("train/subject_train.txt", quote="\"")
subject_test <- read.table("test/subject_test.txt", quote="\"")

X_train <- read.table("train/X_train.txt", quote="\"")
X_test <- read.table("test/X_test.txt", quote="\"")



- 70% of the Volunteer select analysis to obtain training data:

colnames(activity_labels)<- c("V1","Activity")



- y_train with the activity label merge:

subject<- rename(subject_train, subject=V1)
train<- cbind(y_train,subject)
trainmrg<- merge(train,activity_labels, by=("V1"))




- dataframe names for X_train from features:

colnames(X_train)<- features[,2]



- y_train, X_train and activity labels binding:

NTrain <- cbind(trainmrg,X_train)


- remove the first column from trainN variable: 

NNTrain<- NTrain[,-1]


- columns that contain means and std are selected:

NNNTrain <- select(NNTrain,contains("subject"), contains("Activity"), contains("mean"), contains("std"))




- 30% of the Volunteer select to obtain the test data:

colnames(activity_labels)<- c("V1","Activity")


- y_test with the activity label binding:


subject1<- rename(subject_test, subject=V1)
testd <- cbind(y_test,subject1)
testd1 <- merge(testd,activity_labels, by=("V1"))



- data frame names for X_test from features:

colnames(X_test)<- features[,2]


- y_test, X_test, and activity labels binding: 

testd2<- cbind(testd1,X_test)


- remove the first column from testd2 variable: 
testd3 <- testd2[,-1]


- columns that contain means and std are selected:

testd4 <- select(testd3,contains("subject"), contains("Activity"), contains("mean"), contains("std"))


- Bind Train and Test data:

run_analysis <- rbind(NNNTrain,testd4)


- summerize output:

run_analysis1 <- (run_analysis%>%
                        group_by(subject,Activity) %>%
                        summarise_all(funs( mean)))

- write output:

write.table(run_analysis1,"run_analysis.txt", row.name= FALSE)

