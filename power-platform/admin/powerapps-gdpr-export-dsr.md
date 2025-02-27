---
title: Responding to Data Subject Rights (DSR) requests to export PowerApps customer data | Microsoft Docs
description: Walkthrough of how to respond to Data Subject Rights (DSR) requests to export PowerApps customer data.
author: jimholtz
ms.reviewer: paulliew
manager: kvivek
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---

# Responding to Data Subject Rights (DSR) requests to export PowerApps customer data
The “right of data portability” allows a data subject to request a copy of his or her personal data in an electronic format (that is, a structured, commonly used, machine readable and interoperable format) that may be transmitted to another data controller:

* Website access: [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc), [PowerApps Admin center](https://admin.powerapps.com/), and [Office 365 Service Trust Portal](https://servicetrust.microsoft.com/)

* PowerShell access: PowerApps [App creator cmdlets](https://go.microsoft.com/fwlink/?linkid=871448), [Admin cmdlets](https://go.microsoft.com/fwlink/?linkid=871804) and [On-premises gateway cmdlets](https://go.microsoft.com/fwlink/?linkid=872238)

Below is a summary of the types of personal data that PowerApps can store for a specific user and which experiences you can use to find and export it.

Resources containing personal data | Website access |	PowerShell access
--- | --- | --
Environment	| PowerApps Admin center | 	PowerApps cmdlets
Environment permissions**	| PowerApps Admin center 	| PowerApps cmdlets
Canvas App	| PowerApps Admin center <br> PowerApps Portal | 	PowerApps cmdlets
Canvas App permissions	| PowerApps Admin center <br> PowerApps Portal	| PowerApps cmdlets
Gateway | PowerApps Portal***	| On-premises gateway cmdlets
Gateway permissions	| PowerApps Portal***	|
Custom connector | |	App creator: Available <br> Admin: Available
Custom connector permissions | | 	App creator: Available <br> Admin: Available
Connection | | App creator: Available <br> Admin: Available
Connection permissions	| | App creator: Available <br> Admin: Available
PowerApps user settings, user-app settings, and notifications | | App creator: Available <br> Admin: Available

> ** With the introduction of Common Data Service, if a database is created within the environment, environment permissions and model-driven app permissions are stored as records within the Common Data Service database environment. For guidance on how to respond to DSR requests for users that use Common Data Service, see [Responding to Data Subject Rights (DSR) requests for Common Data Service customer data](common-data-service-gdpr-dsr-guide.md).

> *** An administrator can access these resources from the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc) only if the owner of the resource has explicitly granted him or her access. If the administrator has not been granted access, he or she will need to leverage the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804).

## Prerequisites

