# Exercise 2: Protecting Microsoft 365 SaaS Apps with Microsoft Defender for Cloud Apps

## Overview

In this exercise, you will explore how to secure Microsoft 365 cloud services like SharePoint, OneDrive, and Exchange using Microsoft Defender for Cloud Apps. You'll connect Microsoft 365 as a cloud app, configure real-time session policies, and create custom activity detection rules to detect and block risky behavior.

## Objectives

- Task 1: Connect and Onboard a SaaS App to Microsoft Defender for Cloud Apps  
- Task 2: Configure Session Policies to Monitor and Block Risky Behavior  
- Task 3: Investigate Alerts and Create Custom Detection Policies  

## Task 1: Connect and Onboard a SaaS App to Microsoft Defender for Cloud Apps

1. Go to the [Azure Portal](https://portal.azure.com).

1. In the search bar, type **Microsoft Entra ID** and select it.  
   ![](./media/g-1-1.png)

1. In the **Overview** pane, select **Users** under the **Manage** section.  
   ![](./media/g-1-2.png)

1. From the list of users, click on your assigned user (e.g., `ODL_User 1777538`).  
   ![](./media/g-1-3.png)

1. In the user blade, click on **Licenses** and ensure **Microsoft 365 E5 (no Teams)** or an equivalent license is assigned and active.  
   ![](./media/g-1-4.png)

1. Open [https://compliance.microsoft.com](https://compliance.microsoft.com) in the browser.

1. If prompted, click **Switch to the new portal yourself** to access Microsoft Purview.  
   ![](./media/g-1-5.png)

1. On the Microsoft Purview homepage, select the **Audit** tile.  
   ![](./media/g-1-6.png)

1. If audit logging is not enabled, click **Start recording user and admin activity**.

   > Note: This may take a few hours to activate. You may proceed with the rest of the exercise while it initializes.

1. Go back to the [Azure Portal](https://portal.azure.com), search for **Windows Azure Active Directory**, and select it.
  
    ![](./media/g-4-1.png)

1. From the left navigation, select **Conditional Access** and Click on **+ New policy**.  
    ![](./media/g-4-2.png)

1. Name the policy: `MCAS – M365 Session Control`.

1. Under **Assignments > Users**, choose **Select users and groups** and add your lab user.  
    ![](./media/g-4-3.png)

1. Under **Assignments > Target resources**, choose **Cloud apps** → **Office 365**.  
    ![](./media/g-4-4.png)

1. Under **Access controls > Session**, enable **Use Conditional Access App Control**, and select **Use custom policy**.  
    ![](./media/g-4-5.png)

1. Scroll down, toggle **Enable policy** to **On**, and click **Create**.  
    ![](./media/g-4-6.png)

1. Open [Microsoft Defender Portal](https://security.microsoft.com) and select **Settings** from the left menu.

1. Under **Settings**, select **Cloud Apps**.  
    ![](./media/g-1-7.png)

1. Expand **Information Protection** and select **Files**.

1. Enable the checkbox for **Enable file monitoring**, then click **Save**.  
    ![](./media/g-1-8.png)

1. Navigate to **Settings** → **Cloud Apps** → **App Connectors**.  
    ![](./media/g-1-9.png)

1. Click **Connect an app**, and select the following:

    - Microsoft Entra ID Management events  
    - Microsoft Entra ID Sign-in events  
    - Microsoft Entra ID Apps  
    - Microsoft 365 activities  
    - Microsoft 365 files  
    ![](./media/g-1-10.png)

1. Click **Connect Microsoft 365** and complete the authentication.

1. After successful connection, you will see the confirmation message:  
    **“Great, Microsoft 365 is connected.”**  
    ![](./media/g-1-11.png)

1. Click **Done** to complete onboarding.

1. On the **App Connectors** page, verify that Microsoft 365 shows a **Connected** status.  
    ![](./media/g-1-12.png)

   > Note: It may take up to 30–60 minutes for the connection status to update.

## Task 2: Configure Session Policies to Monitor and Block Risky Behavior

1. In the [Microsoft Defender Portal](https://security.microsoft.com), go to **Cloud Apps** → **Policy management**.

1. Click **Create policy** → **Session policy**.  
   ![](./media/g-1-13.png)

1. Configure the fields:

   - Policy template: `Block download based on real-time content inspection`  
   - Policy name: `Block-All-Download`  
   - Category: `DLP`  
   - Session control type: `Control file download (with inspection)`  
   ![](./media/g-3-1.png)

1. Under **Actions**, select **Block**, enable **Send alert as email**, and enter the lab email address.  
   ![](./media/g-3-2.png)

1. Click **Create** to activate the policy.

   > Note: Ensure your Conditional Access policy is routing sessions through Microsoft Defender for Cloud.

1. Open an incognito browser and go to [https://www.office.com](https://www.office.com).

1. Sign in using your lab credentials.  
   ![](./media/g-3-3.png)

1. From the left pane, go to **Apps** → **SharePoint**.  
   ![](./media/g-3-4.png)

1. Select the **Communication site** under **Frequent sites**.  
   ![](./media/g-3-5.png)

1. Go to the **Documents** library and upload `msedge.exe` via **Upload** → **Files**.  
    ![](./media/g-3-6.png)  
    ![](./media/g-3-7.png)

1. After uploading, click the file and select **Download**.  
    ![](./media/g-3-8.png)

1. The download should be blocked with a message:  
    **"Download blocked – Downloading msedge.exe is blocked by your organization’s security policy."**  
    ![](./media/g-3-9.png)

## Task 3: Investigate Alerts and Create Custom Detection Policies

1. In the [Microsoft Defender Portal](https://security.microsoft.com), go to **Cloud Apps** → **Activity log**.  
   ![](./media/p-0-1.png)

1. Filter for:

   - App: `Microsoft SharePoint Online`  
   - Activity type: `Download file`  
   - File name: `msedge.exe`  
   ![](./media/p-0-2.png)

1. Go to **Incidents & alerts** → **Alerts**, and look for `Block-All-Download`.

1. Click the alert, then select **Open alert page**.  
   ![](./media/p-1-6.png)  
   ![](./media/p-1-7.png)

1. Click **Investigate in activity log**.  
   ![](./media/p-1-8.png)

1. Go to **Cloud Apps** → **Policy management**, and click **Create policy** → **Activity policy**.  
   ![](./media/p-1-1.png)

1. Configure the following:

   - Policy name: `Detect Suspicious File Download – msedge.exe`  
   - Severity: `High`  
   - Category: `Threat detection`  
   - Act on: `Single activity`  
   - Activity type: `Download file`  
   - File name: `msedge.exe`  
   - App: `Microsoft SharePoint Online`  
   ![](./media/p-1-2.png)

1. Click **Edit and preview results**, review matches, then click **Save filters**.  
   ![](./media/p-1-3.png)

1. Under **Alerts**, enable **Send alert as email**, add a valid address, set daily alert limit to `5`.

10. Click **Create** to save and activate the policy.  
    ![](./media/p-1-4.png)

11. Simulate a download again from [https://www.office.com](https://www.office.com) by logging in, opening SharePoint, and downloading `msedge.exe`.  
    ![](./media/g-3-6.png)  
    ![](./media/g-3-7.png)  
    ![](./media/g-3-8.png)

1. Open your email inbox and locate the alert email titled:  
    `Alert - Detect Suspicious File Download – msedge.exe`  
    ![](./media/p-1-5.png)

1. In the portal, go to **Incidents & alerts** → **Alerts**, and open the custom alert.  
    ![](./media/p-1-6.png)

1. Click **Open alert page** → **Investigate in activity log**.  
    ![](./media/p-1-7.png)  
    ![](./media/p-1-8.png)

1. Review the event details:

    - User name  
    - File name  
    - App used  
    - IP address  
    - Device info  
    - Triggered policy  
    ![](./media/p-1-9.png)

## Review

- You connected and onboarded Microsoft 365 into Microsoft Defender for Cloud Apps.  
- You configured session policies to monitor user activity and block downloads from unmanaged devices.  
- You created a custom detection policy to alert on suspicious activity and verified it through simulated actions.

## Click Next to continue
