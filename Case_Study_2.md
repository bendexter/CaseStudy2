---
title: "Case_Study_2"
author: "Ben Tanaka"
date: "7/29/2019"
output: 
  html_document:
    keep_md: true
---



# DDSAnalytics:  Predicting Employee Turnover


```r
df_emp_ren <- read_excel("CaseStudy2-data.xlsx")
names(df_emp_ren)
```

```
##  [1] "Age"                      "Attrition"               
##  [3] "BusinessTravel"           "DailyRate"               
##  [5] "Department"               "DistanceFromHome"        
##  [7] "Education"                "EducationField"          
##  [9] "EmployeeCount"            "EmployeeNumber"          
## [11] "EnvironmentSatisfaction"  "Gender"                  
## [13] "HourlyRate"               "JobInvolvement"          
## [15] "JobLevel"                 "JobRole"                 
## [17] "JobSatisfaction"          "MaritalStatus"           
## [19] "MonthlyIncome"            "MonthlyRate"             
## [21] "NumCompaniesWorked"       "Over18"                  
## [23] "OverTime"                 "PercentSalaryHike"       
## [25] "PerformanceRating"        "RelationshipSatisfaction"
## [27] "StandardHours"            "StockOptionLevel"        
## [29] "TotalWorkingYears"        "TrainingTimesLastYear"   
## [31] "WorkLifeBalance"          "YearsAtCompany"          
## [33] "YearsInCurrentRole"       "YearsSinceLastPromotion" 
## [35] "YearsWithCurrManager"
```

```r
df_emp_ren$Attrition_Lvl <- factor(df_emp_ren$Attrition)
str(df_emp_ren)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	1470 obs. of  36 variables:
##  $ Age                     : num  41 49 37 33 27 32 59 30 38 36 ...
##  $ Attrition               : chr  "Yes" "No" "Yes" "No" ...
##  $ BusinessTravel          : chr  "Travel_Rarely" "Travel_Frequently" "Travel_Rarely" "Travel_Frequently" ...
##  $ DailyRate               : num  1102 279 1373 1392 591 ...
##  $ Department              : chr  "Sales" "Research & Development" "Research & Development" "Research & Development" ...
##  $ DistanceFromHome        : num  1 8 2 3 2 2 3 24 23 27 ...
##  $ Education               : num  2 1 2 4 1 2 3 1 3 3 ...
##  $ EducationField          : chr  "Life Sciences" "Life Sciences" "Other" "Life Sciences" ...
##  $ EmployeeCount           : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ EmployeeNumber          : num  1 2 4 5 7 8 10 11 12 13 ...
##  $ EnvironmentSatisfaction : num  2 3 4 4 1 4 3 4 4 3 ...
##  $ Gender                  : chr  "Female" "Male" "Male" "Female" ...
##  $ HourlyRate              : num  94 61 92 56 40 79 81 67 44 94 ...
##  $ JobInvolvement          : num  3 2 2 3 3 3 4 3 2 3 ...
##  $ JobLevel                : num  2 2 1 1 1 1 1 1 3 2 ...
##  $ JobRole                 : chr  "Sales Executive" "Research Scientist" "Laboratory Technician" "Research Scientist" ...
##  $ JobSatisfaction         : num  4 2 3 3 2 4 1 3 3 3 ...
##  $ MaritalStatus           : chr  "Single" "Married" "Single" "Married" ...
##  $ MonthlyIncome           : num  5993 5130 2090 2909 3468 ...
##  $ MonthlyRate             : num  19479 24907 2396 23159 16632 ...
##  $ NumCompaniesWorked      : num  8 1 6 1 9 0 4 1 0 6 ...
##  $ Over18                  : chr  "Y" "Y" "Y" "Y" ...
##  $ OverTime                : chr  "Yes" "No" "Yes" "Yes" ...
##  $ PercentSalaryHike       : num  11 23 15 11 12 13 20 22 21 13 ...
##  $ PerformanceRating       : num  3 4 3 3 3 3 4 4 4 3 ...
##  $ RelationshipSatisfaction: num  1 4 2 3 4 3 1 2 2 2 ...
##  $ StandardHours           : num  80 80 80 80 80 80 80 80 80 80 ...
##  $ StockOptionLevel        : num  0 1 0 0 1 0 3 1 0 2 ...
##  $ TotalWorkingYears       : num  8 10 7 8 6 8 12 1 10 17 ...
##  $ TrainingTimesLastYear   : num  0 3 3 3 3 2 3 2 2 3 ...
##  $ WorkLifeBalance         : num  1 3 3 3 3 2 2 3 3 2 ...
##  $ YearsAtCompany          : num  6 10 0 8 2 7 1 1 9 7 ...
##  $ YearsInCurrentRole      : num  4 7 0 7 2 7 0 0 7 7 ...
##  $ YearsSinceLastPromotion : num  0 1 0 3 2 3 0 0 1 7 ...
##  $ YearsWithCurrManager    : num  5 7 0 0 2 6 0 0 8 7 ...
##  $ Attrition_Lvl           : Factor w/ 2 levels "No","Yes": 2 1 2 1 1 1 1 1 1 1 ...
```

