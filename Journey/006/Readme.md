![Sure lets try to learn the syntax of two command line interfaces at once...](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/006/azccap.JPG)

# Day Six (25OCT20)

## Setting Up PowerShell for AZ 
### [Via Section 3 of Scott Duffy's AZ-104 Course](https://www.udemy.com/course/70533-azure/)

I've made it no secret that I prefer CLI. The syntax just makes more sense to me. But I'm aware that AZ-104 might contain more PowerShell focused exam questions than CLI. In order to force more PowerShell practice, I'm taking my PS exposure beyond the Azure CloudShell (in browser via the Azure Portal) and onto my desktop, with the intent of then trying hands on to replicate various AZ Portal based lessons in PS.
- Step 1: Is my Window's PS up to date? First thing I had to do was even learn the PS command to figure out my version (`PSversiontable`). Looks like I'm still running 5.1.
- Step 2: Download the latest PS .msi (which at the time of this post was 7.1)
- Step 3: Running 7.1 in admin mode, install the Azure module via `Install-Module -Name Az -AllowClobber` 
- Step 4: Connect Azure PS to my Azure account, using `Connect-AzAccount`. This prints a URL and a one-time use code. Going to that URL in browser and inputting the code allows me to connect Azure PS to my Azure account. Good to go! Now I have no excuses for not using or practicing PS more actively in my Azure studies.

## Self Service Password Reset (SSPR) in Azure AD
### [Via MS Learn Module 'Allow users to reset their password with Azure Active Directory self-service password reset'](https://docs.microsoft.com/en-us/learn/modules/allow-users-reset-their-password/)

- Azure supports six methods of Authentication for password reset requrests:
1. Mobile App Notification (pre-registered to MFA)
2. Mobile App Code (pre-registered to MFA)
3. Email (pre-registered external account for recovery)
4. Mobile Phone (receive SMS) (not supported in free/trial Azure AD)
5. Office Phone (not supported in free/trial Azure AD)
6. Security Questions
-You can specify the minimum number of authentication methods to be one or two (ex: you could require both security questions and mobile app code).
-For admin accounts, two (strong) methods of authentication are required, and the Security Question option is not allowed to be one of them.
- Password Reset Notifications: Admins have two options, 1. set to notify the user when their password is reset, or 2. set to notify all admins when a user password is reset. 
- SSPR is only available for Azure AD Premium P1 and Premium P2, not Azure AD free. 
- For Hybrid cloud customers (with one instance of AD on prem, and one Azure AD in the cloud), password resets done in the cloud must be written-back to the AD on prem. -writeback- is a supported feature in Azure AD P1 and P2.
- Internal Self-Note: Remember to select the correct change directory option to use AD in the Sandbox tutorials... The ones all the way at the bottom. 

## Study Resource - [TheDevOps Azure Powershell/CLI Cheat Sheet](http://thedevopspage.com/azurecli-powershell-cheatsheet)

Hugely helpful. They have a great study guide for AZ-104 (and 900 and 500) here as well, as well some flashcards.
