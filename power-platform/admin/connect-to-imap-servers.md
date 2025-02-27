---
title: "Connect to IMAP or SMTP servers  | MicrosoftDocs"
description: Connect to IMAP or SMTP servers 
author: jimholtz
manager: kvivek
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---
# Connect to IMAP or SMTP servers

[!INCLUDE [cc-settings-moving](../includes/cc-settings-moving.md)] 


Follow these steps to connect model-driven apps in Dynamics 365, such as Dynamics 365 Sales and Customer Service, with IMAP email servers such as used for Gmail and Yahoo! Mail.  

> [!NOTE]
> - Only emails in the Inbox folder are synchronized.
> - Existing POP3 email profiles will not be automatically converted to IMAP. There is no support for migrating from POP3 to IMAP.
> - For IMAP/SMTP systems supported by Microsoft, check out the following topic: [Supported email service configurations for server-side synchronization](supported-email-service-configurations-server-side-synchronization.md).  

<a name="BKMK_CreateProfile"></a>   

## Create an email server profile  

1. Go to **Settings** > **Email Configuration** > **Email Server Profiles**.  

2. Choose **New** > **IMAP/SMTP Server**.  

3. **For an Exchange email server profile, specify the following details:**  


   |  Fields  |     Description      |
   |-----------|----------|   
   |   **General**      |     |   
   | Name     |  Specify a meaningful name for the profile.   |   
   |   Description   |  Type a short description about the objective of the email server profile.       |
   | Incoming Server Location and Outgoing Server Location  |  Enter the **Incoming Server Location** and **Outgoing Server Location**<br /><br /> For example, Incoming: outlook.office365.com and Outgoing: smtp.office365.com  |
   |  **Credentials**  |    |
   |  Authenticate Using  | Select a method to authenticate while connecting to the specified email server.<br /><br /> <ul><li> **Credentials Specified by a User or Queue**. If you select this option, the credentials specified in the mailbox record of a user or queue are used for sending or receiving email for the respective user or queue. **Note:**      To ensure the credentials are secured, SQL encryption is used to encrypt the credentials stored in the mailbox.</li><br /><li> **Credentials Specified in Email Server Profile**. If you select this option, the credentials specified in the email server profile are used for sending or receiving email for the mailboxes of all users and queues associated with this profile. The credentials must have impersonation or delegation permissions on the mailboxes associated with profile. This option requires some configuration on the email server, for example, configuring impersonation rights on [!INCLUDE[pn_Exchange](../includes/pn-exchange.md)] for the mailboxes associated with the profile. **Note:**      To ensure the credentials are secured, SQL encryption is used to encrypt the credentials stored in the email server profile if you’re processing email by using server-side synchronization.</li><br /><li> **Windows Integrated Authentication**. This option applies only to [!INCLUDE[pn_Exchange](../includes/pn-exchange.md)] and SMTP email server types. If you select this option, the credentials with which the Asynchronous Service has been configured will be used.</li><br /><li> **Without Credentials (Anonymous)**. Not a valid setting. </li></ul>|
   | User Name   | Type the user name used to connect to the email server for sending or receiving email for the mailboxes of all users and queues associated with this profile. This field is enabled and valid only if **Authenticate Using** is set to **Credentials Specified in Email Server Profile**. The user name that you specify must have permission to send and receive email from the mailboxes of users and queues associated with this profile. **Note:**  If you’re using HTTP for model-driven apps in Dynamics 365, the **User Name** and **Password** fields will be disabled. To enable the option, change the value of the deployment property AllowCredentialsEntryViaNonSecureChannels to 1.  |
   |  Password   |  Specify the password of the user that will be used together with the user name to connect to the email server for sending or receiving email for the mailboxes of users and queues associated with this profile. The password is stored securely. **Note:**  If you’re using HTTP for model-driven apps in Dynamics 365, the **User Name** and **Password** fields will be disabled. To enable the option, change the value of the deployment property AllowCredentialsEntryViaNonSecureChannels to 1.  | Use same settings for Outgoing |  If you want to use the same credential settings for the incoming and outgoing connections, choose **Yes**.  |
   | **Advanced**    |  |
   |  Incoming Port  | This field shows the port on the email server for accessing the incoming email. This field is automatically populated when you save the record.   |
   |  Outgoing Port  | This field shows the port on the email server for accessing the outgoing email. This field is automatically populated when you save the record.   |
   | Use SSL for Incoming Connection    |  Choose **Yes** if the email channel is on a secure channel and [!INCLUDE[pn_ssl_short](../includes/pn-ssl-short.md)] must be used for receiving email.   |
   | Use SSL for Outgoing Connection |  Choose **Yes** if the email channel is on a secure channel and [!INCLUDE[pn_ssl_short](../includes/pn-ssl-short.md)] must be used for sending email.  |
   | Incoming Authentication Protocol and Outgoing Authentication Protocol |  Select a protocol that will be used for authentication for incoming and outgoing email.  |
   |   **Additional Settings**  |  |
   |  Process Email From |  Select a date and time. Email received after the date and time will be processed by server-side synchronization for all mailboxes associated with this profile. If you set a value less than the current date, the change will be applied to all newly associated mailboxes and their earlier processed emails will be pulled.  |
   | Minimum Polling Intervals in Minutes  |  Type the minimum polling interval, in minutes, for mailboxes that are associated with this email server profile. The polling interval determines how often server-side synchronization polls your mailboxes for new email messages.  |
   |  Maximum Concurrent Connections   | Type the maximum number of simultaneous connections that can be made to the corresponding email server per mailbox. Increase the value to allow more parallel calls to [!INCLUDE[pn_Exchange](../includes/pn-exchange.md)] to improve performance or reduce the value if there are errors on [!INCLUDE[pn_Exchange](../includes/pn-exchange.md)] due to large number of calls from model-driven apps in Dynamics 365. The default value of this field is 10. The maximum number is considered per mailbox or per email server profile depending on whether the credentials are specified in a mailbox or email server profile.   |