```r
summary(df_emp_ren)
```

```
##       Age         Attrition         BusinessTravel       DailyRate     
##  Min.   :18.00   Length:1470        Length:1470        Min.   : 102.0  
##  1st Qu.:30.00   Class :character   Class :character   1st Qu.: 465.0  
##  Median :36.00   Mode  :character   Mode  :character   Median : 802.0  
##  Mean   :36.92                                         Mean   : 802.5  
##  3rd Qu.:43.00                                         3rd Qu.:1157.0  
##  Max.   :60.00                                         Max.   :1499.0  
##   Department        DistanceFromHome   Education     EducationField    
##  Length:1470        Min.   : 1.000   Min.   :1.000   Length:1470       
##  Class :character   1st Qu.: 2.000   1st Qu.:2.000   Class :character  
##  Mode  :character   Median : 7.000   Median :3.000   Mode  :character  
##                     Mean   : 9.193   Mean   :2.913                     
##                     3rd Qu.:14.000   3rd Qu.:4.000                     
##                     Max.   :29.000   Max.   :5.000                     
##  EmployeeCount EmployeeNumber   EnvironmentSatisfaction    Gender         
##  Min.   :1     Min.   :   1.0   Min.   :1.000           Length:1470       
##  1st Qu.:1     1st Qu.: 491.2   1st Qu.:2.000           Class :character  
##  Median :1     Median :1020.5   Median :3.000           Mode  :character  
##  Mean   :1     Mean   :1024.9   Mean   :2.722                             
##  3rd Qu.:1     3rd Qu.:1555.8   3rd Qu.:4.000                             
##  Max.   :1     Max.   :2068.0   Max.   :4.000                             
##    HourlyRate     JobInvolvement    JobLevel       JobRole         
##  Min.   : 30.00   Min.   :1.00   Min.   :1.000   Length:1470       
##  1st Qu.: 48.00   1st Qu.:2.00   1st Qu.:1.000   Class :character  
##  Median : 66.00   Median :3.00   Median :2.000   Mode  :character  
##  Mean   : 65.89   Mean   :2.73   Mean   :2.064                     
##  3rd Qu.: 83.75   3rd Qu.:3.00   3rd Qu.:3.000                     
##  Max.   :100.00   Max.   :4.00   Max.   :5.000                     
##  JobSatisfaction MaritalStatus      MonthlyIncome    MonthlyRate   
##  Min.   :1.000   Length:1470        Min.   : 1009   Min.   : 2094  
##  1st Qu.:2.000   Class :character   1st Qu.: 2911   1st Qu.: 8047  
##  Median :3.000   Mode  :character   Median : 4919   Median :14236  
##  Mean   :2.729                      Mean   : 6503   Mean   :14313  
##  3rd Qu.:4.000                      3rd Qu.: 8379   3rd Qu.:20462  
##  Max.   :4.000                      Max.   :19999   Max.   :26999  
##  NumCompaniesWorked    Over18            OverTime        
##  Min.   :0.000      Length:1470        Length:1470       
##  1st Qu.:1.000      Class :character   Class :character  
##  Median :2.000      Mode  :character   Mode  :character  
##  Mean   :2.693                                           
##  3rd Qu.:4.000                                           
##  Max.   :9.000                                           
##  PercentSalaryHike PerformanceRating RelationshipSatisfaction
##  Min.   :11.00     Min.   :3.000     Min.   :1.000           
##  1st Qu.:12.00     1st Qu.:3.000     1st Qu.:2.000           
##  Median :14.00     Median :3.000     Median :3.000           
##  Mean   :15.21     Mean   :3.154     Mean   :2.712           
##  3rd Qu.:18.00     3rd Qu.:3.000     3rd Qu.:4.000           
##  Max.   :25.00     Max.   :4.000     Max.   :4.000           
##  StandardHours StockOptionLevel TotalWorkingYears TrainingTimesLastYear
##  Min.   :80    Min.   :0.0000   Min.   : 0.00     Min.   :0.000        
##  1st Qu.:80    1st Qu.:0.0000   1st Qu.: 6.00     1st Qu.:2.000        
##  Median :80    Median :1.0000   Median :10.00     Median :3.000        
##  Mean   :80    Mean   :0.7939   Mean   :11.28     Mean   :2.799        
##  3rd Qu.:80    3rd Qu.:1.0000   3rd Qu.:15.00     3rd Qu.:3.000        
##  Max.   :80    Max.   :3.0000   Max.   :40.00     Max.   :6.000        
##  WorkLifeBalance YearsAtCompany   YearsInCurrentRole
##  Min.   :1.000   Min.   : 0.000   Min.   : 0.000    
##  1st Qu.:2.000   1st Qu.: 3.000   1st Qu.: 2.000    
##  Median :3.000   Median : 5.000   Median : 3.000    
##  Mean   :2.761   Mean   : 7.008   Mean   : 4.229    
##  3rd Qu.:3.000   3rd Qu.: 9.000   3rd Qu.: 7.000    
##  Max.   :4.000   Max.   :40.000   Max.   :18.000    
##  YearsSinceLastPromotion YearsWithCurrManager Attrition_Lvl
##  Min.   : 0.000          Min.   : 0.000       No :1233     
##  1st Qu.: 0.000          1st Qu.: 2.000       Yes: 237     
##  Median : 1.000          Median : 3.000                    
##  Mean   : 2.188          Mean   : 4.123                    
##  3rd Qu.: 3.000          3rd Qu.: 7.000                    
##  Max.   :15.000          Max.   :17.000
```
## Transform Factors into actual results


