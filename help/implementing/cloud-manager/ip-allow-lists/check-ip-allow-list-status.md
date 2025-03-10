---
title: Checking IP Allow List Status
description: Checking IP Allow List Status
exl-id: 5ddea04f-3720-4663-90a8-9399019bfcbe
---
# Checking IP Allow List Status {#check-allow-list-status}

Follow the steps below to determine the status of updates to your IP Allow List:

1. Click on the Status icon for the IP Allow List from the table from the **Environments** screen and select **IP Allow Lists** page.

1. Cloud Manager will display one of the following statuses, as mentioned in the section below.

## Status of an IP Allow List {#status}

The following are the definitions of status that will appears in an IP Allow List:

* **Applied**: IP Allow List is successfully applied on environments.  The environments and service can be seen as shown in the image below.

* **Updating**: An update to the IP Allow List which may include one or more apply or unapply is in process. Each Apply and Unapply will be  listed along with Not Started/In Progress/Complete or Failed.

* **Failed**: One or more apply or unapply process in an Update failed. Each Apply and Unapply will be  listed along with Complete or Failed.
   * The status will be Failed, even if one apply/unapply in the update fails. 
   * The status will remain Failed until all failures are cleared. User must select the retry icon next to the status to clear the failure.
   * User will not be able to Update or Delete IP Allow List while the status is Failed.

* **Deleting**: Delete request is in progress. This involves unapply of all services. Each Unapply will be  listed along with Not Started/In Progress/Complete or Failed.
Once Delete operation is completed, the IP Allow List will:
   * No longer appear in the IP Allow List table.
   * No longer be applied on any service in the program in Cloud Manager.

* **Delete Failed**: One or more unapply process in a Delete operation failed. Each Unapply will be  listed along with Complete or Failed.

   * The status will be Delete Failed, even if one unapply fails. 
   * The status will remain Delete Failed until all failures are cleared. User must select Delete from the **...** menu at the far right of the row in the table to clear any failure. 
   * User will not be Allow to Update IP Allow List while the status is Failed.

## Pre-existing CDN Configurations for IP Allow Lists {#pre-existing-cdn}

Customers with environments that includes pre-existing CDN configurations for IP Allow Lists, SSL Certificates or Custom Domain Names will see the following message in the the **IP Allow List** and the **Environment** details page. The message displayed on the UI will disappear once the customer has fully migrated all pre-existing environment configurations via the UI and it may take 1-2 business days for the message to disappear.

>[!NOTE]
>In order to see and manage the pre-existing configurations they must be added via the UI. Refer to [Adding an IP Allow List](/help/implementing/cloud-manager/ip-allow-lists/add-ip-allow-lists.md) for more details.

![](/help/implementing/cloud-manager/assets/ip-allow-list-message1.png)

![](/help/implementing/cloud-manager/assets/ip-allow-list-message2.png)
