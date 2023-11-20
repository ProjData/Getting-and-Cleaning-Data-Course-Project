library(dplyr)

######################### TRAINING DATASETS #########################

# Load the training set
x_train <- read.table("train/X_train.txt")

# Load Activity-Codes from the training set
y_train <- read.table("train/y_train.txt")

# Load the dataset containing the subjects performing the activities
subject_train <- read.table("train/subject_train.txt")

# Combine all training data into one table
train <- cbind(subject_train, y_train, x_train)

# Remove the individual datasets to free up memory
rm(x_train, y_train, subject_train)

######################### TEST DATASETS #########################

# Load the test set
x_test <- read.table("test/X_test.txt")

# Load Activity-Codes from the test set
y_test <- read.table("test/y_test.txt")

# Load the dataset containing the subjects performing the activities
subject_test <- read.table("test/subject_test.txt")

# Combine all test data into one table
test <- cbind(subject_test, y_test, x_test)

# Remove the individual datasets to free up memory
rm(x_test, y_test, subject_test)

######################### COMBINE TRAINING AND TEST #########################

# 1. Merges the training and the test sets to create one data set.

# Combine training and test sets
completeSet <- rbind(train, test)

# Remove the individual train and test sets
rm(train, test)

# 4. Appropriately labels the data set with descriptive variable names.

# Load column names of all variables recorded in the train/test set 
# X_train.txt / X_test.txt to rename columns in "completeSet"
features <- read.table("features.txt")
colnames(completeSet)[3:563] <- features[,2]

# Remove features dataset to free up memory
rm(features)

# Rename the first two columns to fitting names
colnames(completeSet)[1:2] <- c("Subject", "Activity")

# 3. Uses descriptive activity names to name the activities in the data set

# Rename Activity-Codes for better understanding
completeSet[, 2] <- recode(completeSet[, 2], `1` = "WALKING", 
                           `2` = "WALKING_UPSTAIRS", `3` = "WALKING_DOWNSTAIRS",
                           `4` = "SITTING", `5` = "STANDING", `6` = "LAYING")

######################### FILTER RELEVANT DATA #########################

# 2. Extracts only the measurements on the mean and standard deviation 
# for each measurement.

# As the task states, we only need the mean values and the standard-deviations 
# for each measurement.
# Therefore, we determine the column indices that contain the terms 
# "mean" and "std".
relevantColumns <- grep( "(mean|std)",colnames(completeSet))

# Now, we use the indices to extract only the relevant columns.
# Of course, we also keep the first two columns containing the subject number
# and activities.
relevantSet <- completeSet[, c(1:2, relevantColumns)]

# Remove  "completeSet" and "relevantColumns" to free up memory
rm(completeSet, relevantColumns)

######################### DATASET WITH AVERAGES #########################

# 5. From the data set in step 4, creates a second, independent tidy data 
# set with the average of each variable for each activity and each subject.
tidyDataSet <- relevantSet %>%
    group_by(Subject, Activity) %>%
    summarise(across(everything(), mean, na.rm = TRUE))

# Rename Columns
colnames(tidyDataSet)[3:81] <- paste0("avg-", colnames(tidyDataSet)[3:81])

# Export "tidyDataSet" as a txt file
write.table(tidyDataSet, "tidyDataSet.txt", row.names = FALSE)