```r
df_emp_ren <- df_emp_ren %>%
  mutate(Education = as.factor(if_else(Education == 1,"Below College", if_else(Education == 2, "College", if_else(Education == 3, "Bachelor", if_else(Education == 4, "Master","Doctor")))))
         ,EnvironmentSatisfaction = as.factor(if_else(EnvironmentSatisfaction == 1,"Low",if_else(EnvironmentSatisfaction == 2, "Medium", if_else(EnvironmentSatisfaction == 3, "High", "Very High"))))
         ,JobInvolvement = as.factor(if_else(JobInvolvement == 1,"Low",if_else(JobInvolvement == 2, "Medium",if_else(JobInvolvement == 3, "High", "Very High"))))
         ,JobSatisfaction = as.factor(if_else(JobSatisfaction == 1, "Low",if_else(JobSatisfaction == 2, "Medium",if_else(JobSatisfaction == 3, "High","Very High"))))
         ,PerformanceRating = as.factor(if_else(PerformanceRating == 1, "Low",if_else(PerformanceRating == 2, "Good", if_else(PerformanceRating == 3, "Excellent", "Outstanding"))))
         ,RelationshipSatisfaction = as.factor(if_else(RelationshipSatisfaction == 1, "Low",if_else(RelationshipSatisfaction == 2, "Medium", if_else(RelationshipSatisfaction == 3, "High", "Very High"))))
         ,WorkLifeBalance = as.factor(if_else(WorkLifeBalance == 1, "Bad",if_else(WorkLifeBalance == 2, "Good", if_else(WorkLifeBalance == 3, "Better", "Best"))))
         ,JobLevel = as.factor(JobLevel)
         )
```

