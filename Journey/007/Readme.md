
# Day Seven

## Azure Subscriptions and Governance 

Continuing on a linear progression through Tim Duffy's AZ-104 Udemy Course.

### Section 5: Manage Subscriptions and Governance

- Subscriptions vs tenants. Resource groups. Navigating the subscription dashboard. All pretty remedial stuff from AZ-900 at the top of this section.
- Assign Azure Policies via PowerShell. Oh boy! Here we go... Scotty Duffy's example here is to find a policy to audit missing tags for resources, with the following PS command line: `Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq "Audit missing tags on resources" }` Doesn't work on my PowerShell... or in Cloud Shell, so it's not a PS configuration issue on my desktop. `Get-AzPolicyDefinition` alone works fine, and spits back a million results. So I suspect that Scott Duffy's example is based on a now out-of-date policy name. Either that our PS continues to be the bane of my Cloud Journey.
- Management Groups. Act as an organizing principle (like Resource Groups) for multiple subscriptions, nesting them to a single tenant, from which you can manage permissions and policies across multiple subscriptions at once instead of individually. 

## Lab - [Manage Governance via Azure Policy](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/LAB_02b-Manage_Governance_via_Azure_Policy.md)
*Objectives of this lab are to 1: Create and assign tags via the Azure portal, 2: Enforce tagging via an Azure policy, Task 3: Apply tagging via an Azure policy.*

- Running PS via CloudShell, we begin with the `df` command to bring up the storage account used for PS. 
- PS spits out some details on the storage acccount, and I make note of the actual `xxxxxx` path it provides for my account in the `//xxxxxxxxxxxxxx.file.core.windoNws.net/cloudshell   (..)  /usr/csuser/clouddrive` line of the results. I'm then able to find the corresponding storage account in the storage blade of the portal (which is of course easy and obvious for me since it's the only storage account, but I can see how this would have been useful if my tenant had dozens or hundreds of storage account resources). 
- Navigating up to the resource group that this storage account falls under, `cloud-shell-storage-eastus`, I then access `tags` from the RG blade. From here I complete Objective 1 by adding tags to this RG, designating it as infrastructre with the tag of `Name` = role and `Value` = infra.
- But a-ha! Even though the tag was added to the RG, it was not added to the resources within, namely the PS storage account itself. To fix that we'll have to create a policy that enforces tagging... On to objectve 2.

![Azure Policy Blade - Definitions](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/007/azpolicy.JPG)

- Navigating to the Policy blade, the objective is now to author a new policy that will require the resources to be tagged. Within the Policy blade is the Authoring section, and the Definitions sub section. This provides us with the full and very long list of pre-made policies for Azure. We need to find ones that are related specifically to `Tags`, so we use the Category drop down menu, un-select all, and select Tags. The specific policy we're looking to apply to our RG is `Require a tag and its value on resources`. 
- In order to apply this policy to our RG containing the storage account for our shell, we click Assign and set the Scope to a.) our subscription, and b.) the `cloud-shell-storage-eastus` RG.
- Before we're done with the Assign screen, we have to set up a few more Basic properties for administrative tracking purposes. a.) Change the default Assignment Name to "Require Role tag with Infra value", b.) Add the Description of "Require Role tag with Infra value for all resources in the Cloud Shell resource group" and c.) ensure that Policy Enforcement is 'enabled'. We then click Next (NOT Review + Create!) so that we can Tag the policy itself with the same `Name` = role and `Value` = infra convention that we're applying to the resource and RG itself. Lastly we make sure that Create a Managed Identity is left unchecked in the Remediation screen, and finally Review + Create the policy. And then we wait 5 - 15 min for Azure to apply it...
- *15 minutes later...* We go to the RG, add a Storage Account with all defaults, and ah-ha! It won't let us because we failed to specify the required 'role infra' tag! The policy worked!

![Policy Success](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/007/policysuccess.JPG)
- Now we're on to objective 3: Apply tagging via an Azure policy. Ah the sweet smell of administrative policy via automation, I can almost taste it. Our goal here is to now apply a more robust tagging policy to our RG that won't just stop us from creating resources without the required tags, but will remediate the lack of appropriate tagging automatically in the creation process. 
- In the portal we navigate back to Policy -> Authoring -> Assignments, and delete our previously created policy. 
- Click Assign Policy to start again. Same steps as before for the Scope, selecting out Subscription and Resource Group. 
- In Policy Defintion, we search for and select `Inherit a tag from the resource group if missing`, add a description (I used the policy name) and ensure enforcement is enabled.
- Clicking next, we add the tag of 'Role'. Next again, in Remediation we click the option for 'Create a Remediation task' which will autopopulate 'Inherit a tag from the resource group if missing' as the policy to be remediated (which seems redundant here, but I can imagine the application for having a policy built that remediates an entirely different policy). And then we wait another 5 - 15 minutes...
- While waiting impatiently, I discover that I can navigate back over to the Resource Group blade and select the Policies for this particular RG. My new policy is listed, but with the 'Compliance State' status of 'Not Started'. That's cool! I can clearly reference whether or not it's been applied yet and check back periodically instead of just guessing. A few minutes and F11s later, the Compliance State has changed to 'Non-Compliant'. 
- Repeating the previous steps, I create a Storage Account in the RG, changing none of the defaults. Nothing stops me from completing the creation this time. Navigating back to the RG, I can see the new 'storagedemo321' storage account I created, complete with the intended role infra tags! Automated policy remedition success!! 
