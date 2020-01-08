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
![Duplicates.png]{}

Suplicates were removed so the event only appeared once in the datatable. After removing the duplicates there are 789 rows in the Assignments table.


    


The events logged in this datatable are between __06/21/2017 20:15:42__ to __06/22/2017 19:59:06__, about a days worth of events.

The events logged in this data table are for requests created between __06/19/2017 13:43:51__ to __06/22/2017 18:16:11__, about 3 days worth of requests.

Looking at all the requests that were created after __06/21/2017 20:15:42__, the start time of the events logged, we can see how long it took for each of those requests to begin activity. Of the 42 requests created after __06/21/2017 20:15:42__, 2 began right away, with 0 minutes before activity started. The longest it took for an activity to begin was 393 minutes. The average time it took for activity to begin on a request was about 69 minutes.

Since this log does not record when an event is completed, the best estimate to determine


## Suggestions:
-Log when an assignment is complete

## Questions:
- What