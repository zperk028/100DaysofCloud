**Add a cover photo like:**
![placeholder image](https://via.placeholder.com/1200x600)

# Day 5 (20OCT20)

## [Azure Identity Management and Governance (via CloudSkills.io AZ-104 Exam Prep Course)](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course)

✍️ (Why) Explain in one or two sentences why you choose to do this project or cloud topic for your day's study.


### [Part 3: Manage Subscriptions and Governance](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course/categories/2692676/posts/8980104)

- Policies and RBAC: Inherit from the scope above (eg: cascade down from subscription to resource groups, etc)
- Taxinomic tags: Do not! They are extremely useful for billing specific resources for specific clients within a subscription, but have to be added and configured manually. 
- Cost Management + Billing blades is used to create a budget or forecast your spend.
- Azure Advisor in the Cost Management + Billing blade can give basic recommendations. If more detailed consumption details and optomization advice (beyond just a cost analysis chart, possibly with "machine learning"), are needed, then the Azure element you're looking for is Cloudyn. 
- Moving Resources and Dependent Resources to New Subscriptions: To move a VM and it's Network Interface Cards to a new subscription you must move all dependent resouces as well. Focus studying on options for moving VMs and NICs within a Vnet, between Vnets, and between Subscriptions. 
