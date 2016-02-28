Below, I describe the variables, the data, transformations & work done to complete the final assignment.

I used the help of the following packages to complete this assignment:

library(plyr)
library(tidyr)
library(dplyr)
library(stringr)


First, I read in the Training set and the Test set using read.table()  These two files contain data from the training set group and the test group.  
x_train <- read.table("X_train.txt")
x_test <- read.table("X_test.txt")

I Read the Features file into R next
features <- read.table("features.txt")

The features_info.txt file indicates the following values to to represent mean and standard deviation:
#         mean(): Mean value
#         std(): Standard deviation
Since these values don't appear past row 554 of the features.txt file, I selected the first 554 rows of the features set to be used as column names of the merged dataset.  

features <- features[1:554, ]

I also selected only the first 554 columns from the x_test and x_train groups.  The trimmed data do not contain mean() or std() values.

x_train <- x_train[, 1:554]
x_test <- x_test[, 1:554]

I changed the V2 variable of features to class character and selected the row values of features to use as descriptive variable names for the dataset

features$V2 <- as.character(features$V2)
variableNames <- features$V2

I appropriately labeled the training set and test set with the descriptive variable names from the features file:
colnames(x_train) <- variableNames
colnames(x_test) <- variableNames


Next, I used the grep() function to create objects that indicate the columns representing mean and standard deviation.

means <- features[grep(".mean", features$V2), ]
devs <- features[grep(".std", features$V2), ]

I used bind_rows() function to combine the objects representing the mean and std columns

vars <- bind_rows(means, devs) %>% arrange(V1)

Create an integer vector to select the columns for mean and std

cols <- vars$V1

I then extract only the measurements on the mean and standard deviation for each measurement.

x_test <- x_test[, cols]
x_train <- x_train[, cols]

I removed objects no longer needed to clear the workspace

rm(features, means, devs, vars, cols, variableNames)

Next, I read the subject files indicating the subject producing the data

sub_train <- read.table("subject_train.txt")
sub_test <- read.table("subject_test.txt")


I renamed the columns in the above subject files with the incredibly descriptive variable name: "Subject"

colnames(sub_train) <- "Subject"
colnames(sub_test) <- "Subject"


Next, I used cbind() to bind the column "Subject" to the Test & Training files

x_test <- cbind(sub_test, x_test)
x_train <- cbind(sub_train, x_train)


Again, remove files to clean the workspace
rm(sub_train, sub_test)


I read the y train & y test files in next.  These are the training group labels.

y_train <- read.table("y_train.txt")
y_test <- read.table("y_test.txt")


I set column names with another very descriptive variable name: Activity

colnames(y_train) <- "Activity"
colnames(y_test) <- "Activity"

I changed the activity to a factor and used the descriptive variable names provided.

y_train$Activity <- factor(y_train$Activity, levels = c(1,2,3,4,5,6),
                  labels = c("Walking", "Walking_Upstairs","Walking_Downstairs","Sitting",
                             "Standing","Laying"))

y_test$Activity <- factor(y_test$Activity, levels = c(1,2,3,4,5,6),
                     labels = c("Walking", "Walking_Upstairs","Walking_Downstairs","Sitting",
                                "Standing","Laying"))


I used bind_cols() to add the Activity column to the Training & Test sets

x_train <- bind_cols(y_train, x_train)
x_test <- bind_cols(y_test, x_test)

Once again, I removed objects no longer needed in the workspace

rm(y_test, y_train)

I added new columns indicating the Testing or training groups and labeled with undeniably descriptive variable names.

x_test$Test_or_Train_Group <- "Test"
x_train$Test_or_Train_Group <- "Train"

Finally, I merged the training and the test sets to create one data set.

merged_data <- rbind(x_test, x_train)

To finish, I creates a second, independent tidy data set, TidyData, with the average of each variable for each activity and each subject.

tidyData <- ddply(merged_data, .(Subject, Activity), function(x) colMeans(x[, 3:81]))


To conclude, I used write.table() to make the tidy dataset a text file.
write.table(tidyData, file = "TidyDataset.txt", row.names = FALSE)