4. Choose **Save**.  

<a name="BKMK_ConfigureDefault"></a>   

## Configure default email processing and synchronization  
Set server-side synchronization to be the default configuration method. 
 
1. Go to **Settings** > **Email Configuration** > **Email Configuration Settings**.  

2. Set the processing and synchronization fields as follows:  

   - **Server Profile**: The profile you created in the above section.  

   - **Incoming Email**: Server-Side Synchronization or Email Router  

   - **Outgoing Email**: Server-Side Synchronization or Email Router  

   - **Appointments, Contacts, and Tasks**: Server-Side Synchronization or Email Router  

       > [!NOTE]
       >  Server-Side Synchronization or Email Router for Appointments, Contacts, and Tasks is not supported for the IMAP profile.  

     If you leave the **Email processing form unapproved user and queues** at the default values (checked), you will need to approve emails and queues for user mailboxes as directed below in **Approve Email**.  

     ![System Settings for server-side synchronization](../admin/media/imap-profile.png "System Settings for server-side synchronization")  

3.  Click **OK**.  

<a name="BKMK_ConfigureMailboxes"></a>  
 
## Configure mailboxes  
 To set mailboxes to use the default profile, you must first set the Server Profile and the delivery method for email, appointments, contacts, and tasks.  

 In addition to administrator permissions, you must have Read and Write privileges on the Mailbox entity to set the delivery method for the mailbox.  

 Click **one** of the following methods:  

### Set mailboxes to the default profile  

1. Go to **Settings** > **Email Configuration** > **Mailboxes**.  

2. Choose **Active Mailboxes**.  

3. Select all the mailboxes that you want to associate with the IMAP profile you created, click **Apply Default Email Settings**, verify the settings, and then click **OK**.  

   ![Apply default email settings](../admin/media/apply-default-email-settings.png "Apply default email settings")  

    By default, the mailbox configuration is tested and the mailboxes are enabled when you click **OK**.  

### Edit mailboxes to set the profile and delivery methods  

1.  Go to **Settings** > **Email Configuration** > **Mailboxes**.  

2.  Click **Active Mailboxes**.  

