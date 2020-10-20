![From MS Learn module](https://docs.microsoft.com/en-us/learn/modules/manage-users-and-groups-in-aad/media/2-azure-vs-windows-ad.png)

# Day 4 (19OCT20)

*Note: Due to work emergencies at the end of last week and family commitments over the weekend, I'm picking back up on Day 4 despite the 4 days between now and my Day 3 entry. My #100DaysofCloud entries will be tagged to the date that I do the work and I'll complete the 100 Days on my own schedule, not in 100 consecutive days.*

## [Manage identities and governance in Azure (via Microsoft Learn)](https://docs.microsoft.com/en-us/learn/paths/az-104-manage-identities-governance/?source=learn)

### Module 2: Manage users and groups in Azure Active Directory

Picking up with Module 2 of this MS Learn path.

- Azure AD is not simply a cloud version of Windows Server AD, or a replacement for it on-prem. 
- Using Azure AD exclusively (for example in a small company) is an option if they're primarily using cloud based SaaS.
- Question For Myself/Project Idea: Can I build a home lab running Windows Server AD, and expand it out into a hybrid cloud with my free Azure account? 
- Digging into Azure Resource roles via PowerShell, these are structured as JSON files for RBAC (and holy crap, I can actually read these with my limited exposure to JavaScript over the last few weeks!) 
- In these role definitions, `NotActions` are subtractions from what has been granted by the properties of `Actions`. In these role properties, `{*}` short-hands for "all", then subtracting from that in the `NotActions` (or `NotDataActions`) property. 
- `Actions` vs `DataActions` are Azure resource management (think admin) control vs data operations (think capital-D Data. eg: read a DB, write a storage blob to a container, etc).
- Actually it's more than just capital-D Data operations, it includes compute too, for example: `Microsoft.Compute/virtualMachines/login/action` as a `DataActions` property allows the role to to log in to a VM as a regular user.
- Creating custom roles requires Azure AD Premium P1 or P2.

## [Azure Identity Management and Governance (via CloudSkills.io AZ-104 Exam Prep Course)](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course)

![From CloudSkills.io AZ-104 course](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/004/rbac.JPG) 

### [Manage Role-Based Access Control, RBAC](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course/categories/2692676/posts/8980102

- I inverted my approach from last time, which was to watch a course video followed by a closely adjacent MS Learn path module. Doing the module first and then watching the video seems more effective. 
- RBAC scope: Management Group > Subscription > Resource Group > Resource.  The closer to the resource, the stricter (more specific?) the permission options.
- RBAC determines if a user has access to a resource via the token assigned to a user for Azure Resource Manager. 
- Whether the action is done in the Portal GUI or in PowerShell/CLI, this involves a REST API call to ARM with that token attached. Order of operations is: ARM retrieves role assignments and deny assignments, checks against assignments, if not denied then checks against deny assignments. 

