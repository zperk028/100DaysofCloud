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

## Lab: [Implement Monitoring](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/LAB_11-Implement_Monitoring.md) 

*This would arguably be a better lab to be running after I had already created an actual project/RG of substance on AZ, but alas, here we go. We'll be setting up Azure Monitor and Log Analytics for some VM resources.* 
*Tasks for this lab will be 1: Provision the lab environment, 2: Create and configure an Azure Log Analytics workspace and Azure Automation-based solutions, 3: Review default monitoring settings of Azure virtual machines, 4: Configure Azure virtual machine diagnostic settings, 5: Review Azure Monitor functionality, and 6: Review Azure Log Analytics functionality.*

1. Provision the lab environment and deploy a VM. Using PS in AZ Cloud Shell, upload the two JSON files provided for the lab (I wonder how I would have done this via PS in Windows...) a template JSON and a parameters JSON. These files are now in the Cloud Shell home directory. 

![uploaded JSON files for Lab](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/009/azPScloudshelldir.JPG)

Then we set up the RG via PS: `$location = 'EastUS'
$rgName = 'az104-11-rg0'
New-AzResourceGroup -Name $rgName -Location $location` 

And deploy the pre-made VM resources via the template and parameters from the JSON files we uploaded earlier, via PS: `New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/az104-11-vm-template.json `
   -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
   -AsJob` 
   
![Deploying in Cloud Shell PS](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/009/aztemplatedeploy.JPG) 

We then need to register the Microsoft.Insights and Microsoft.AlertsManagement resource providers with the following command. 
`Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement`
I'm going to be honest, I'm not 100% sure what this is does yet.

2. Next task, create and configure an Azure Log Analytics workspace and Azure Automation-based solutions. In the portal we navigate via search bar to the Log Analytics workspaces blade, click 'Add' and configure the workspace as indicated in the lab instructions. Note: It's telling me to use the RG for the analytics workspace as 'az104-11-gr1', but the existing RG for the lab that we deployed with the JSON templates is 'az104-11-gr0'. I'm not 100% sure if that's a typo or if it's telling me to greate a new RG specifically for the workspace. I'm going with the new RG for now. 

3. Task 3, using the Azure Portal to setup the Azure Log Analystics workspace and Azure Automation. Both are done using their respective blades and tagging it to the new RG, az-104-11-gr1, taking special care to set the log analytics workspace to region East US and automation to East US 2, as each service is only allowed in those respective regions. 

4. And now I've hit a hiccup. I'm supposed to configuring the virtual machine deployed by the az-104-11-gr0 template, but there's nothing in the resource group. I checked my powershell commands and tried a few variants. No joy. I've committed an issue to the lab on github, and will have to wait and see. 

![dafuq](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/009/azrgdeploydafuq.JPG)
