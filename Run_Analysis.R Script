library(plyr)
library(tidyr)
library(dplyr)
library(stringr)

# You should create one R script called run_analysis.R that does the following.

# 1.      Merges the training and the test sets to create one data set.
# 2.      Extracts only the measurements on the mean and standard deviation for each measurement.
# 3.      Uses descriptive activity names to name the activities in the data set
# 4.      Appropriately labels the data set with descriptive variable names.
# 5.      From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

# Good luck!
  

#Read in the Training set and the Test set
x_train <- read.table("X_train.txt")
x_test <- read.table("X_test.txt")

#Read the Features file
features <- read.table("features.txt")

#  The features_info.txt file indicates the following values to
#  to represent mean and standard deviation:
#         mean(): Mean value
#         std(): Standard deviation
# Since these values don't appear past row 554 of the features.txt file, 

# select the first 554 rows of the features set.  
# These will be the column names of the merged dataset
features <- features[1:554, ]

# select only first 554 columns from the x_test and x_train groups
x_train <- x_train[, 1:554]
x_test <- x_test[, 1:554]

#Change V2 to class character 
features$V2 <- as.character(features$V2)

#Select the rows features to use as descriptive variable names
variableNames <- features$V2

# Appropriately label the data set with descriptive variable names:
colnames(x_train) <- variableNames
colnames(x_test) <- variableNames


# Find the columns representing mean and standard deviation
means <- features[grep(".mean", features$V2), ]
devs <- features[grep(".std", features$V2), ]

#Bind Rows
vars <- bind_rows(means, devs) %>% arrange(V1)

#Create an integer vector to select columns
cols <- vars$V1


# 2. Extracts only the measurements on the mean and standard deviation for each measurement.

#Subset to only mean and std columns
x_test <- x_test[, cols]
x_train <- x_train[, cols]

#Remove objects no longer needed
rm(features, means, devs, vars, cols, variableNames)

#Read the subject files
sub_train <- read.table("subject_train.txt")
sub_test <- read.table("subject_test.txt")



#Rename the column with descriptive variable name: "Subject"
colnames(sub_train) <- "Subject"
colnames(sub_test) <- "Subject"


#Bind the column "Subject" to the Test & Training files
x_test <- cbind(sub_test, x_test)
x_train <- cbind(sub_train, x_train)


#Remove the subject files
rm(sub_train, sub_test)


#Read y train & y test: the training labels.
y_train <- read.table("y_train.txt")
y_test <- read.table("y_test.txt")


#Set column name with descriptive variable name: Activity
colnames(y_train) <- "Activity"
colnames(y_test) <- "Activity"


# Change to a Factor.  Use the descriptive variable names provided.
y_train$Activity <- factor(y_train$Activity, levels = c(1,2,3,4,5,6),
                  labels = c("Walking", "Walking_Upstairs","Walking_Downstairs","Sitting",
                             "Standing","Laying"))

y_test$Activity <- factor(y_test$Activity, levels = c(1,2,3,4,5,6),
                     labels = c("Walking", "Walking_Upstairs","Walking_Downstairs","Sitting",
                                "Standing","Laying"))


#Bind the Activity column to the Training & Test sets
x_train <- bind_cols(y_train, x_train)
x_test <- bind_cols(y_test, x_test)

#Remove the training labels
rm(y_test, y_train)

# Add new columns indicating the Testing or training group
# label the data set with descriptive variable names.
x_test$Test_or_Train_Group <- "Test"
x_train$Test_or_Train_Group <- "Train"

# Merge the training and the test sets to create one data set.
merged_data <- rbind(x_test, x_train)

# creates a second, independent tidy data set with the average of 
# each variable for each activity and each subject.
tidyData <- ddply(merged_data, .(Subject, Activity), function(x) colMeans(x[, 3:81]))





