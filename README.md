# Ignite 2023 session DEM191

**Top tips for self-service management using Microsoft Entra**

Microsoft Entra offers self-service solutions while staying secure and compliant. Here´s the corresponding demo material for **Ignite session DEM191** how you can implement governance and create a self-service approach and a guest user lifecycle management.

## Invite User

This scenario includes a Power App that manages data from a SharePoint Online list. It includes a form to invite a new guest user, and a status screen that shows all requests with their status. The Power App is connected to an Azure Logic App (flow) that creates the guest user and sends an email to the sponsor. The flow also updates the status in the SharePoint Online list when the guest user is created. The Power App can be imported in any environment, and be integrated in Microsoft Teams. It provides a self-service tool for users to collaborate with external users.

| Name | Type | Description |
|------|------|-------------|
| InviteGuest.zip | Power App | The user interface that can be used to invite a guest and to see the status of the recent requests. See here how to [Export and import canvas app packages](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/export-import-app). |
| 01-InviteGuest.json | Azure Logic App | The flow that uses Microsoft Graph to invite a new user, and to update the status. |

## Guest Lifecycle Management

This solution provides an automatic process for doing a cleanup of inactive guest users in the M365 tenant. The process is using multiple Azure Logic Apps to identify inactive guest users, to notify them, and to finally delete them if they do not sign-in within a given timeframe.

The flows runs through all guests in the M365 tenant and send an email with a notification to each inactive guest users. The LastLoginDate is used as an indicator as to whether a user has not logged in for a while, e.g. for more than 180 days. After an additional 30 days of inactivity, the guest user will be notified that their account has been deleted. These values can be configured in the parameters. Administrators also receive a list of notified and deleted guest users.

| Name | Type | Description |
|------|------|-------------|
| 02-GetInactiveGuests.json | Azure Logic App | The flow reads all users with Graph, calculates the LastLoginDays and saves them into the storage table, |
| 03-SendGuestsEmails.json | Azure Logic App | The flow reads all inactive users from the storage table, and sends out an email to each inactive guest and a summary to the admins. |
| 04-DeleteInactiveGuests.json | Azure Logic App | The flow reads all inactive users that should be deleted today from the storage table, deletes them, and sends out an email to each guest about the account deletion and a summary to the admins. |
| guestscleanup | Azure Table Storage | This is a helper table for storing the users, and if and when they shall be deleted. This table can be created manually, or any other table storage can be used, like a SharePoint list, or similar. |

Currently, the flows are available as it. A version for the complete solution with ARM Deployment will be available soon.
I hope the session was helpful in providing some ideas and automating your Guest Lifecycle processes. If you have any questions, please feel free to contact me.
