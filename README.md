# ENRON EMAIL - CODING CHALLENGE FOR SLACK
### by Daniel Kristiyanto - Palo Alto, Autumn 2016

## Intro
Enron, the infamous energy company that went bankrupt in the early 2000s after engaging in massive accounting fraud, was a heavy user of email. As part of the government’s investigation into Enron’s malfeasance, a large number of emails written by Enron’s executive team was released to the public, and this corpus has since become a useful resource for computer scientists, computational linguists, and sociologists.

You will want to download an annotated sample of the Enron emails from UC Berkeley (4.5 MB, tar.gz), and you can find the entire data set and a collection of analyses/papers that have been written about it at the Carnegie Mellon site if you’re interested. We’d like to answer the following questions about the email messages provided in the sample from Berkeley:

1. How many emails did each person receive each day?
2. Let's label an email as "direct" if there is exactly one recipient and "broadcast" if it has multiple recipients. Identify the person (or people) who received the largest number of direct emails and the person (or people) who sent the largest number of broadcast emails.
3. Find the five emails with the fastest response times. (A response is defined as a message from one of the recipients to the original sender whose subject line contains all of the words from the subject of the original email, and the response time should be measured as the difference between when the original email was sent and when the response was sent.)

We would like you to write a program (in a language of your choice) to extract the fields you need to answer these questions from the email messages, and then a set of SQL queries (or if you prefer, MapReduce/Spark jobs) that generate the answers to our questions on the resulting data. Remember that this is messy, real-world data, so you’ll have to make some decisions about how to handle certain edge cases that you encounter. Please note where you made an assumption about the data and provide a brief justification for the choice you made.


# ANSWERS
Please refer to [queries](queries) and [results](results) for more detailed information.  
1.  Please refer to [results\Email_Per_Day.csv](results\Email_Per_Day.csv)  
2.  A: maureen.mcvicker@enron.com Received the largest of direct email with the total of 115 emails.  
    B: dan.wall@lw.com sent the most broadcast email with an average of 104 recipients per message. As within Enron email corporation only, angela.wilson@enron.com sent most broadcast emails with average of 64 recipients per email  

3. As follow:  

| Date (Sender1)      | Date (Sender2)      | Respond Time (seconds) | Subject                                                                 | Sender1                   | Sender2                     |
|---------------------|---------------------|------------------------|-------------------------------------------------------------------------|---------------------------|-----------------------------|
| 2001-11-21 08:52:26 | 2001-11-21 08:49:58 |                    148 | FW: Confidential - GSS Organization Value to ETS                        | stanley.horton@enron.com  | rod.hayslett@enron.com      |
| 2001-05-10 06:55:00 | 2001-05-10 06:51:00 |                    240 | RE: Eeegads...                                                          | jeff.dasovich@enron.com   | paul.kaufman@enron.com      |
| 2001-11-21 10:59:36 | 2001-11-21 11:12:04 |                    748 | RE: Confidential - GSS Organization Value to ETS                        | rod.hayslett@enron.com    | morris.brassfield@enron.com |
| 2001-10-26 09:13:36 | 2001-10-26 09:28:56 |                    920 | RE: CONFIDENTIAL Personnel issue                                        | lizzette.palmer@enron.com | michelle.cash@enron.com     |
| 2001-11-14 15:24:08 | 2001-11-14 14:34:57 |                   2951 | RE: PG&E PX Credit Calculation -- CONFIDENTIAL ATTY CLIENT WORK PRODUCT | d..steffes@enron.com      | alan.comnes@enron.com       |


# Folder Structures:
**mysql**               : Schema for MySQL  
**queries**             : Queries to answer the questions  
**rawdata**             : Extracted Data of [Enron Emails from UC Berkeley](http://bailando.sims.berkeley.edu/enron/enron_with_categories.tar.gz)  
**enronparser**         : contains script downloaded from [UC Berkeley](http://courses.ischool.berkeley.edu/i290-2/f04/assignments/enronEmail.py), written by Andrew Fiore to Parse Enron email to Dictionary.  
  
**Parse_and_Load_Data_To_MySQL.py** : A python script to load the data into MySQL  
**runMySQL.sh**         : A shell script to run MySQL as a Docker Container. 
**run-spark-python.sh** : A shell script to run Spark-enabled Python Docker Container --Only used to debug/further analysis. Not used for subscribtion.  
**README.md**           : This file.  

This submission is also available on GitHub: [https://github.com/kristiyanto/enron](https://github.com/kristiyanto/enron) (Currently Private only)

# Requirements
1. MySQL (Latest version)
2. Python 2.6, Packages: dateutil, mysql.conector, email.utils, pytz

# Architecture
Emails were downloaded from [Enron Emails from UC Berkeley](http://bailando.sims.berkeley.edu/enron/enron_with_categories.tar.gz), and extracted. A python script with leverage a parser (also from UC Berkeley) was used to load the email data into MySQL. 
To answer the questions, SQL queries were performed. Results are saved as CSV in results folder.

- Please refer to [schema.sql.txt](mysql/schema.sql.txt) for more detailed about the SQL schema
- Please refer to [Parse_and_Load_Data_To_MySQL.py](Parse_and_Load_Data_To_MySQL.py) for more detailed information about how the data were loaded to MySQL
- Please refer to [queries](queries) folder for more detailed information for each queies


# Assumptions
Within each query (in queries folder), a more detailed information is provided, but in general:
- All emails (both Enron-specific and external emails) were treated equally
- Sender with average recipients over time are considered as the largest number of broadcast email
- Only DISTINCT recipients are performed (e.g if a recipient is included in both To and CC, is considered as 1 recipient)
- Some of the data show wrong dates (back in 1979), this data was not treated differently --assuming that there was some explanation for this mistake (e.g. Server misconfiguration, or Millenium bug/Y2K problem.). Given that there was no email sent/received between Dec 29, 1999 - Jan 3, 2000, Y2K millennium bug may be considered as the problem, however, a further investigation needed. Reference: https://www.britannica.com/technology/Y2K-bug
- Although some email may appear unusual, e.g. email address containing apostrophes or two dots, these addresses are a valid address and often used for internal usage. These emails were treated no differently. More Info: https://tools.ietf.org/html/rfc5322 


# Contact 
Daniel Kristiyanto, danielkr@uw.edu

