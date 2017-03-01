###How to access ESS data via R
          
Steps:      
1. Request the data from ESS API using `GET()` from the `httr` library        
2. Check status of the response. If you're pulling data via multiple iterations then implement either `warn_for_status(x)` or `stop_for_status(x)`.          
3. If status is OK (200), then proceed to convert the data from response from JSON character string to an R object using `content()` as type `text`        
4. Then convert JSON character string to an R object using `fromJSON()` function from the `jsonlite` library              
5. Assign index labels to each data pull (refer to data dictionary above)   
6. Save as dataframes or export as .csv files for offline analysis      
      
#####Data Source 1: Sentiments Data
Sample File: https://data.thumbtack.com/v1/sentiments/states      

Here is a basic example on how to pull sentiment scores data at the states level. The same can be applied to CBSAs.
```r
library(httr) # functions to send request to ESS API
library(jsonlite) # to convert json data type to R object 

ss_url <- modify_url(paste0("https://data.thumbtack.com/v1/sentiments/states?"))
ss_response <- GET(ss_url)
warn_for_status(ss_response)
ss_text <- content(ss_response, "text")
ss_data <- fromJSON(ss_text, flatten = TRUE)$data
```

Here is a more complex example of pulling all sentiment scores by demographic variables: gender, age, origin, and employer.      
```r
demographics <- c("gender", "age", "origin", "employer")
demographics_levels <- c(2, 6, 5, 3)

demographics_df <- data.frame(demographics, demographics_levels)

for(h in 1:nrow(demographics_df)) {
  
  dem <- demographics_df$demographics[h]

  s_data <- c()
  for(i in 1:demographics_df$demographics_levels[h]){
    s_url <- modify_url(paste0("https://data.thumbtack.com/v1/sentiments/states?", dem, "=", i , "&index=1&industry=0"))
    s_response <- GET(s_url)
    warn_for_status(s_response)
    s_text <- content(s_response, "text")
    s_data_i <- fromJSON(s_text, flatten = TRUE)$data
    s_data_i$index <- i
    s_data <- rbind(s_data, s_data_i)
  }

#Assign labels to the age category codes:
if(dem == "gender"){
    gender_code <- c(Female = 1, Male = 2)
    s_data$gender <- names(gender_code)[match(s_data$index, gender_code)]
} else if (dem == "age"){
    age_code <- c(`Under 25`= 1, `25-34` = 2, `35-44` = 3, `45-54` = 4, `55-64` = 5, `65 or above` = 6)
    s_data$age <- names(age_code)[match(s_data$index, age_code)]
} else if (dem == "origin"){
    origin_code <- c(`Hispanic, Latino, or Spanish` = 1, `Asian` = 2, `Other` = 3, `White (Caucasian)` = 4, `Black or African American ` = 5)
    s_data$origin <- names(origin_code)[match(s_data$index, origin_code)]
} else {
    employer_code <- c(`Non-employer` = 1, `Small Employer` = 2, `Large Employer` = 3)
    s_data$employer <- names(employer_code)[match(s_data$index, employer_code)]
}

write.csv(s_data, paste("s_", dem , "_data.csv", sep = ""), row.names = FALSE)
}
```
          
#####Data Source 2: Question Scores
Sample File: https://data.thumbtack.com/v1/question-scores      
Note that column `scores` is type list.  
```r
library(plyr) # to convert list to dataframe

qs_response <- GET("https://data.thumbtack.com/v1/question-scores")
warn_for_status(qs_response)
qs_text <- content(qs_response, "text")
qs_df <- fromJSON(qs_text, flatten = TRUE)$data

str(qs_df) # check type of each column 
```
![](figure/head_question_scores.png)
        
              
Given that the `scores` column in this data is a list, we want to reshape the data. We can manipulate each item in the list by appending the information in the row of the nested list to each element in the list. Then, we convert this updated list to a data frame.         
```r
s <- qs_df$scores

for(i in 1:length(s)) {
    s[[i]]$id <- qs_df$id[i]
    s[[i]]$start_date <- qs_df$start_date[i]
    s[[i]]$end_date <-  qs_df$end_date[i]
}

s_df <- ldply(s, data.frame) # convert s to dataframe
s_df$index <- "National"
```        
![](figure/unlisted_question_scores.png)   


          
#####Data Source 3: Top Problems
Sample File: https://data.thumbtack.com/v1/top-problems        
Example of pulling all top problem metrics at the industry level for "Landscape and Lawncare"       
```
tp_response <- GET("https://data.thumbtack.com/v1/top-problems?index=1&industry=1")
warn_for_status(tp_response)
tp_text <- content(tp_response, "text")
tp_df <- fromJSON(tp_text, flatten = TRUE)$data
tp_df$industry <- "Landscape and Lawncare"
```
![](figure/head_top_problems.png)     