## Employee Personal Demographics

```r
p1 <- ggplot(df_emp_ren, aes(x=EducationField ,  group=Attrition)) +
geom_bar(aes(y = ..prop.., fill = factor(..x..)), stat="count") +
geom_text(aes( label = scales::percent(..prop..),
y= ..prop.. ), stat= "count", size = 2.5, position=position_dodge(width=0.2)) +
labs(y = "Percent", fill="EducationField") +
theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +
theme(legend.position = "none") +
ggtitle("Attrition Counts - Education Field")+
facet_grid(~Attrition) +
scale_y_continuous(labels = scales::percent)

p2 <- df_emp_ren %>%
  group_by(EducationField) %>%
  summarise(attrition_rate = round((sum(if_else(Attrition == "Yes",1,0))/n()*100),2)) %>%
  ggplot(aes(x = EducationField, y = attrition_rate))+ geom_bar(stat = 'identity',fill = "coral3") + ggtitle("Attrition Rate - Education Field") + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +geom_text(aes(label=attrition_rate), size = 2.5, position=position_dodge(width=0.2), vjust=-0.25)+ scale_y_continuous(limits = c(0, 30))

p3 <- ggplot(df_emp_ren, aes(x=Education ,  group=Attrition)) +
geom_bar(aes(y = ..prop.., fill = factor(..x..)), stat="count") +
geom_text(aes( label = scales::percent(..prop..),
y= ..prop.. ), stat= "count",  size = 2.5, position=position_dodge(width=0.2)) +
labs(y = "Percent", fill="Attrition_Lvl") +
  theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +
  theme(legend.position = "none") +
  ggtitle("Attrition Counts - Education")+
facet_grid(~Attrition) +
scale_y_continuous(labels = scales::percent)

p4 <- df_emp_ren %>%
  group_by(Education) %>%
  summarise(attrition_rate = round((sum(if_else(Attrition == "Yes",1,0))/n()*100),2)) %>%
  ggplot(aes(x = Education, y = attrition_rate))+ geom_bar(stat = 'identity',fill = "coral3") + ggtitle("Attrition Rate - Education") + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +geom_text(aes(label=attrition_rate), size = 2.5, position=position_dodge(width=0.2), vjust=-0.25)+ scale_y_continuous(limits = c(0, 30))


grid.arrange(p1, p2, p3, p4, nrow = 2, ncol = 2)
```

![](Case_Study_2_files/figure-html/Employee_Demographics-1.png)<!-- -->

```r
EmpPerDem2 <- df_emp_ren[c("NumCompaniesWorked","TotalWorkingYears", "DistanceFromHome" ,"Attrition_Lvl")]
ggpairs(EmpPerDem2)
```

![](Case_Study_2_files/figure-html/Employee_Demographics-2.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = NumCompaniesWorked, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Number Companies Worked")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = TotalWorkingYears, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Total Working Years")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = DistanceFromHome, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Distance From Home")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, nrow = 2, ncol = 2)
```

![](Case_Study_2_files/figure-html/Employee_Demographics-3.png)<!-- -->

```r
p1 <- ggplot(df_emp_ren, aes(x=MaritalStatus ,  group=Attrition)) +
geom_bar(aes(y = ..prop.., fill = factor(..x..)), stat="count") +
geom_text(aes( label = scales::percent(..prop..),
y= ..prop.. ), stat= "count", size = 2.5, position=position_dodge(width=0.2)) +
labs(y = "Percent", fill="MaritalStatus") +
theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +
theme(legend.position = "none") +
ggtitle("Attrition Counts - Marital Status")+
facet_grid(~Attrition) +
scale_y_continuous(labels = scales::percent)

