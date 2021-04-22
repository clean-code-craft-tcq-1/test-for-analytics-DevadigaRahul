# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked '_enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetric in a csv file  
 - a)	Server availability(Online)
 - b)	Availability of csv file
 - c)	Read access to csv file
 - d)	where to store weekly PDF report
 -  - write access to store PDF_report(if to be stored in server)
 - e)what/which day of the week PDF_report must be stored
2. Data-accuracy cannot be validated 
 - a)	Data read from sensor may be corrupted.
 - b)	Sensor may be faulty
 - c)	Corrupted csv file 
3. Data format:
 - a)Data in csv should be compatible with defined internal data-structure(format and parameter miss match)

4.Other 
 - Input arguments(sent to utility function) data mismatch(format and parameter) with 
	- PDF creation utility
	- Notification utility 
 - PDF creation utility [raw data to PDF]
 - Notification utility [Notify end user/admin]

(add more if needed)

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No            | PDF creation utility could be mocked
Counting the breaches       | Yes           | This is part of the software being developed
Detecting trends            | Yes           | This is part of the software being developed
Notification utility        | No            | Notification utility could be mocked

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:
1. write "Server connection error" to the "raw_report" when server_client_handshake fails (Server denies access to client)
2. Write "CSV File Missing" to the "raw_report" when the csv file is not available/stored in server
3. Write "CSV File access denied" to the "raw_report" when the client dose not have read access to csv file in server
4. Write "Invalid input" to the "raw_report" when the csv doesn't contain expected data
5. Write minimum and maximum to the "raw_report" from a csv containing positive and negative readings
6. write "count of breaches" to the "raw_report" from a csv which contains the readings that crosed threshold in a month
7. write "trends(date & time)" to the "raw_report" from a csv which contains the readings that are continuously increasing for 30 minutes
8. triger "raw_report" to PDF conversion,when defined/agreed day of week for storing PDF report.[Assuming we are storing PDF report in local machine]
9. Notification tests
 - a. trigger Noficialtion utility(Notify "PDF report" to end user/admin), once "PDF report" available.[Positive scenario ]
 - b. trigger Noficialtion utility(Notify "PDF report:File Missing" to end user/admin),if PDF_report is not available for agreed max time-out (for given/current week)[Negative scenario]


### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | PDF_report   | Notification sent[to end user or admin]| mock-Notification utility  
Report inaccessible server | error code   | report appropriate error[based on error code]| Fake - server [which deny access and returns(error code)]
Find minimum and maximum   | csv data     | minimum and maximum  | None - it's a pure function
Detect trend               | csv data     | list of Detect trend | None - it's a pure function
Write to PDF               | raw_report   | PDF_report           | mock-PDF creation utility