### For users
Any user with a valid PowerApps license can perform the user operations outlined in this document using the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc) or [App creator cmdlets](https://go.microsoft.com/fwlink/?linkid=871448).

### For admins
To perform the administration operations outlined in this document using the PowerApps Admin center, Power Automate Admin Center, or [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804), you'll need the following:

* A paid PowerApps plan or a PowerApps trial. You can sign-up for a 30-day trial at [http://web.powerapps.com/trial](http://web.powerapps.com/trial). Trial licenses can be renewed if they've expired.

* [Office 365 Global Administrator](https://support.office.com/article/assign-admin-roles-in-office-365-for-business-eac4d046-1afd-4f1a-85fc-8219c79e1504) or [Azure Active Directory Global Administrator](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) permissions if you need to search through another user’s resources. (Note that Environment Admins only have access to those environments and environment resources for which they have permissions.)

## Step 1: Export personal data contained within environments created by the user

### PowerApps Admin center
Administrators can export all environments created by a specific user from the [PowerApps Admin center](https://admin.powerapps.com/) by following these steps:

1. From the [PowerApps Admin center](https://admin.powerapps.com/), select each environment in your organization.

    ![Admin Center Landing Page](./media/powerapps-gdpr-export-dsr/admin-center-landing.png)

2. If the environment was created by the user from the DSR request, go to the **Details** page, copy the details, and then paste them into a document editor, such as Microsoft Word.

    ![Environment details](./media/powerapps-gdpr-export-dsr/environment-details.png)

### PowerShell cmdlets for app creators
Users can export the environments they have access to in PowerApps by using the **Get-PowerAppsEnvironment** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount
Get-PowerAppsEnvironment | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export all of the environments that have been created by a user by using the **Get-AdminEnvironment** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "7557f390-5f70-4c93-8bc4-8c2faabd2ca0"
Get-AdminEnvironment -CreatedBy $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~
 
## Step 2: Export the user’s environment permissions
Users can be assigned permissions (such as Environment Admin, Environment Maker, etc.) in an environment, which are stored in PowerApps as a *role assignment*. With the introduction of Common Data Service, if a database is created within the environment, the role assignments are stored as records within the Common Data Service database environment. For more information, see [Administer environments within PowerApps](environments-administration.md).

### For environments without a Common Data Service database

#### PowerApps Admin center
Administrators can export a user’s environment permissions from the [PowerApps Admin center](https://admin.powerapps.com/) by following these steps:

1. From the [PowerApps Admin center](https://admin.powerapps.com/), select each environment in your organization. You must be an [Office 365 Global Administrator](https://support.office.com/article/assign-admin-roles-in-office-365-for-business-eac4d046-1afd-4f1a-85fc-8219c79e1504) or [Azure Active Directory Global Administrator](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) to be able to review all environments created within your organization.

    ![Admin Center Landing Page](./media/powerapps-gdpr-export-dsr/admin-center-landing.png)

2. Select **Security**.

    If your environment does not have a Common Data Service database, you'll see a section for **Environment Roles.**

3. Select both **Environment Admin** and **Environment Maker** separately, and then using the search bar, search for the user’s name.

    ![Environment roles page](./media/powerapps-gdpr-export-dsr/admin-environment-role-share-page.png)

4. If the user has access to either role, go to the **Users** page, copy the details, and then paste them into a document editor, such as Microsoft Word.

#### PowerShell cmdlets for admins
Administrators can export all environment role assignments for a user across all environments without a Common Data Service database by using the **Get-AdminEnvironmentRoleAssignment** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminEnvironmentRoleAssignment -UserId $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

> [!IMPORTANT]
>  This function only works for environments that do not have a Common Data Service database environment.
>
>

### For environments with a Common Data Service database
With the introduction of the Common Data Service, if a database is created within the environment, role assignments are stored as records within the Common Data Service database environment. For information on how to remove personal data from a Common Data Service database environment, see [Common Data Service User personal data removal](https://go.microsoft.com/fwlink/?linkid=871886).
 
## Step 3: Export personal data contained within canvas apps created by the user

### PowerApps portal
A user can export an app from the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc). For step-by-step instructions on how to export an app, see [Exporting an app](environment-and-tenant-migration.md#exporting-an-app).

### PowerApps Admin center
An administrator can export apps created by a user starting from the [PowerApps Admin center](https://admin.powerapps.com/) by following these steps:

1. From the [PowerApps Admin center](https://admin.powerapps.com/), select each environment in your organization. You must be an [Office 365 Global Administrator](https://support.office.com/article/assign-admin-roles-in-office-365-for-business-eac4d046-1afd-4f1a-85fc-8219c79e1504) or [Azure Active Directory Global Administrator](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) to be able to review all environments created within your organization.

    ![Admin Center Landing Page](./media/powerapps-gdpr-export-dsr/admin-center-landing.png)

2. Select **Resources**, and then select **Apps**.

3. Using the search bar, search for the user’s name, which brings up any apps that user created within this environment:

    ![Search apps](./media/powerapps-gdpr-export-dsr/search-apps.png)

4. Select **Share** for each of the apps created by that user and give yourself **Can edit** access to the app:

    ![Select app share](./media/powerapps-gdpr-export-dsr/select-share.png)

    ![Give a user access](./media/powerapps-gdpr-export-dsr/grant-access.png)

5. Once you have access to each of the user’s apps you can export an app from the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc). For step-by-step instructions on how to export an app, see [Exporting an app](environment-and-tenant-migration.md#exporting-an-app).

### PowerShell cmdlets for admins
Administrators can export apps created by a user by using the **Get-AdminApp** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminApp -Owner $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

## Step 4: Export the user’s permissions to canvas apps
Whenever an app is shared with a user, PowerApps stores a record called a *role assignment* that describes the user’s permissions (CanEdit or CanUser) to the application. For more information, see [Share an app](/powerapps/maker/canvas-apps/share-app.md#share-an-app).

### PowerShell cmdlets for app creators
Users can export the app role assignments for all apps that they have access to by using the **Get-RoleAssignment** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount
Get-AppRoleAssignment | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

### PowerApps Admin center
Administrators can export app roles assignments for a user from the [PowerApps Admin center](https://admin.powerapps.com/) by following these steps:

1. From the [PowerApps Admin center](https://admin.powerapps.com/), select each environment in your organization. You must be an [Office 365 Global Administrator](https://support.office.com/article/assign-admin-roles-in-office-365-for-business-eac4d046-1afd-4f1a-85fc-8219c79e1504) or [Azure Active Directory Global Administrator](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) to be able to review all environments created within your organization.

    ![Admin Center Landing Page](./media/powerapps-gdpr-export-dsr/admin-center-landing.png)

2. For each environment, select **Resources**, and then select **Apps**.

3. Select **Share** for each of the apps in the environment.

    ![Select app share](./media/powerapps-gdpr-export-dsr/select-admin-share-nofilter.png)

4. If the user has access to the app, go to the app’s **Share** page, copy the details, and then paste them into a document editor, such as Microsoft Word.

    ![Admin app share page](./media/powerapps-gdpr-export-dsr/admin-share-page.png)

### PowerShell cmdlets for admins
Administrators can export all app role assignments for a user across all apps in their tenant by using the **Get-AdminAppRoleAssignment** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminAppRoleAssignment -UserId $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

## Step 5: Export personal data contained within connections created by the user
Connections are used in conjunction with connectors when establishing connectivity with other APIs and SaaS systems. Connections include references to the user who created them and, as a result, can be deleted to remove any references to the user.

### PowerShell cmdlets for app creators
Users can export all of the connections they have access to by using the **Get-Connection** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount
Get-Connection | ConvertTo-Json | out-file -FilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export all connections created by the user using the  **Get-AdminConnection** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminConnection -CreatedBy $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~
 
## Step 6: Export the user’s permissions to shared connections

### PowerShell cmdlets for app creators
Users can export the connection role assignments for all connections that they have access to by using the **Get-ConnectionRoleAssignment** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount
Get-ConnectionRoleAssignment | ConvertTo-Json | Out-file -FilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export all connection role assignments for a user using the  **Get-AdminConnectionRoleAssignment** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminConnectionRoleAssignment -PrincipalObjectId $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

## Step 7: Export personal data contained within custom connectors created by the user
Custom Connectors supplement the existing out-of-box connectors and allow for connectivity to other APIs, SaaS, and custom-developed systems.

### PowerApps App creator PowerShell cmdlets
Users can export all custom connectors they've created by using the **Get-Connector** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount  
Get-Connector -FilterNonCustomConnectors | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export all custom connectors created by a user using the  **Get-AdminConnector** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminConnector -CreatedBy $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

## Step 8: Export the user’s permissions to custom connectors

### PowerShell cmdlets for app creators
Users can export all connector role assignments for the custom connectors to which they have access by using the **Get-ConnectorRoleAssignment** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount  
Get-ConnectorRoleAssignment | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export all custom connector role assignments for a user using the  **Get-AdminConnectorRoleAssignment** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminConnectorRoleAssignment -PrincipalObjectId $userId | ConvertTo-Json | Out-File -FilePath "UserDetails.json"
~~~~
 
## Step 9: Export PowerApps Notifications, User Settings, and User-App Settings
PowerApps sends several types of notifications to users, including when an app is shared with them and when a Common Data Service export operation has completed. A user’s notification history is visible to them within the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc).

PowerApps also stores several different user preferences and settings that are used to deliver the PowerApps runtime and portal experiences, including when a user last opened an application, pinned an app, etc.

### PowerShell cmdlets for app creators
Users can export their own PowerApps notifications, user settings, and user-app settings using the **Get-AdminPowerAppsUserDetails** function in the [PowerApps App creator PowerShell cmdlets](https://go.microsoft.com/fwlink/?linkid=871448):

~~~~
Add-PowerAppsAccount  
Get-AdminPowerAppsUserDetails -WriteToFile -OutputFilePath "UserDetails.json"
~~~~

### PowerShell cmdlets for admins
Administrators can export the PowerApps notifications, user settings, and user-app settings for a user using the  **Get-AdminPowerAppsUserDetails** function in the [PowerApps Admin PowerShell cdmlets](https://go.microsoft.com/fwlink/?linkid=871804):

~~~~
Add-PowerAppsAccount
$userId = "0ecb1fcc-6782-4e46-a4c4-738c1d3accea"
Get-AdminPowerAppsUserDetails -WriteToFile -OutputFilePath "UserDetails.json" -UserPrincipalName name@microsoft.com
~~~~

## Step 10: Export personal data contained for a user-stored gateway or in the user’s gateway permissions

### PowerApps Portal
Users can export the personal data stored within the gateway service from the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc) by following these steps:

1. From the [PowerApps portal](https://web.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc), within the default environment for your tenant, select **Gateways**, and then select **Details** for each gateway to which you have access.

    ![Gateway landing page](./media/powerapps-gdpr-export-dsr/gateway-select-details.png)

2. On the **Details** page, if the gateway details contain any personal data, copy the details, and then paste them into a document editor, such as Microsoft Word.

    ![Gateway details](./media/powerapps-gdpr-export-dsr/gateway-details-drillin.png)

3. Select **Share**, copy the contents of the page, and then paste it into a document editor, such as Microsoft Word.

    ![Gateway details](./media/powerapps-gdpr-export-dsr/gateway-details-share.png)

### Gateway PowerShell cmdlets
There are also PowerShell cmdlets that allow you to retrieve, manage, and delete your personal gateways. For more information, see [On-premises gateway cmdlets](https://go.microsoft.com/fwlink/?linkid=872238).

### Administrators
Please refer to the **Tenant Administration** section in the [Understand on-premises data gateways for Microsoft PowerApps](https://docs.microsoft.com/powerapps/maker/canvas-apps/gateway-reference#tenant-level-administration) article for guidance around managing gateways for your organization.

## Step 11: Export the user’s personal data in Power Automate
PowerApps licenses always include Power Automate capabilities. In addition to being included in PowerApps licenses, Power Automate is also available as a standalone service. For guidance on how to respond to DSR requests for users that use the Power Automate service, see [Responding to GDPR Data Subject Requests for Power Automate](https://go.microsoft.com/fwlink/?linkid=872250).

> [!IMPORTANT]
>  We recommend that administrators complete this step for PowerApps users.


## Step 12: Export the user’s personal data in Common Data Service environments
Anyone with a PowerApps license, provided there is 1GB available database capacity, can create Common Data Service environments and create and build apps on Common Data Service; this includes the PowerApps Community Plan, which is a free license that allows users to try out Common Data Service in an individual environment. To see which Common Data Service capabilities are included in each PowerApps license, see the [PowerApps Pricing page](https://powerapps.microsoft.com/pricing).

For guidance on how to respond to DSR requests for users that use Common Data Service, see [Responding to Data Subject Rights (DSR) requests for Common Data Service customer data](common-data-service-gdpr-dsr-guide.md).

> [!IMPORTANT]
>  We recommend that administrators complete this step for PowerApps users.
