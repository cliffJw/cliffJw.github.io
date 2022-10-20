---
title: "Coding Assessment - Notes"
date: 2022-10-02 07:06:00 +0530
categories: [Assessments]
image: /assets/img/Posts/Assessment/assessment.png
tags: [Python,User Journey,Analysis]

---

> This is a coding assessment that I did. It entails performing analysis on a given dataset and deriving useful and actionable insights. The writeup covers setup of the assessment environment and the actual analysis/assessment.


****

## Setup and Resources For the assessment
### *Setup*
I am running on Parrot 5.1 as my primary operating system. This task can however be done on any other operating system with python3 installed and virtual environment set up accordingly.
![](/assets/img/Posts/Assessment/pcspecs.png)

To perform analysis and answer the questions asked in the assignment,  i will go ahead and create a virtual environment  where I will install all the required libraries such as pandas, matplotlib, and numpy. I will need  a jupyter note book for writing and executing code.

**Step 1: Creat virtual environment and then activate it**
```
┌─[admin@pc]─[~/Desktop/assessment]
└──╼ $virtualenv .venv
created virtual environment CPython3.9.2.final.0-64 in 524ms
  creator CPython3Posix(dest=/home/admin/Desktop/assessment/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/admin/.local/share/virtualenv)
    added seed packages: pip==22.2.2, setuptools==65.3.0, wheel==0.37.1
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
┌─[admin@pc]─[~/Desktop/assessment]
└──╼ $. .venv/bin/activate
(.venv) ┌─[admin@pc]─[~/Desktop/assessment]
└──╼ $

```

![](/assets/img/Posts/Assessment/virtualenv.png)

**Step 2: Install  jupyter notebook, pandas,  matplotlib, openpyxl, and  numpy**
![](/assets/img/Posts/Assessment/jupyter.png)

**Step 3: Run jupyter notebook and start your project**
![](/assets/img/Posts/Assessment/notebook.png)

Follow the link to the browser and open your notebook. "test.ipynb"
![](/assets/img/Posts/Assessment/20220922131032.png)


# The Assessment
For the assignment, I was provided with two documents.
1. An excel document containing the dataset "20220919 Software Developer Interview Assignment Data.xlsx", which I renamed to ***data.xlsx***
2. Instructions on a PDF file, ***"20220919 Software Developer Interview Assignment.pdf"***

## Preliminary Analysis
### Manual Observation
We begin the analysis of the given dataset by first opening the excel file in an excel reader application for preliminary observations and deduction. Having opened the file, it is observed that the dataset contains 5 columns:
1. Datetime: Contains datetime information
2. UserID: contain 32 character values encoded in base 16
3. Original Text: Blank column
4. Translation: Blank column
5. Intent: NLP assigned intent
6. Confidence: Confidence level of the NLP incategorizing the messages into the assigned Intents. Float.

![](/assets/img/Posts/Assessment/20220922070812.png)

### Obtaining More details from the dataset
After manually observing the dataset, we can now perform automated preliminary analysis of the dataset by reading the file using pandas; a python library for data analysis. After reading the data, we can use info(), head(), and tail() methods to assess the data.

![](/assets/img/Posts/Assessment/20220922085434.png)

![](/assets/img/Posts/Assessment/20220922074859.png)

#### <strong>General Assessment Results</strong>
<ul>
    <li>The Dataset has six columns and 113385 rows</li>
    <li>The memory usage of the data is about 5.2 MB</li>
</ul>

#### <strong>Quality Assessment Results</strong>
<ul>
    <li>Original Text and Translation columns all have missing values</li>
    <li>Confidence column contains some incorrect values:NaN instead of floating point values</li>
    <li>Floating point and integer values in the Intent column instead of text</li>
    <li>There are some missing values in the Datetime column</li>
</ul>


#### <strong>Tidiness Assessment Results</strong>
<ul>
    <li>Datetime column has two values: Date and Time</li>
</ul>


## Data Cleaning
Original Text and Translation columns are droped  since they both have  missing values.
![](/assets/img/Posts/Assessment/20220922075007.png)
The Datetime column also contain null values that should be filled with arbitrary values. For this case, we assign the communication start date to the null datetime values. We then check data info to see if Datetime, UserID, and intent columns have same rows.
![](/assets/img/Posts/Assessment/20220922081450.png)
We can now find the duration for the entire communication by subtracting the start_date from the final date.
![](/assets/img/Posts/Assessment/20220922081947.png)
The analysis reveal that the duration for communication captuyred in the dataset is 92 days, 20 hours and 19 minutes.

# Constructing User Journey
Having performed preliminary analysis and cleaned the data, we can now begin creating user journey for each patient/user. We begin by converting the dataframe into a JSON list for ease of processing with python.

The data is filtered to leave only users with more than 1 question. We then format the data into a multi-index frame.
![](/assets/img/Posts/Assessment/20220922092527.png)
![](/assets/img/Posts/Assessment/20220922092710.png)
At this stage the data still has duplicate UserIDs. We then remove the duplicates to have each user mapped to respective intents with corresponding dates. This gives the user journey containing intents and corresponding dates and time as shown in the screenshot below.
![](/assets/img/Posts/Assessment/20220922093323.png)
Having obtained the user journey in the table format. we can then write it to an excel file for ease of use for any consumer including a person with little technical capabilities. The excel file is saved as assessment.xlxs.
![](/assets/img/Posts/Assessment/20220922094540.png)
# Further Analysis
Exploring the data further, we begin by determining the duration for the communication, which we found to be **92 days, 20 hours and 19 minutes**. We then determine the number of patients that asked questions by counting the UserIDs within the dataframe.
![](/assets/img/Posts/Assessment/20220922105749.png)
We establish that during the period of 92 days 20hours and 19 minutes, a total of **86710** patients asked more than 1 question. 

Performing correlation analysis revealed that there is no correlation between the intents.  
![](/assets/img/Posts/Assessment/20220922112640.png)
![](/assets/img/Posts/Assessment/20220922112723.png)

## Visualization of intent counts
![](/assets/img/Posts/Assessment/20220922112910.png)
From the visualized counts of intents, it is observed that questions with "ok_thanks", "qsurvey_positive", and  "survey_response" intents were the most asked during the period.

Questions with "default_fallback" and "danger_signs" intents were the least asked.

Counting patients per intent in within the period gives the following results.
![](/assets/img/Posts/Assessment/20220922114220.png)
plotting the data gives the folllowing bar chart.
![](/assets/img/Posts/Assessment/20220922114411.png)

It is equally visible that most patients asked questions relating to ok_thanks", "qsurvey_positive", and  "survey_response" intents. The least number of patients asked questions relating to "default_fallback"

<center><strong>End</strong></center>

