Project for Getting and Cleaning Data course

submitted by Merza Klaghstan
==================
used R script:
***

xtest <- read.table("./UCI HAR Dataset/test/X_test.txt")
ytest <- read.table("./UCI HAR Dataset/test/y_test.txt")
stest <- read.table("./UCI HAR Dataset/test/subject_test.txt")
# column bind, 561+Y+Subject
test <- cbind(xtest, ytest, stest)

xtrain <- read.table("./UCI HAR Dataset/train/X_train.txt")
ytrain <- read.table("./UCI HAR Dataset/train/y_train.txt")
strain <- read.table("./UCI HAR Dataset/train/subject_train.txt")
# column bind, 561+Y+Subject
train <- cbind(xtrain, ytrain, strain)

# @Task-1@ row bind in 1 data frame
df <- rbind(test, train)

# read features names
features <- read.table("./UCI HAR Dataset/features.txt", stringsAsFactors=F)
# @Task-4@ assign names to data frame's variables
names(df)<-c(paste(features$V1,features$V2), "label", "subject")
# @Task-2@ select out only Means & Stds
df <- select(df, contains("mean") , contains("std"), label, subject)

# @Task-3@ read labels' names and replace them in the data fame
labels <- read.table("./UCI HAR Dataset/activity_labels.txt", stringsAsFactors=F)
df<-merge(df, labels, by.x="label", by.y="V1")
df<-select(df, -contains("label"))
names(df)[names(df)=="V2"] <- "label"

# @Task-5@
meansByLabel<-by(df[,c(1:86)], df$label, colMeans)
meansBySub<-by(df[,c(1:86)], df$subject, colMeans)
d1 <- data.frame( unlist(meansByLabel) )
names(d1)<-"mean"
d2 <- data.frame( unlist(meansBySub) )
names(d2)<-"mean"
res <- rbind(d1,d2)
res<-mutate(res, new=rownames(res))
res<-separate(res, new, into=c("x", "measurement"), sep="[.]")

write.table(res, "res.txt", row.name=FALSE)