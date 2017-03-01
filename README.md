## <span style="color: #F27802">Accessing and Interpreting Thumbtack's Economic Sentiment Survey Data</span>

# Table of Contents   
1. [Overview of Thumbtack's Economic Sentiment Survey (ESS)](#summary)     
    * [What is ESS?](#what-is-ess?)     
    * [How the data is collected](#how-the-data-is-collected)    
    * [How the data is measured](#how-the-data-is-measured)      
    * [How the data is stored](#how-the-data-is-stored)   
2. [Data Dictionary and Sources](#data-dictionary-and-sources)   
    * [Demographics](#demographics)   
    * [Industry](#industry)     
    * [Data Source 1](#data-source-1)     
    * [Data Source 2](#data-source-2)     
    * [Data Source 3](#data-source-3) 
    * [Methodology](#methodology)     
        
###Overview of Thumbtack's Economic Sentiment Survey (ESS)     
####What is ESS?

Every month, the [Thumbtack Economic Sentiment Survey](https://www.thumbtack.com/survey#/sentiment) captures the attitudes and perspectives of thousands of business owners from across the country to gauge how they are feeling about the economy and their businesses. Designed in coordination with economists at Bloomberg in 2012, Thumbtack's Economic Sentiment Survey captures a ground-level view of the economy in all 50 states. The business owners we hear from in this survey work in a variety of service-based industries, with occupations such as general contractor, interior designer, photographer, and personal trainer. They are demographically and politically diverse: over 30% are non-white, 40% are female, and there are a near-identical number of self-identified Democrats and Republicans (24.65% and 24.32%, respectively). These respondents are largely mobile service professionals with five or fewer employees who operate in households across the United States. Because they are hard to reach, these professionals are frequently overlooked in other surveys of small businesses. 

####How the data is collected####

We administer this survey through an online, in-product survey portal, prompting small business owner-operators to take it by by displaying a banner at the top of their user dashboard. The survey is fielded for one week in the middle of every month, with the results posted on the Monday before the first Friday of every month. This is meant to synchronize with the monthly release by the Bureau of Labor Statistics of the national [Employment Situation](https://www.bls.gov/news.release/empsit.toc.htm). On average, we receive between 5,500 and 6,500 responses per month.     
        
####How the data is measured####

Based on the responses to the survey in each period, Thumbtack and Bloomberg have constructed two indices to track activity among small service professionals over time – a Small Business Sentiment Index and a Small Business Inflation index. These indices represent our current best estimates of where the strongest signal can be taken from the survey data collected to date.

An individual index number is computed for each question in each period using the following formulas:

* For questions with three point scales:
   - `(%_positive_response) + (0.5*(%_neutral_response))`
* For questions with a five point scale:
   - `(%_positive_response + %_very_positive_response) + (0.5*(%_neutral_response)`
* For questions with true/false answers, only the percentage of positive answers was used.

These indexes are combined into a Sentiment Index, comprised of the following 8 questions, each weighted accordingly:
* Current Financial Condition (0.17 weight) 
* Financial Condition in 3 months (0.17 weight)
* Economic Condition in 3 months (0.08 weight)
* Profitability Expectations in 3 months (0.13 weight)
* Job openings in the past 3 months (0.08 weight)
* Employment change expected in 3 months (0.17 weight)
* Expected credit conditions (0.08 weight)
* Expected capital expenditures (0.13 weight)

A separate Inflation Index is composed of the following 3 questions, with weights in parentheses:
* Price Expectations (0.5 weight)
* Inflation pass-thru (0.333 weight)
* Wage plans (0.167 weight)

In addition to the national index scores, we collect data on the state and city level. Cities are defined by the borders of their Core-Based Statistical Area, as defined by the [Office of Management and Budget](https://www.census.gov/population/metro/data/metrodef.html). We only collect data for cities and states when they have at least 50 respondents in the given survey period.

####How the data is stored####

The data from our Economic Sentiment Survey are publicly available via our API. There are three primary data sources:

1. Sentiments Data
   * These data summarize overall economic sentiment and are available on both the state and cbsa level. 
   * These data are available in JSON format on the state level [here](https://data.thumbtack.com/v1/sentiments/states) and the city / CBSA level [here](https://data.thumbtack.com/v1/sentiments/cbsa).
   * Information on the data structure is avaialable [below](#data-source-1).
2. Question Scores
   * The ESS asks a core set of 15 questions every month. The data in this section provides a breakdown of answers for each of these questions. 
   * These data are available in JSON format [here](https://data.thumbtack.com/v1/question-scores).
   * For the full list of questions and corresponding answer choices, see [below](#data-source-2).
3. Top Problems
   * These data describe the top problems faced by the small business owner-operators on Thumbtack. The options include:
      * Access to credit 
      * Interest rates
      * Access to health care or health care costs 
      * Competition from big business and overseas 
      * Competition from other small buisnesses 
      * Uncertain economic conditions 
      * Consumer confidence 
      * Acquiring new customers 
      * Cost / quality of labor 
      * Inflation Poor sales 
      * Complying with government regulations 
      * Taxes 
      * Other
   * These data are available in JSON format [here](https://data.thumbtack.com/v1/top-problems).
   * For more, see [below](#data-source-3).
   
###Data Dictionary and Sources
In addition to asking the core set of economic and business questions every month, we also ask about our respondents' demographic characteristics, including gender, age, race / ethnicity ("origin"), employer status,  and industry type. Each answer to these questions has a numerical code, as defined below:

####Demographics

    Gender    
    1 = Female  
    2 = Male  
                
    Age  
    1 = Under 25  
    2 = 25 - 34  
    3 = 35 - 44  
    4 = 45 - 54   
    5 = 55 - 64  
    6 = 65 or above    
        
    Origin   
    1 = Hispanic, Latino, or Spanish   
    2 = Asian   
    3 = Other   
    4 = White (Caucasian)  
    5 = Black or African American   
          
    Employer    
    1 = Non-employer  
    2 = Small Employer   
    3 = Large Employer   


####Industry
    0 = All Industries      
    1 = Landscaping & Lawncare      
    2 = Transportation and Moving     
    3 = Wellness         
    4 = Lessons     
    5 = Pet Care    
    6 = Cleaning    
    7 = Events    
    8 = Home Improvement      
    9 = Personal Services     
    10 = Professional Services      
    11 = Photography      
    
####Data Source 1: Sentiment Data 
This data, defined on either the state or city (CBSA) level, is a composite score of economic expecations among the small business owner-operators we heard from in the given period. The data in this section are structured as follows:
        
 Field         | Definition      
 ------------- | -------------       
 `id`          | (chr) state or cbsa     
 `start_date`  | (chr) survey start date   
 `end_date`    | (chr) survey end date      
 `count`       | (int) sample size   
 `score`       | (num) sentiment index value ranging from (0-1) based on 8 survey questions and weighted accordingly (see ESS methodology doc. for details)       
   
      
####Data Source 2: Question Scores
The ESS asks a core set of 15 questions every month. The data in this section provides a breakdown of answers for each of these questions. The data in this section are structured as follows:     

Field          |  Definition      
-------------  |  ----------------      
`id`           |  (chr) survey question id     
`levels`       |  (int) number of levels         
`scores`       |  (list) level = level number of answer; score = answer proportion selected by survey participants      
`end_date`     |  (chr) survey end date     
`start_date`   |  (chr) survey start date     
        
             
#####Questions: (`id`)
The following section defines `id` and matches its subject heading on the survey site to the survey questions and answers it corresopnds to. For example, 'Cost passed on to customers' is the subject heading on the survey site, which corresponds to id `higherCostPassOn` and the question "If your costs increased 5% today, how much of that cost increase could you pass on to customers through higher prices?", which has three levels.   
              
Costs passed on to customers (`higherCostPassOn`):
```
If your costs increased 5% today, how much of that cost increase could you pass on to customers through higher prices?   
    1 = Little or none of the cost increase         
    3 = About half of the cost increase     
    5 = All or most of the cost increase    
```       
      
Availability of credit (`pastThreeDidYou`):
```
During the past three months did you:   
    1 = Seek credit but were turned down or denied
    2 = Seek credit but refuse it because of the terms or conditions attached   
    3 = Not seek credit because you didn't think your loan application would be approved  
    4 = Not seek credit because you didn't need it      
    5 = Seek credit and receive some of the amount needed   
    6 = Seek credit and receive most of the amount needed     
    7 = Seek credit and receive the full amount needed    
```
      
Difficulty of hiring new people (`pastThreeFillEase`):         
```
How difficult or easy was it to fill the full-time or part-time job openings(s) over the past three months?   
    1 = Very difficult   
    2 = Somewhat difficult   
    3 = Neither easy nor difficult          
    4 = Somewhat easy      
    5 = Very easy   
```

Any recent hires? (`pastThreeFillJob`):
```
Have you filled or attempted to fill any full-time or part-time job openings over the past three months?    
    1 = No      
    2 = Yes     
```
      
Change in employee compensation (`threeMonthAvgComp`):
```
How do you expect your average employee compensation to change over the next three months?
    1 = Decrease a lot
    2 = Decrease a little
    3 = Stay about the same     
    4 = Increase a lot 
    5 = Increase  a little         
```
          
Outlook on business conditions (`threeMonthBizCond`):
```
How do you think business conditions in the general economy will be in three months, compared with the conditions now?    
    1 = Much worse than they are now          
    2 = A little worse than they are now      
    3 = About the same as they are now      
    4 = A littler better than they are now      
    5 = Much better than they are now     
```

Outlook on expenditures (`threeMonthCapExpChange`):
```
How do you exepct your capital expenditures to change over the next three months?   
    1 = Decrease        
    3 = Stay about the same     
    5 = Increase       
```
      
Outlook on number of employees (`threeMonthEmpChange`):
```
How do you expect the total number of employees in your business to change over the next three months?    
    1 = Decrease          
    3 = Stay about the same       
    5 = Increase        
```

Financial outlook (`threeMonthFinSit`):
```
How do you think your company's financial situation will be in three months from now?       
    1 = Substantially worse     
    2 = A little worse    
    3 = About the same as today   
    4 = A little better   
    5 = Substantially better      
```

Outlook on getting loans (`threeMonthHardLoans`):
```
Do you expect to find it harder or easier to get loans in three months than it is now? 
    0 = I don't know    
    1 = Much harder in three months     
    2 = A little harder in three months     
    3 = About the same      
    4 = A little easier in three months     
    5 = Much easier in three months       
```
      
Change of prices charged (`threeMonthPriceChange`):
``` 
How do you expect the prices you charge your customers or clients to change over the next three months?   
    1 = Decrease by more than 10%     
    2 = Decrease by 1-10%   
    3 = Stay about the same        
    4 = Increase by 1-10%       
    5 = Increase by more than 10%         
```
      
Outlook on profitability (`threeMonthProfitChange`):
```
How do you expect your company's profitability (net earnings after taxes) to change over the next three months?     
    1 = Decrease by more than 10%         
    2 = Decrease by 1-10%       
    3 = Stay about the same       
    4 = Increase by 1-10%     
    5 = Increase by more than 10%       
```

Revenue outlook (`threeMonthRevChange`):
```     
How do you exect your company's revenues to change over the next three months?        
    1 = Decrease by more than 10%           
    2 = Decrease by 1-10%       
    3 = Stay about the same       
    4 = Increase by 1-10%     
    5 = Increase by more than 10%      
```
                      
Change in employee education (`threeMonthTrainingChange`):
```
How do you expect your expenditures to improve your or your employees' training, education or expertise about your business to change over the next three months?
    1 = Decrease    
    3 = Stay about the same     
    5 = Increase          
```

Financial situation today (`todayFinSit`):
```
How would you rate your company's financial situation today?
    1 = Very bad      
    2 = Somewhat bad    
    3 = Neither good nor bad      
    4 = Somewhat good     
    5 = Very good       
```          
          
####Data Source 3: Top Problems              
Every month, the ESS also asks Thumbtack pros about the top problem their business is currently facing. The data from this question is structured as follows:
          
Field          |   Definition      
-------------  |   ----------------        
`date`         |   (chr) survey month and year     
`count`        |   (int) sample size     
`label`        |   (chr) top problem of concern for survey participants     
`score`        |   (num) proportion of survey participants who selected problem type       
 
#### Curious to Learn More About The Data?
Check out the [ESS survey website](https://www.thumbtack.com/survey#/sentiment!context=states&index=1&industry=0&view=absolute) for an interactive view of this data and [monthly reports](https://www.thumbtack.com/blog/tag/economic-sentiment-survey/) on the economic sentiments of Thumbtack Pros.

If you need help pulling this data into R or Python, we've create two tutorials to help you do so. You can access the R tutorial here and the Python tutorial here.
