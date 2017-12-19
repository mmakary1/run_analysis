==================================================================
Human Activity Recognition Using Smartphones Dataset
Version 1.0
==================================================================
Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Università degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws
==================================================================

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

Summary of variables: 

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

write.table(run_analysis1,"run_analysis.txt", row.name= FALSE