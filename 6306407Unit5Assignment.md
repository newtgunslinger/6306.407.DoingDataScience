---
title: "6306 407 Unit 5 Assignment"
author: "Blaine Brewer"
date: "February 11, 2019"
output: 
  html_document:
    keep_md: yes
---



### Questions

##### 1.	Data Munging (30 points): Utilize yob2016.txt for this question. This file is a series of popular children’s names born in the year 2016 in the United States.  It consists of three columns with a first name, a gender, and the amount of children given that name.  However, the data is raw and will need cleaning to make it tidy and usable.  

###### a.	First, import the .txt file into R so you can process it.  Keep in mind this is not a CSV file.  You might have to open the file to see what you’re dealing with.  Assign the resulting data frame to an object, df, that consists of three columns with human-readable column names for each.  


```r
  df <- read.table(file = "yob2016.txt", header = FALSE, sep = ";")
  names(df) <- c("name", "sex", "count")
```

###### b.	Display the summary and structure of df.  


```r
  summary(df)
```

```
##       name       sex           count        
##  Aalijah:    2   F:18758   Min.   :    5.0  
##  Aaliyan:    2   M:14111   1st Qu.:    7.0  
##  Aamari :    2             Median :   12.0  
##  Aarian :    2             Mean   :  110.7  
##  Aarin  :    2             3rd Qu.:   30.0  
##  Aaris  :    2             Max.   :19414.0  
##  (Other):32857
```

```r
  str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ name : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ sex  : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ count: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

###### c.	Your client tells you that there is a problem with the raw file.  One name was entered twice and misspelled.  The client cannot remember which name it is; there are thousands he saw! But he did mention he accidentally put three y’s at the end of the name.  Write an R command to figure out which name it is and display it.


```r
  grep("yyy", df$name, value = TRUE)
```

```
## [1] "Fionayyy"
```

###### d.	Upon finding the misspelled name, please remove this particular observation, as the client says it’s redundant.  Save the remaining dataset as an object: y2016.  


```r
  y2016 <- df[!(grepl("yyy", df$name)),]
```

##### 2.	Data Merging (30 points): Utilize yob2015.txt for this question.  This file is similar to yob2016, but contains names, gender, and total children given that name for the year 2015.

###### a.	Like 1a, please import the .txt file into R.  Look at the file before you do.  You might have to change some options to import it properly.  Again, please give the dataframe human-readable column names.  Assign the dataframe to y2015.  


```r
  y2015 <- read.table(file = "yob2015.txt", header = FALSE, sep = ",")
  names(y2015) <- c("name","sex","count")
```

###### b.	Display the last ten rows in the dataframe.  Describe something you find interesting about these 10 rows.

####### A: The names all begin with "Z" and they are all males.  


```r
  tail(y2015,10)
```

```
##         name sex count
## 33054   Ziyu   M     5
## 33055   Zoel   M     5
## 33056  Zohar   M     5
## 33057 Zolton   M     5
## 33058   Zyah   M     5
## 33059 Zykell   M     5
## 33060 Zyking   M     5
## 33061  Zykir   M     5
## 33062  Zyrus   M     5
## 33063   Zyus   M     5
```

###### c.	Merge y2016 and y2015 by your Name column; assign it to final.  The client only cares about names that have data for both 2016 and 2015; there should be no NA values in either of your amount of children rows after merging.


```r
  final <- merge(y2016, y2015, by = "name")
  summary(final)
```

```
##       name       sex.x        count.x        sex.y        count.y       
##  Aalijah:    4   F:17827   Min.   :    5.0   F:17798   Min.   :    5.0  
##  Aamari :    4   M:13814   1st Qu.:    8.0   M:13843   1st Qu.:    8.0  
##  Aarian :    4             Median :   16.0             Median :   16.0  
##  Aaron  :    4             Mean   :  179.2             Mean   :  181.3  
##  Aarya  :    4             3rd Qu.:   45.0             3rd Qu.:   46.0  
##  Aaryn  :    4             Max.   :19414.0             Max.   :20415.0  
##  (Other):31617
```

##### 3.	Data Summary (30 points): Utilize your data frame object final for this part.

###### a.	Create a new column called “Total” in final that adds the amount of children in 2015 and 2016 together.  In those two years combined, how many people were given popular names?


```r
  final$Total <- final$count.x + final$count.y
```

###### b.	Sort the data by Total.  What are the top 10 most popular names?


```r
  final <- final[order(final$Total, decreasing = T),]
  kable(final$name[1:10],caption = "10 most popular names in 2015 and 2016 combined.", col.names = "Names")
```



Table: 10 most popular names in 2015 and 2016 combined.

|Names    |
|:--------|
|Emma     |
|Olivia   |
|Noah     |
|Liam     |
|Sophia   |
|Ava      |
|Mason    |
|William  |
|Jacob    |
|Isabella |

###### c.	The client is expecting a girl!  Omit boys and give the top 10 most popular girl’s names.


```r
  y2016.f <- y2016[y2016$sex == "F",]
  y2015.f <- y2015[y2015$sex == "F",]
  girls <- merge(y2016.f, y2015.f, by = "name")
  girls$Total <- girls$count.x + girls$count.y
  girls <- girls[order(girls$Total, decreasing = T),]
  kable(girls$name[1:10],caption = "10 most popular girls names in 2015 and 2016 combined.", col.names = "Girls Names")
```



Table: 10 most popular girls names in 2015 and 2016 combined.

|Girls Names |
|:-----------|
|Emma        |
|Olivia      |
|Sophia      |
|Ava         |
|Isabella    |
|Mia         |
|Charlotte   |
|Abigail     |
|Emily       |
|Harper      |

###### d.	Write these top 10 girl names and their Totals to a CSV file.  Leave out the other columns entirely.


```r
  write.csv(girls[,c("name","Total")],"girls.csv", row.names = FALSE)
```

##### 4.	Upload to GitHub (10 points): Push at minimum your RMarkdown for this homework assignment and a Codebook to one of your GitHub repositories (you might place this in a Homework repo like last week).  The Codebook should contain a short definition of each object you create, and if creating multiple files, which file it is contained in.  You are welcome and encouraged to add other files—just make sure you have a description and directions that are helpful for the grader.

See accompanying codebook.