p2 <- df_emp_ren %>%
  group_by(MaritalStatus) %>%
  summarise(attrition_rate = round((sum(if_else(Attrition == "Yes",1,0))/n()*100),2)) %>%
  ggplot(aes(x = MaritalStatus, y = attrition_rate))+ geom_bar(stat = 'identity',fill = "coral3") + ggtitle("Attrition Rate - MaritalStatus") + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +geom_text(aes(label=attrition_rate), size = 2.5, position=position_dodge(width=0.2), vjust=-0.25)+ scale_y_continuous(limits = c(0, 30))


p3 <- ggplot(df_emp_ren, aes(x=Gender ,  group=Attrition)) +
geom_bar(aes(y = ..prop.., fill = factor(..x..)), stat="count") +
geom_text(aes( label = scales::percent(..prop..),
y= ..prop.. ), stat= "count", size = 2.5, position=position_dodge(width=0.2)) +
labs(y = "Percent", fill="Gender") +
theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +
theme(legend.position = "none") +
ggtitle("Attrition Counts - Gender")+
facet_grid(~Attrition) +
scale_y_continuous(labels = scales::percent)

p4 <- df_emp_ren %>%
  group_by(Gender) %>%
  summarise(attrition_rate = round((sum(if_else(Attrition == "Yes",1,0))/n()*100),2)) %>%
  ggplot(aes(x = Gender, y = attrition_rate))+ geom_bar(stat = 'identity',fill = "coral3") + ggtitle("Attrition Rate - Gender") + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank()) +geom_text(aes(label=attrition_rate), size = 2.5, position=position_dodge(width=0.2), vjust=-0.25)+ scale_y_continuous(limits = c(0, 30))