3.  Select the mailboxes that you want to configure, and then click **Edit**.  

4.  In the **Change Multiple Records** form, under **Synchronization Method**, set **Server Profile** to the IMAP profile you created earlier.  

5.  Set **Incoming** and **Outgoing** **Email** to **Server-Side Synchronization or Email Router**.  

6.  Set **Appointments, Contacts, and Tasks** to **None**.  


7.  Click **Change**.  

<a name="BKMK_ApproveEmail"></a> 
  
## Approve email  
 You need to approve each user mailbox or queue before that mailbox can process email.  

1.  Go to **Settings** > **Email Configuration** > **Mailboxes**.  

2.  Click **Active Mailboxes**.  

3.  Select the mailboxes that you want to approve, and then click **More Commands** (**…**) > **Approve Email**.  

4.  Click **OK**.  

<a name="BKMK_TestConfiguration"></a>   

## Test configuration of mailboxes  

1. Go to **Settings** > **Email Configuration** > **Mailboxes**.  

2. Click **Active Mailboxes**.  

3. Select the mailboxes you want to test, and then click **Test & Enable Mailboxes**.  

    This tests the incoming and outgoing email configuration of the selected mailboxes and enables them for email processing. If an error occurs in a mailbox, an alert is shown on the Alerts wall of the mailbox and the profile owner. Depending on the nature of the error, model-driven apps in Dynamics 365 try to process the email again after some time or disables the mailbox for email processing.  

    The result of the email configuration test is displayed in the **Incoming Email Status**, **Outgoing Email Status**, and **Appointments, Contacts, and Tasks Status** fields of a mailbox record. An alert is also generated when the configuration is successfully completed for a mailbox. This alert is shown to the mailbox owner.  

    You can find information on recurring issues and other troubleshooting information in [Blog: Test and Enable Mailboxes in Microsoft Dynamics CRM 2015](https://blogs.msdn.com/b/crm/archive/2015/08/31/test-and-enable-mailboxes-in-microsoft-dynamics-crm-2015.aspx) and [Troubleshooting and monitoring server-side synchronization](troubleshooting-monitoring-server-side-synchronization.md).  

> [!TIP]
>  If you’re unable to synchronize contacts, appointments, and tasks for a mailbox, you may want to select the **Sync items with Exchange from this org only, even if Exchange was set to sync with a different org** check box. [Read more about this check box](when-would-want-use-check-box.md).  

<a name="BKMK_TestEmailConfig"></a>   

## Test email configuration for all mailboxes associated with an email server profile  

1. Go to **Settings** > **Email Configuration** > **Email Server Profiles**.  

2. Select the profile you created, and then click **Test & Enable Mailboxes**.  

    When you test the email configuration, an asynchronous job runs in the background. It may take a few minutes for the test to be completed. Model-driven apps in Dynamics 365 test the email configuration of all the mailboxes associated with the IMAP profile. For the mailboxes configured with server-side synchronization for synchronizing appointments, tasks, and contacts, it also checks to make sure they’re configured properly.  

> [!TIP]
>  If you’re unable to synchronize contacts, appointments, and tasks for a mailbox, you may want to select the **Sync items with Exchange from this org only, even if Exchange was set to sync with a different org** check box. [Read more about this check box](when-would-want-use-check-box.md).  

<a name="BKMK_NetworkPorts"></a>   

## Network ports for PowerApps US Government  
 The following ports are open for outbound connections between PowerApps US Government and internet services.  

- 80 HTTP  
- 443 HTTPS 
- 465 Secure SMTP  
- 587 Secure SMTP  
- 993 Secure IMAP  

Customizations or email configurations in PowerApps US Government can only use these ports.  

### See also  
 [Troubleshooting and monitoring server-side synchronization](troubleshooting-monitoring-server-side-synchronization.md) <br />
 [Test mail flow with the Remote Connectivity Analyzer](https://technet.microsoft.com/library/dn305950\(v=exchg.150\).aspx)   
 [Set up server-side synchronization](set-up-server-side-synchronization-of-email-appointments-contacts-and-tasks.md)   

