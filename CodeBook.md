First I manually downloaded the file into my working directory and set the working directory manually using the RStudio interface. 

Then I assigned the names to the variables in the data set:
features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")

These codes above shows how I have assigned the provided data to descriptive names that I will be using. 

After this I merged the training and test data sets into a single dataset using the code below:

X <- rbind(x_train, x_test)
Y <- rbind(y_train, y_test)
Subject <- rbind(subject_train, subject_test)
Merged <- cbind(Subject, Y, X)

I named the merged data set "Merged"

Then, I calculated the mean and SD of each of the variables to tidy up the data, and I named the tidied dataset "Tidy" uding this code below:

Tidy <- Merged %>% select(subject, code, contains("mean"), contains("std"))
Tidy$code <- activities[Tidy$code, 2]

Then I labelled the data set with descriptive names that are easy to understand to make the data more comprehensible using the following codes:

names(Tidy)[2] = "activity"
names(Tidy)<-gsub("Acc", "Accelerometer", names(Tidy))
names(Tidy)<-gsub("Gyro", "Gyroscope", names(Tidy))
names(Tidy)<-gsub("BodyBody", "Body", names(Tidy))
names(Tidy)<-gsub("Mag", "Magnitude", names(Tidy))
names(Tidy)<-gsub("^t", "Time", names(Tidy))
names(Tidy)<-gsub("^f", "Frequency", names(Tidy))
names(Tidy)<-gsub("tBody", "TimeBody", names(Tidy))
names(Tidy)<-gsub("-mean()", "Mean", names(Tidy), ignore.case = TRUE)
names(Tidy)<-gsub("-std()", "STD", names(Tidy), ignore.case = TRUE)
names(Tidy)<-gsub("-freq()", "Frequency", names(Tidy), ignore.case = TRUE)
names(Tidy)<-gsub("angle", "Angle", names(Tidy))
names(Tidy)<-gsub("gravity", "Gravity", names(Tidy))

Finally I created the Final, cleaned dataset using these codes and exported them:

Final <- Tidy %>%
  group_by(subject, activity) %>%
  summarise_all(funs(mean))
write.table(Final, "Final.txt", row.name=FALSE)

I doubled checked if any mistakes are made by looking at the structure and the first 6 rows of the tidied data set I created using the following 2 codes: 

str(Final)

head(Final)
