![Lab 11 redux](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/011/lab11d.JPG)

# Day Eleven (aka Day 9 revisted) 

## Introduction

Revisting my issues with [Lab 11: Implement Monitoring in Day 9](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/009/Readme.md), in which I failed to successfully deploy a VM from an ARM template and was always left with an empty resource group.  It turns out I'm a moron and the problem was caused by the fact that I didn't know how to correctly download a .JSON file from GitHub. Here's what I learned is the correct way to do it:
1. DON'T RIGHT-CLICK A JSON FILE IN A GITHUB DIRECTORY AND CLICK 'SAVE AS'. If you open that file in Visual Studio you will see a ton of additional junk in the code. That is wrong. Don't do it. 
2. Click through on the JSON file so that you actually see the code/content of it, then click the `Raw` button on the top right. 
3. `Raw` opens the file in a clean browswer window containing nothing but the code. 
4. From the clean window, ctrl + s and download. You now have the proper JSON file.

## Back to [Lab 11: Implement Monitoring](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/LAB_11-Implement_Monitoring.md) 
After repeating Tasks 1 to 3, succesfully this time, we pick back up at Task 4. 

![It Lives!](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/011/lab11.JPG) 

4. Reviewing the default monitoring setting of our *successfully deployed* VM (which, by the way, is a Windows Server 19 Datacenter). In the VM blade we access the Monitoring section and go into Metrics. From there we create a chart that displays the CPU usage (Metric: Precentage CPU, Aggregation: Avg). 

5. For our fifth task in the lab, we will next configure the VM's diagnostic settings. Still in the Monitoring section of the VM's blade, we click into Diagnostic Settings and enable guest-level monitoring. Once deployed, we swtich to the Performance Counters tab and ensure that the CPU, Memory, Disk and Network of the machine are being sampled. Next to the Logs tab of Diagnostic Settings, where we confirm that Application: Critical, Error, Warning, Security: Audit Failure, System: Critical, Error, Warning are all configured for logging. We then go into the Logs blade of the VM under the Monitoring section and enable the feature. Over to the Metrics blade in the Monitoring section, we are now given the 'Guest (Classic)' option in the Metrics Namespace drop down menu, which gives a new set of options for trackable Metrics. Configuring it to show memory in available bytes (Metric: \Memory\Available Bytes, Aggregation: Max), we're presented with new pretty chart to review our VM as proof of life.

![Still alive](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/011/lab11a.JPG)

6. Now it's time to review Azure Monitor functionality and really get into it. In the Portal we search for the Monitor feature, and from there click into Metrics, prompting us to select a scope while be presented with a list of all of our resource groups. We expand the az-104-11-rg0 group where the VM and it's associated resources (NIC, VHD, etc) are housed, and click to add a checkmark specifically to the VM resource itself. Clicking 'apply', this takes us to the same chart creation screen for Metrics. Like before, we configure Metric: Precentage CPU and Aggregtaion: Avg, but this time we also click New Alert Rule at the top right above the chart. We're given an incomplete condition in the alert window, based on the CPU precentage monitor graph we were previously working with. Clicking through on that condition, we finish the configuration by entering a Threshold Value (precentage) and changing the Aggregation granularity (time Period) and Frequency of evaluation. After clicking done, back in the Create Alert rule window we click Select Action Group and Create Action Group. After affiliating it with the logging RG (rg1) we set up SMS/voice/email and configure to email our admin address. Back on the Create alert rule blade we scroll down to Alert rule details and configure our alert with some defaults to indicate the CPU precentage has gone above the threshold of 2%. Going back to the az104-11-vm0 VM blade, we click 'Connect' and RDP, and download an RDP file. 

![Remote inside the VM](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/011/lab11c.JPG)

From the RDP into the VM, the lab calls for me to enter the following into the VM's command prompt to spike the CPU use and trigger an AZ alert: `for /l %a in (0,0,1) do echo a`.  From the VM's task manager I can see the CPU cookin.  I close the command prompt after a few minutes, and notice that I only have one email alert (time stamped from before I ran the command prompt process, likely the CPU firing on start up of the VM) and leave the VM up over night.  In the morning, I finally have some email alerts, but I notice that the emails come in sporadically and have a big delay compared to the actual alert log in the Monitor blade. Good to know for the future that you can't rely on these email alerts for real-time monitoring. 

7. On to the final task. Heading to Logs in the Monitor blade, and set the scope to the VM as an individual resource scope. The lab then provides us with the following query to run: 

`// Virtual Machine available memory
// Chart the VM's available memory over the last hour.
InsightsMetrics
| where TimeGenerated > ago(1h)
| where Name == "AvailableMB"
| project TimeGenerated, Name, Val
| render timechart`

![Memory available query](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/011/lab11e.JPG)

This outputs a chart for us that displays memory available over time in the VM. The dips in memory available roughly match the spikes in CPU usage, as expected. 

## Cleaning Up
Alright! We did it! After a few false starts, we completed Lab 11, finally deployed a real live VM, and learned how not to be a bone head with ARM templates and JSON files on GitHub. Now it's time to clean up the lab resources so they don't cost me any more money. Using the PowerShell command `Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob` we initiate our clean up and delete the rg0 and rg1 resource groups.
