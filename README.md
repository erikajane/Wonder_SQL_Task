# Wonder_SQL_Task

# Part 1: Analysis

## Analyze and extract any interesting insights you can derive from the data set attached (each row represents the assignment of a job in our research queue, including some data about the analyst who received the assignment and the current state of the research queue). What can you infer? What do you think this means for us from a business perspective?

### About the Datatable, Assignment Log

- There are 791 rows
- There are 19 columns
    - Event occured at
    - Analyst
    - Quality score (sourcing)
    - Quality score (writing)
    - Action
    - Request
    - Request created at
    - Job
    - Wait time (min)
    - Waiting for
    - Analyst available
    - Analyst occupied
    - Total jobs available
    - Review jobs available
    - Vetting jobs available
    - Planning jobs available
    - Editing jobs available
    - Sourcing jobs available
    - Writing jobs available

In order for this csv file to be converted into a SQL friendly data table, it was firt converted into a Pandas DataFrame. Once Assignment Log was converted into a pandas dataFrame a few formattin changes were made on the data so that the SQL ueries could run. The following are the format changes done on the Assignment Log data table:
    - Added id column
    - Whitespace was removed from column names
    - Spaces were replaced with underscores for column names
    - Paraentheses were removed from column names
    - Events occured at column was reformated to follow datetime format
    - Request created at was reformatted to follow datetime format
    - Events created at was converted into a datetime object
    - Request created at was converted into a datetime object

### Duplicates

There were two sets of duplicates as seen below:
![Duplicates.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Duplicates.png)

Duplicates were removed so the event only appeared once in the datatable. After removing the duplicates there are 789 rows in the Assignments table.

### Time

The first aspect I investigated was time.

#### Time for activity on an event to occur

The events logged in this datatable are between __06/21/2017 20:15:42__ to __06/22/2017 19:59:06__, about a days worth of events.

The events logged in this data table are for requests created between __06/19/2017 13:43:51__ to __06/22/2017 18:16:11__, about 3 days worth of requests.

Looking at all the requests that were created after __06/21/2017 20:15:42__, the start time of the events logged, we can see how long it took for each of those requests to begin activity. Of the 42 requests created after __06/21/2017 20:15:42__, 2 began right away, with 0 minutes before activity started. The longest it took for an activity to begin was 393 minutes. The average time it took for activity to begin on a request was about 69 minutes.

The average time for activity to begin on a request may appear high but it has to be taken into account that some requests may be made at night and are not started until the following morning. Also the size of the job and complexity of the question being asked may influence whether an analyst accepts the job or not.

#### Time between job assignment and job acceptance or denial

Within the Assignment log there are 350 occurrences where a job was assigned and either accepted or declined. The rest of the assignment occurrences have yet to be accepted or declined in the assignment log. Of those 350 occurrences, 5 had a negative time difference between when the assignment was accepted or denied. This may mean that the time the event occurred was recorded incorrectly for a few circumstances or a job was accepted before it was assigned (assignment event may not have been recorded). Of the 345 occurrences with a positive difference between assignment time and acceptance or denial time, the average time for a job to be accepted or denied after assignment is only 54 seconds. This appears quite fast.

What does this mean? Perhaps if the time to assign a job can be reduced the time for activity to begin on an assignment can also be reduced and therefore requests can be completed more quickly. 

#### Time for a request to be complete

Since this log does not record when an event is completed, my best estimate to determine the time a request would take to be competed was to calculate the time difference between when the request was created and when the final event was logged for the request. I only used requests that were created before the events in the assignment logged were recorded. I used the max Event_occurred_at time as a best estimate to when the request was completed. As a result, I found that the average time for a request to be completed was 1212.9 minutes, about 20 hours.

### Analysts

#### How many analysts are there?
There are 71 diffrent analysts recorded in the Assignment Log data table.

#### What is the quality of the analysts?

First to determine what was the quality of each analyst I had to determine if each analyst had multiple scores for Quality_scoure_sourcing and multiple scores for Quality_score_writing.

8 of the analysts have more than one score for Quality_score_sourcing and Quality_score_writing. This lets us know that these values can change. 

Another thing to note, on each row, Quality_score_sourcing is equivalent to the Quality_score_writing.

The average quality of all analysts in the data table is 3.089.


