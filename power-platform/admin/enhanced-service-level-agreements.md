---
title: "Enhanced service level agreements  | MicrosoftDocs"
description: Enhanced service level agreements
author: jimholtz
manager: kvivek
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 09/30/2017
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---
# Enhanced service level agreements

[!INCLUDE [cc-settings-moving](../includes/cc-settings-moving.md)] 

Service level agreements (SLAs) are a formalized method to help organizations meet service levels when they provide customer service and support. For example, an organization can have an SLA to complete the first customer response within 48 business hours after a case is created. Another example is to escalate an unresolved case after a specified duration, such as five business days. SLAs are used to define these different aspects of service.  
  
Model-driven apps in Dynamics 365, such as Dynamics 365 Sales and Customer Service, include two kinds of SLAs, standard and enhanced. Enhanced SLAs include the following features not available in standard SLAs:  
  
-   Case-on-hold support  
  
-   Auto-pause and resume of time calculation  
  
-   Support for success actions  
  
-   Creation of dashboards or reports based on the SLA KPI environment entity  
  
## Case-on-hold support  
 One feature of SLA tracking is the ability to control the case-on-hold status. For example, this functionality lets you pause a case for a time when the case is on hold waiting for a response from the customer. Once the response is received, the case is resumed.  
  
 System administrators turn on SLAs and select case hold functionality in **Settings** > **Service Management** > **Service Configuration Settings**. Afterwards, CSR Managers can create SLAs using the enhanced SLA type that allows pause and resume functionality. SLAs are created in **Settings** > **Service Management**.  
  
 [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Define service level agreements (SLAs)](/dynamics365/customer-engagement/customer-service/define-service-level-agreements.md)  
  
## Considerations when you choose a SLA type  
 Because there are two types of SLAs that have different functionality, consider the following features before you choose an SLA type. We recommend that you use only one type of SLA for an organization.  
  
-   After you select an SLA type, either standard or enhanced, you cannot change the SLA type for any record associated with the SLA.  
  
-   Because standard and enhanced SLAs exist as separate entities with separate forms, views, and fields, the following behaviors exist.  
  
    -   Case views cannot be sorted by enhanced SLA fields. To display enhanced SLA fields in Case views, you can modify any of the Case views to display the fields from the enhanced SLA (which has the entity name SLA KPI environment).  Although you can sort on the fields that are part of the Case entity, because the enhanced SLA fields are on a related entity, you cannot sort on columns that are associated with the enhanced SLA fields.  
  
    -   Queue Item views do not display enhanced SLA fields. Although, Queue Item views display the standard fields SLA (First Response By and Resolve By), because the enhanced SLA (SLA KPI environment entity) is not directly related to the Queue Item entity, the columns associated with enhanced SLAs cannot be displayed.  
  
> [!TIP]
>  To monitor enhanced SLA details, consider creating custom dashboards based on the SLA KPI environment entity or custom views using the Regarding (Case) relationship.  
  
### See also  
 [Video: SLA Enhancements in Microsoft Dynamics CRM 2015](http://youtu.be/rgmPqLsG8WI)   
 [Enable languages](../admin/enable-languages.md)
