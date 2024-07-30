# Splunk Lab

## Description
In a hypothetical role as a security analyst at Buttercup Games, I was tasked with identifying potential security issues with the mail server. My focus was to investigate any failed SSH login attempts for the root account. To conduct this analysis, I was provided with a zip file containing company data to be analyzed in Splunk.

### Skills Learned
- Developed the ability to analyze log data effectively to detect failed SSH login attempts and other potential security issues.
- Proficiency in using Splunk for data ingestion, indexing, and performing advanced search queries to identify security threats.


## Process

### Uploading Company Data to Splunk
First, I needed to upload the company file to Splunk. Below are the steps I took to accomplish this. 
1. On the home page, I clicked on settings at the top and selected <b>Add Data</b> from its menu.
<br>

![image](https://github.com/user-attachments/assets/f30f2406-9ea0-4bbd-93ea-27988463e7c2)

2. Then, I selected <b>Upload</b>.
<br>

![image](https://github.com/user-attachments/assets/f0d9fb8f-d83c-4268-8167-6e41890de2f1)

3. I clicked through the steps and kept the default settings except for the following difference:
<br>

![image](https://github.com/user-attachments/assets/ae364179-ffcc-4b9c-b8a5-f5c825617d8c)

4. This was my final configuration for the file. I verified everything was correct and clicked <b>SUBMIT</b>.

<br>

![image](https://github.com/user-attachments/assets/16406bd8-bef0-4f02-8d19-ece33ee1c284)

### Performing a Basic Search
Now that the data was uploaded, I could continue my investigation of the mail server. I first needed to confirm that the uploaded data was properly ingested and indexed. To determine this, I took the following steps to perform a quick query:
1. I clicked the <b>Search and Reporting</b> tab in the app panel on the left.
<br>

  ![image](https://github.com/user-attachments/assets/10551f3b-3a31-4c66-aede-51ce3c69d974)

2. I entered <b>index=main</b> in the search bar, specifying that I only wanted to view a dataset containing events from the main index. I wanted to view all events across all time for this index. I selected “All Time” in the time dropdown menu.

<br>

![image](https://github.com/user-attachments/assets/85bca05a-12e0-4423-8d50-fc64dabcb2e4)

<br>

3. I returned over one hundred thousand events after entering the search. I would need to implement more specific search terms and other modifiers to view meaningful data in future queries. However, this was enough to confirm that Splunk ingested my uploaded data properly.
<br>

![image](https://github.com/user-attachments/assets/029c5bf0-a7e5-4acb-bd0b-c3469eda0293)

### Evaluating the Fields

When I processed data with Splunk, it assigned fields to each event, making them part of the searchable index. This helped me quickly find the specific data I needed. After running my first query, I reviewed the search results and their fields.

Below is the information I found by looking through <b>SELECTED FIELDS</b>:
- <b>host</b>: Specifies the network host from which the event originated. There were five sources:
 - - <b>vendor_sales</b>: Information from the company’s sales team
 - - <b>ww1</b>, <b>ww2</b>, <b>ww3</b>: appear to be web applications owned by Buttercup Games
 - - <b>mailsv</b>: ButterCup Game’s mail server. My task was to investigate this host.
- <b>source</b>: Indicates the file name from which an event originates. There were eight total sources, but <b>/mailsv/secure.log was significant as it contained information related to authentication attempts on the mail server.
- <b>sourcetype: Determines data format. The only significant one was <b>secure-2</b>.

![image](https://github.com/user-attachments/assets/a5718ca9-a5c4-4d0e-a714-fe0ada16b869)

### Narrowing My Search to Find Failed Login Attempts on Root
My task was to explore any failed SSH logins for the root account on the mail server. Now that I had familiarized myself with the Splunk interface and determined that the mail server was named <b>mailsv</b>, I wrote a more specific query in the search bar: <b>index=main host=mailsv</b>.

<br>

![image](https://github.com/user-attachments/assets/3a114347-8933-4295-b362-66bf60891a3d)

(Note that clicking <b>SELECTED FIELDS →host→mailsv</b> would return the same outcome)

<br>

This search narrowed down the results to 9,829 events. I only wanted to locate any failed SSH attempts for the root account. I added the keyword <b>fail*</> with the wildcard included to tell Splunk to search for all terms that contained the word “fail” (such as “failed” or “failure”). More importantly, I included the keyword <b>root</b> to search for all root-related events. I added <b>sshd</b> to only view SSH data. The search term and its output are displayed below. 

![image](https://github.com/user-attachments/assets/ff817e24-22e0-4242-9781-f8edc03ddb8c)
There were over three hundred attempts from various IP addresses.

## Conclusion
Splunk allowed me to efficiently identify over three hundred failed SSH login attempts for the root account from various IP addresses. This investigation highlights the importance of regular log monitoring to detect and respond to potential security threats. By leveraging Splunk’s powerful search capabilities, I quickly found security issues, demonstrating the value of data analysis tools in maintaining a secure environment.