### Analysts & Jobs

- The average number of analysts available is 2.6768
- The average number of analysts occupied is 15.2218
- The average number of total jobs available is 8.0494

On average there are about 5.38 more jobs available than analysts available, this is an indication that more analysts may be needed.

#### What job is being waited on the most?

The number of jobs being waited on goes in this order:

- sourcing: 480
- writing: 361
- review: 255
- editing: 244
- planning: 214
- vetting: 145

The number of jobs being done on goes in this order:

- sourcing: 209
- review: 201
- writing: 160
- planning & vetting: 94
- editing: 31

More analysts need to be editing than planning and vetting. More writing should be done than reviewing.

### Requests

There are 74 different requests in this assignment log.

On average, 5 analysts work on each request.

# Part 2: Data Modeling

## Assuming we are starting with the data set from Part 1 as our raw data table, how would you model this data in a data warehouse for analytical purposes? What tables would you create? What kinds of questions do you imagine business users would want to ask of this data, and how would they express them in your data model? Please use whatever tools you are comfortable with to answer this question and whatever flavor of SQL you are most familiar with. A github repo or gist is preferred.

## Tables

__Table 1: Requests__
 - Requests
 - Request_created_at

__Table 2: Analyst Quality__
 - Event_occured_at (Another option is Request but I am not sure if a Analyst can do anothr job for a request after their Quality_score has been changed; If the source to quality can be connected, then perhaps this column would nit be needed and a smaller table can be stored)
 - Analyst
 - Quality_score (Quality_score_sourcing & Quality_score_writing are equivalent in all observations)
 
__Table 3: Jobs Available__
 - Event_occured_at
 - Total_jobs_available
 - Analysts_available
 - Analysts_occupied
 - Job_type
 - Number_available
 
  For this table I would be converting the following column from wide format to long format
     - Review_jobs_available
     - Vetting_jobs_available
     - Editing_jobs_available
     - Sourcing_jobs_available
     - Writing_jobs_available
 
__Table 4: Job__
 - Job
 - Analyst
 - Action
 - Request
 - Wait_time_min
 
## Business Questions

### How long does it take for activity (accepted job) to begin on a request?

![Question1.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question1.png)

The average time for activity to begin on a request is about 69 minutes or a little more than an hour.

### How long does it take for a job to be accepted or denied after it has been assigned?

![Question2.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question2.png)

On average it only takes 54 seconds for a job to be accepted or declined after it has been assigned to an analyst.

### How long does it take for a request to be completed?

![Question3.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question3.png)


There is no indication of when an assignment is completed in this log or if the activity is the last activity. For my best guess at how to calculate this, I only used requests that were created before the events in the assignment logged were recorded. I used the max Event_occurred_at time as a best estimate to when the request was completed.As a result, I found that the average time for a request to be completed was 1212.9 minutes, about 20 hours.

### How many analysts are there?

![Question4.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question4.png)

There are 71 different analysts included in the assignment log.

### What is the quality of the analsysts?

![Question5.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question5.png)

The average quality of all the analysts in this data table is 3.089133.

### How many jobs are available and how many analysts are occupied?

![Question6.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question6.png)

On average there are about 5.38 more jobs available than analysts available, this is an indication that more analysts may be needed.

### How many analysts work on a request?

![Question7.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Question7.png)

On average 4.054054 analysts work on a request.

 

# Part 3: SQL

## What are the problem(s) with this SQL query?

![Part3SQL.png](https://github.com/erikajane/Wonder_SQL_Task/blob/master/Images/Part3SQL.png)

- __WHERE order.order_date >= '20090101'__
    - In the orders table the format of the column order_date is, 'year-month-day'. The format of the date in the SQL query is 'yearmonthday'. These two formats do not match. This results in every row being returned that does not have a null value.
- __SELECT ... SUM(COALESCE(orders.order_amt, 0)) AS total_2019__
    - Using COALESCE does not make any sense in this scenario. The order of operations says the FROM and WHERE statements are executed before the SELECT statement. Therefore, there is no need to account for any null values in the orders_amt column because any rows that have null values through the join (Julie Peters) will be dropped during the WHERE statement because the date will be null as well. It would be simpler to use the statement __SUM(orders.orders_amt).

