Code Book
==================
Steps:
==================
1. read data for both test and train measurements
2. combine them in one data frame
3. change columns' names, read from features
4. select only columns having "mean" or "std" (also keep the "label" and "subject" columns)
5. replace labels' values with their real names, read from activity_labels
6. calculate means, grouped by labels and subjects
7. split row names into: x (label or subject) + measurement
8. write to output file

==================
Data:
==================
1. Mean
2. Used "label" or "subject"
3. Measurement