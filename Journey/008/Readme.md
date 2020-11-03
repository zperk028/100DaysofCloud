# Day Eight

## Section 6: [Monitor Resources by using Azure Monitor (Part 1)](https://www.udemy.com/course/70533-azure/learn/lecture/12322450#overview)

Continuing with Scott Duffy's Udemy AZ-104 course, we're on to Section 6.

### Diagnostics

- For Virutal Missions, within the VM's specific resource blade, there is a menu called 'Diagnostic Settings'. This will allow access to the Diagnostic Agent and Overview of basic (and custom) diagnostice Performance Counters. For VMs, the basic Performance Counters are CPU, Memory, Disk, and Network. 
- Custom will allow you to get deep into the weeds for a huge amount of possible Performace Counters. It sounds like these settings affect your resource logs. 
- Logs - yep, that's exactly what these Performace Counter settings are for. This all feeds into what are essentially Windows Event Logs and what is captured by the logs for this VM.

### Baseline Environment

- "Baseline Environment" = an ARM template of your resources for the baseline of your system before you eff around with it.
- From the RG blade, the Azure Portal allows you to track past deployments to an RG of ARM templates. From the Deployments section of the RG, it will show the deployment log, and baseline ARM template of that deployment, including the JSON script in one of four languages (PS, CLI, etc). 
- As you build resources, it's a good idea to create blueprints of them as PS scripts to back-up the resources of your baseline environment, for example by pushing those PS scripts to GitHub.

### Create Alerts (in Azure Monitor)

- From Azure Monitor (or directly from within a specific resource in the Portal) there is a section to Create Alerts. Alerts can be set via an alert logic (event level, status, initiated by; ex: an alert for when a webapp stops running). 
- The alerts created are themselves resource objects, by default stored in the same RG as the associated resource. Because it is itself a resource, the alert is given a name and description. 
- An Action is also defined for the alert (ex: email admins with the Azure Resource Manager role of Owners). The alert resource can also be assigned to a new RG (an RG made specifically to house alerts) from the page where actions are defined. 
- Activated alerts will be logged in the activity log. 

### Metrics (in Azure Monitor)

- From Azure Monitor you can select a specific resource (or directly from within that resource, just like Alerts), and from the Monitor section you can generate an on the fly report of a specific attribute of that resources - everything from CPU usage for a VM, to I/O, to webapp requests.  These metric reports are in the form of a chart and can be pinned to your dashboard for targetted monitoring. 