grid.arrange(p1, p2, p3, p4, nrow = 2, ncol = 2)
```

![](Case_Study_2_files/figure-html/Employee_Demographics-4.png)<!-- -->

```r
EmpPerDem3 <- df_emp_ren[c("Age","Attrition_Lvl")]
ggpairs(EmpPerDem3)
```

![](Case_Study_2_files/figure-html/Employee_Demographics-5.png)<!-- -->

```r
p3 <- df_emp_ren %>%
  ggplot(aes(x = Age, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Age")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3
```

![](Case_Study_2_files/figure-html/Employee_Demographics-6.png)<!-- -->

### Employee Personal Demographics:  Insights
* Insights:
    1.  enter insights here
      + Hypothesis: 


## Employee Pay Rates

```r
EmpPayRte1 <- df_emp_ren[c("HourlyRate", "DailyRate", "MonthlyRate","MonthlyIncome", "PercentSalaryHike","Attrition_Lvl")]
ggpairs(EmpPayRte1)
```

![](Case_Study_2_files/figure-html/Employee Pay Rates-1.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = HourlyRate, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Hourly Rate")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = DailyRate, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Daily Rate")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = MonthlyRate, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Monthly Rate")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p4 <- df_emp_ren %>%
  ggplot(aes(x = MonthlyIncome, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Monthly Income")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p5 <- df_emp_ren %>%
  ggplot(aes(x = PercentSalaryHike, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Percent Salary Hike")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, p4, p5, nrow = 3, ncol = 3)
```

![](Case_Study_2_files/figure-html/Employee Pay Rates-2.png)<!-- -->

```r
EmpPayRte2 <- df_emp_ren[c("StockOptionLevel","OverTime","Attrition_Lvl")]
ggpairs(EmpPayRte2)
```

![](Case_Study_2_files/figure-html/Employee Pay Rates-3.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = StockOptionLevel, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Stock Option Level")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = OverTime, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Over Time")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())


grid.arrange(p1, p2, nrow = 1, ncol = 2)
```

![](Case_Study_2_files/figure-html/Employee Pay Rates-4.png)<!-- -->

### Employee Pay Rates:  Insights
* Insights:
    1.  enter insights here
      + Hypothesis: 

## Employee Work Profile Demographics

```r
EmpPflDem1 <- df_emp_ren[c("YearsWithCurrManager","YearsSinceLastPromotion","YearsInCurrentRole","YearsAtCompany","PerformanceRating", "Attrition_Lvl")]
ggpairs(EmpPflDem1)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-1.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = YearsWithCurrManager, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years With Current Manager")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = YearsSinceLastPromotion, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years Since Last Promotion")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = YearsInCurrentRole, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years In Current Role")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p4 <- df_emp_ren %>%
  ggplot(aes(x = YearsAtCompany, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years At Company")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p5 <- df_emp_ren %>%
  ggplot(aes(x = PerformanceRating, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Performance Rating")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, p4, p5, nrow = 3, ncol = 3)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-2.png)<!-- -->

```r
EmpPflDem2 <- df_emp_ren[c("WorkLifeBalance","RelationshipSatisfaction","JobSatisfaction","JobInvolvement","EnvironmentSatisfaction", "Attrition_Lvl")]
ggpairs(EmpPflDem2)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-3.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = WorkLifeBalance, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Work Life Balance")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = RelationshipSatisfaction, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Relationship Satisfaction")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = JobSatisfaction, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Job Satisfaction")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p4 <- df_emp_ren %>%
  ggplot(aes(x = JobInvolvement, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Job Involvement")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p5 <- df_emp_ren %>%
  ggplot(aes(x = EnvironmentSatisfaction, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Environment Satisfaction")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, p4, p5, nrow = 3, ncol = 3)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-4.png)<!-- -->

```r
EmpPflDem3 <- df_emp_ren[c("TrainingTimesLastYear","StandardHours","JobRole","JobLevel","Department","BusinessTravel", "Attrition_Lvl")]
ggpairs(EmpPflDem3)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-5.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = TrainingTimesLastYear, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Training Times Last Year")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = StandardHours, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Standard Hours")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = JobRole, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Job Role")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p4 <- df_emp_ren %>%
  ggplot(aes(x = Department, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Department")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p5 <- df_emp_ren %>%
  ggplot(aes(x = BusinessTravel, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Business Travel")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, p4, p5, nrow = 3, ncol = 3)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-6.png)<!-- -->

```r
EmpPflDem4 <- df_emp_ren[c("YearsAtCompany", "YearsInCurrentRole", "YearsSinceLastPromotion","YearsWithCurrManager", "Attrition_Lvl")]
ggpairs(EmpPflDem4)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-7.png)<!-- -->

```r
p1 <- df_emp_ren %>%
  ggplot(aes(x = YearsAtCompany, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years At Company")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p2 <- df_emp_ren %>%
  ggplot(aes(x = YearsInCurrentRole, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years In Current Role")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p3 <- df_emp_ren %>%
  ggplot(aes(x = YearsSinceLastPromotion, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years Since Last Promotion")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

p4 <- df_emp_ren %>%
  ggplot(aes(x = YearsWithCurrManager, fill = Attrition)) + geom_density(alpha = 0.5) + ggtitle("Years With Current Manager")  + theme(plot.title = element_text(size =10),axis.text.x = element_text(size =7,angle = 45, hjust = 1),axis.title.x=element_blank())

grid.arrange(p1, p2, p3, p4,  nrow = 2, ncol = 2)
```

![](Case_Study_2_files/figure-html/Employee Work Profile Demographics-8.png)<!-- -->

### Employee Work Profile Demographics:  Insights
* Insights:
    1.  enter insights here
      + Hypothesis: 
      
      
  

```r
p1 <- ggplot(df_emp_ren, aes(x= Attrition,  group=EducationField)) +
geom_bar(aes(y = ..prop.., fill = factor(..x..)), stat="count") +
geom_text(aes( label = scales::percent(..prop..),
y= ..prop.. ), stat= "count", vjust = -.5) +
labs(y = "Percent", fill="Attrition_Lvl") +
facet_grid(~EducationField) +
scale_y_continuous(labels = scales::percent)
```

