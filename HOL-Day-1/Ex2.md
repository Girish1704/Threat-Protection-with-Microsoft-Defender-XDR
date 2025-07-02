# Exercise 2: Threat Investigation and Security Posture Management with Microsoft Defender

## Overview

In this exercise, you will investigate and remediate security incidents using Microsoft Defender XDR, gaining hands-on experience with incident timelines and response actions. You will also configure anti-phishing and Safe Links policies to protect users from malicious emails and links. Finally, you will implement and monitor security posture settings in Defender for Office 365 to ensure ongoing protection and compliance across your organization.

## Objectives

- Task 1: Investigate and remediate Incidents in Microsoft Defender XDR  
- Task 2: Configure Anti-Phishing and Safe Links Policies

## Task 1: Investigate and Remediate Incidents in Microsoft Defender XDR

In this task, you’ll investigate a phishing email, take appropriate actions, submit it to Microsoft for review, and monitor the automated investigation and alerts triggered in Microsoft Defender XDR.

1. Use Outlook or another email client to send a test phishing message with suspicious links to your lab user.

   ![](./media/g25-1.png)

1. Go to the [Microsoft 365 Defender Portal](https://security.microsoft.com) Navigate to **Email & collaboration** → **Explorer**  

1. Find and click the suspicious message titled **Test Phishing**.

   ![](./media/g25-2.png)

   > **Note:** It may take 2–3 minutes after the phishing email is delivered for it to appear in Threat Explorer.

1. Click the message to view its details, then click **Take action** to initiate response actions.

   ![](./media/g25-3.png)

1. Configure the following settings:

    - Enable **Show all response actions**  
    - Choose **Move to Junk**  
    - Submit to Microsoft for review:
      - Select **I've confirmed it's a threat**
      - Choose **Phish** as the category  
    - Enable **Initiate automated investigation**

1. Click **Next** to continue.

   ![](./media/g25-4.png)

1. Set a name like `report-phish` and confirm impacted users and actions.

   ![](./media/g25-5.png)

1. Navigate to **Incidents & alerts** → **Alerts** and Look for the alert titled **Administrative action submitted by an Administrator**.

   ![](./media/g25-6.png)

1. Click the alert to open details, then click **Manage alert**.

   ![](./media/g25-7.png)

1. In the Manage alert pane:  
 
    - Set **Status** to `In progress`  
    - Assign to your lab user  
    - Set **Classification** to `True positive – Phishing`  
    - Click **Save**

      ![](./media/g25-8.png)

1. Go to **Email & collaboration** → **Investigations**.Click on the newly triggered investigation linked to your phishing test.

    ![](./media/g25-9.png)

1. Review the investigation graph showing the alert path, analyzed entities, and final result.

   ![](./media/g25-10.png)

   >**Note:** It may take **10–15 minutes** for the automated investigation to complete and display results.

   > You've successfully investigated and responded to a phishing incident using Microsoft Defender XDR.

## Task 2: Configure Anti-Phishing and Safe Links Policies

1. Go to the [Microsoft 365 Defender Portal](https://security.microsoft.com) Navigate to **Email & collaboration** → **Policies & rules** → **Threat policies**  and click on **Anti-phishing** under the Policies section.

    ![](./media/g25-11.png)

1. Click **+ Create** to start configuring a custom anti-phishing policy.

    ![](./media/g25-12.png)

1. Enter a name such as `Anti-Phish` and click **Next**.

    ![](./media/g25-13.png)

1. Add your lab user under **Users**, then click **Next**.

    ![](./media/g25-14.png)
   
1. Set **Phishing email threshold** to `4 - Most Aggressive` for strict detection.(Other impersonation settings can be skipped for now.)

    ![](./media/g25-15.png)

1. Enable all recommended options under trusted senders and spoofing intelligence and click **Next** to continue.
    
    -  Enable mailbox intelligence  
    -  Enable impersonation intelligence  
    -  Enable spoof intelligence  

       ![](./media/g25-16.png)

1. Set the following actions for detected spoofing and impersonation and click **Next** to continue.

   - Move impersonated messages to **Junk Email**
   - Honor DMARC policies:
      - If `p=quarantine` → move to Junk
      - If `p=reject` → quarantine
      - If spoof intelligence triggers → move to Junk

        ![](./media/g25-17.png)

1. Go to **Email & collaboration** → **Policies & rules** → **Threat policies**.Click on **Safe Links** under the Policies section.

   ![](./media/g25-18.png)

1. Click **+ Create**, name the policy (e.g., `Anti-Safe`), then click **Next**.

   ![](./media/g25-19.png)

1. Add your lab user (e.g., `ODL_User 1777538`) under **Users** and proceed.

   ![](./media/g25-20.png)

1. Enable all recommended protection settings. Click **Next**.

    - Real-time URL scanning  
    - Safe Links for email, Teams, and Office apps  
    - Track user clicks  
    - Wait for URL scanning before delivery  

      ![](./media/g25-21.png)

1. Go to **Email & collaboration** → **Policies & rules** → **Alert policy**.

   ![](./media/tbh-1-1.png)

1. Click **+ New Alert Policy**.

   ![](./media/tbh-1-2.png)

1. Fill in the following details and click **Next**.
   
    - **Name**: `Alert-Safe`
    - **Severity**: `High`
    - **Category**: `Threat management`

       ![](./media/tbh-1-3.png)

1. Set the alert logic and click **Next**.
  
    - **Activity**: `Detected malware in an email message`
    - **Mail direction**: `Inbound`
    - **Trigger**: `Every time an activity matches the rule`

       ![](./media/tbh-1-4.png)

1. Add the email recipient to get notified about the alert.Click **Next**

   ![](./media/tbh-1-5.png)

1. Enable the alert immediately and click **Submit** to create the policy.

   ![](./media/tbh-1-6.png)

1. Send an email to the lab user with the following links:

   - [https://www.amtso.org/check-desktop-phishing-page/](https://www.amtso.org/check-desktop-phishing-page/)
   - [https://malware.wicar.org/data/eicar.com.txt](https://malware.wicar.org/data/eicar.com.txt)

      ![](./media/tbh-1-7.png)

      > **Note:** These are safe test links designed to simulate phishing and malware detection.

1. Go to **Email & collaboration** → **Explorer** and locate the **Test-safe** email.

   ![](./media/tbh-1-8.png)

1. Click **Open email entity** to inspect the detection and delivery information.

1. Check details like quarantine status and detection technologies used.

   ![](./media/tbh-1-9.png)

1. Navigate to **Investigation & response** → **Incidents**.Open the incident titled **Alert-Link**.

   ![](./media/tbh-1-10.png)

   >  Dive deep into the incident: review alerts, evidence, entities involved, and the automated investigation trail.

   >  You’ve now created an alert policy, triggered it with test emails, and followed the investigation trail using Defender XDR.

## Review

1. In this exercise, you learnt how to investigate and respond to security incidents using Microsoft Defender XDR.  
2. You also configured anti-phishing and Safe Links policies and monitored Defender for Office 365 to strengthen overall security posture.

## Congratulations! You have successfully completed the Lab.
