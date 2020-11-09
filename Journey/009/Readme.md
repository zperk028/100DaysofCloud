# Day Nine

## Section 6: [Monitor Resources by using Azure Monitor (Part 2)](https://www.udemy.com/course/70533-azure/learn/lecture/12322450#overview)

Continuing with Scott Duffy's Udemy AZ-104 course, we're on to Section 6.

### Action Groups

- Back [Part 1](https://github.com/zperk028/100DaysofCloud/edit/main/Journey/008/Readme.md) of Section 6, we covered how to Create Alerts. In that example, we looked at creating an alert so that when a certain performance metric dropped on a specific resource, it would initiate an Action where the admins of that resource group would be automatically notified via email or SMS. When that alert was created with an associated action, it automatically populated an Action Group.  
- Action Groups are essentially if/then functions that are all about automating administrator actions. Multiple actions can be assigned, and far beyond the straight forward "email me/SMS" example above. 'callfunctions' for Actions can include action types from Azure Functions, LogicApp, Webhook, ITSM, and Automation Runbook, so long as you have those resources already created and available. 

### Manage Costs

- Besides the usual on Cost Management + Biling and Cost Analysis, of note here is the Cloudyn service.  Cloudyn was a third party app that Microsoft purchased, and is now available to access as a service from within the Azure Portal.  Cloudyn helps users manage cloud costs across multiple platforms in one place, so if you work somewhere that is using some combination of Azure, AWS and GCP, Cloudyn can be extremely useful. 

### Log Analytics 

![AZ Log Analytics Workspace](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/009/AZloganalyticsqueries.JPG) 

- In order to use the Log Analytics service, Azure creates the underlying fundamental resource for it, called a Workspace.
- Once the Log Analytics Workspace is fully configured (resources are connected and logging to it), you have the ability to run queries as if the log is a database, in the same fashion as running SQL queries. 
- Alerts tie-in directly to Log Analytics - you can 'Set Alert' to create new alerts directly from the Workspace. 
- Scotty Duffy interestling makes the comparison between AZ Log Analytics and Splunk (which I'm familiar with as a cybersecurity SIEM platform), which makes me excited to focus more on AZ logging and the Log Analytics service. 
