![Someone stole my toys from the AZ sandbox](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/003/azAD%20sandbox%20error.JPG)

# Day 3 - Back on the 104 Horse, Slowly.

## [Azure Identity Management and Governance (via CloudSkills.io AZ-104 Exam Prep Course)](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course)

*aka: Why I used to have to log in to my Microsoft 365 work account with totally different credentials than my actual work email back at "Corporation X" (because my Azure AD UPN was based on the overall corporate domain, not our division's unique email domain. Spoiler alert: having my AD UPN not match my work email was a really bad practice! Way to go "Corporation X" IT department).*

### [Part 1: Manage Azure AD Objects](https://portal.cloudskills.io/products/azure-administrator-az-104-exam-prep-course/categories/2692676/posts/8980096)

After learning that I in fact did not have access to Pluralsight's AZ-104 path, I threw my $7 at CloudSkill's AZ-104 course so I could listen to the dulcet sounds of Tim Warner's voice while learning about Azure Active Directory, managing identities and governance. An hour well spent. It was a good refresher on tenants vs subscriptions vs identities. I learned a few new things about Azure, but most importantly about myself:

1. I need to learn PowerShell basics. I have a passing familiarity with Linux and bash, so I feel more comfortable with CLI. But this lesson specifically called out the need to know PowerShell for the AZ-104 exam, so I'll need to do some familiarization so I can read PowerShell script examples for possible exam questions.
2. I need more hands-on time in Azure in general, and coming up with some personal projects to build as a part of #100DaysofCloud is how I will address this.
3. I need to work on improving my study habits, close my other tabs, turn off my phone and focus. I can't tell you much time I spent on reddit, browsing jobs I know I'm not qualified for (yet) on LinkedIn, or researching SANS Institute courses and degree options.

## [Manage identities and governance in Azure (via Microsoft Learn)](https://docs.microsoft.com/en-us/learn/paths/az-104-manage-identities-governance/?source=learn)
 
*aka: the slightly more active, hands on approach the to video sesh above.*

### [Module 1: Create Azure users and groups in Azure Active Directory](https://docs.microsoft.com/en-us/learn/modules/create-users-and-groups-in-azure-active-directory/)

1. I asked for more hands-on with PowerShell, and I got it. And rejected it. After immediately being turned off by the big blue screen and confused on my first failed attempt to create a new user in PowerShell, I tried it in bash. Still took a few tries, but with each attempt I corrected mistakes and eventually figured it out. Another point for Linux in my book. 
2. Next, we're back working for Conteso Marketing Company and setting up their AD Premium P2 trial, this time through the Azure Portal. After adding the timeless "Chris Green" to our AD, I'd like to opt to delete him via PowerShell instead of the Portal as instructed, but don't have access during this exercise because I'm using the MS Learn sandbox which is not a "valid subscription". Alas, I delete and recover the user the easy way in the Portal. 
3. Moving on through the rest of Module 1, AZ AD groups, managing external and guest accounts, and Azure AD B2B are covered. I finish the module, but am unable to do the two exercises there because the Sandbox is telling me I don't have access to AZ AD.... Better luck next time I suppose.
