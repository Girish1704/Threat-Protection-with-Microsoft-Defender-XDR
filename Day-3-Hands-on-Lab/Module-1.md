# Hands on Labs - Day 3 
## Estimated Duration: 90 Minutes
### Exercise: 1

### Task 1: Deploy Microsoft Defender for Identity Sensor on Domain Controllers

In this task you will install and configure the Defender for Identity sensor on a domain controller to monitor identity-based threats.

Install the AD DS role to enable the server to function as a domain controller.

1. Open **Server Manager**:
   - Click the **Start** button and select **Server Manager**, or type "Server Manager" in the search bar and press **Enter**.

      ![](../media/E1T0S1.png)

2. Launch the **Add Roles and Features Wizard**:
   - In Server Manager, click **Manage** in the top-right corner, then select **Add Roles and Features**.

      ![](../media/E1T0S2.png)

3. Configure the Wizard:
   - Select **Role-based or feature-based installation**, then click **Next**.
   - Choose your server (e.g., `DC01`) from the server pool, then click **Next**.
   - In the "Server Roles" list, check **Active Directory Domain Services**.
   - When prompted, click **Add Features** to include required tools, then click **Next**.

      ![](../media/E1T0S3.png)

4. Complete the Installation:
   - Skip the "Features" page by clicking **Next**.
   - Review the AD DS information page, then click **Next**.
   - Confirm your selections and click **Install**.
   - Wait for the installation to complete (this may take a few minutes).

      ![](../media/E1T0S4.png)

   > **Note:** Do not close Server Manager after installation; the next step begins from there.

1. Start the Promotion Process:
   - In Server Manager, click the yellow notification flag and select **Promote this server to a domain controller**.

      ![](../media/E1T0S5.png)

1. Configure the Deployment:
   - In the wizard, select **Add a new forest**.
   - Enter a root domain name `defenderxdr.internal`, then click **Next**.

      ![](../media/E1T0S6.png)

1. Set Domain Controller Options:
   - Set both **Forest Functional Level** and **Domain Functional Level** to **Windows Server 2016** (or your server’s version).
   - Ensure **Domain Name System (DNS) server** is checked.
   - Enter a **Directory Services Restore Mode (DSRM)** password (e.g., `P@ssw0rd123!`), then click **Next**.

      ![](../media/E1T0S7.png)

1. Proceed Through Additional Options:
   - Ignore DNS delegation warnings (safe for lab environments), then click **Next**.
   - Accept the default **NetBIOS domain name** `DEFENDERXDR`, then click **Next**.
   - Use default paths for the AD DS database, logs, and SYSVOL (e.g., `C:\Windows\NTDS`), then click **Next**.

      ![](../media/E1T0S8.png)

1. Finalize and Install:
   - Review your selections, then click **Next**.
   - Run the prerequisites check. Resolve any critical errors, then click **Install**.
   - Wait for the process to complete; the server will restart automatically.

      ![](../media/E1T0S9.png)

   > **Note:** The restart is required to apply the domain controller configuration.

1. Sign in to the Microsoft Defender Portal, open a Edge browser and navigate to `security.microsoft.com`.

   - If you see the **Sign in to Microsoft Azure** tab, you will see the login screen. Enter the following email/username, and click on **Next**.

   * **Email/Username:** <inject key="AzureAdUserEmail"></inject>
     
     ![](./media/login2.png)

   - Now enter the following password and click on **Sign in**.
   
   * **Password:** <inject key="AzureAdUserPassword"></inject>

     ![](./media/Lab-01-task1-password.png) 

      >**Note:** Take a moment to allow the option panel to fully load on the security portal.
      
1. On the Microsoft Defender page, select **Settings** and select **Identities** and you will be navigated to **Microsoft Defender for Identity** page.

      ![](../media/E1T1S10.png)

      >**Note:** Please wait while the **identities** page loads—this may take a few minutes.

1. To create a Sensor, in the **Identities** section, click **Sensors** at the top, then select **Add sensor** in the top-right corner.

      ![](../media/E1T1S11.png)

1. On the **Simplify your installation process** pop-up, click **Continue with classic sensor**.

      ![](../media/E1T1S12.png)

1. A pop-up will display a **Download installer** button and an **Access key**. Click **Download installer** to download `Azure ATP Sensor Setup.zip`.

      ![](../media/E1T1S13.png)

1. Copy the **Access key** to your clipboard which will be used during the installation.

      ![](../media/E1T1S14.png)

1. From your VM, navigate to the downloaded `Azure ATP Sensor Setup.zip` file in the Downloads folder, extract it to `C:\ATP`, and run `Azure ATP Sensor Setup.exe` as administrator.

      ![](../media/E1T1S15.png)

1. On the Setup wizard follow the below steps:
     - Accept the license terms and click **Next**.
     - Enter the **Access key** copied earlier and click **Next**.
         ![](../media/E1T1S116a.png)

     - Choose the default installation path (e.g., `C:\Program Files\Azure Advanced Threat Protection Sensor`) and click **Install**.
   - Wait for the installation to complete.

1. Return to the Microsoft Defender portal, go to **Settings** > **Identities** > **Sensors**, find the sensor for `defenderxdr.internal`, and verify that the **Status** shows **running** within 5–10 minutes.

      ![](../media/E1T1S17.png)

### Task 2: Simulate and Detect Lateral Movement Attacks (Read-Only)

In this task you will simulate DC Sync attacks and detect them using Defender for Identity.
1. Open **PowerShell (Admin)** and navigate to the below mentioned directory.

      ```powershell
      cd 
      ```
1. In the same PowerShell session, run the below command to simulate a DC Sync attack and extract the credentials of the krbtgt account.

     ```powershell
     mimikatz.exe "lsadump::dcsync /domain:yourdomain.com /user:krbtgt" exit
     ```

1. Now you can check your alerts in the Microsoft Defender portal, navigate to **Incidents & alerts** in the left-hand navigation pane.

      ![](../media/E1T2S3.png)

1. Click **Alerts** to view the alerts queue.

      ![](../media/E1T2S4.png)

1. You will find alerts with the below names
     - **Suspected Pass-the-Hash attack (NTLM)** for the Pass-the-Hash simulation.
     - **Suspected DCSync attack** for the DC Sync simulation.

      ![](../media/E1T2S5.png)

### Task 3: Investigate Threats and User Timelines 

In this task you will analyze using user timelines and alert details in the Defender portal.

1. In the Microsoft Defender portal, in the search bar, type `demouser` and select it.

      ![](../media/E1T3S1.png)

1. Click on **Go to user page**

      ![](../media/E1T3S2.png)

1. In the user profile, click the **Timeline** tab and you will be able to see the events like `Lateral movement attacks` and `mimikatz credential theft tool` alerts

      ![](../media/E1T3S3.png)

### Task 4: Integrate Defender for Identity with Microsoft 365 Defender Portal

In this task you will enable integration to view Defender for Identity incidents in the unified Microsoft 365 Defender portal and Microsoft Sentinel.

1. Navigate to `portal.azure.com` from your browser and Sign in with the default user.

      ![](../media/E1T4S1.png)

1. Navigate to **Microsoft Sentinel** and select `loganalyticworkspace`.

      ![](../media/E1T4S2.png)

1. In the left-hand pane, click **Data connectors**.

      ![](../media/E1T4S3.png)

1. Search for **Microsoft Defender XDR** and make sure that the status is **Connected**.

      ![](../media/E1T4S4.png)

1. Return to the Defender portal and navigate to **Incidents & alerts** > **Incidents** in the left-hand pane.

      ![](../media/E1T4S5.png)

1. Filter incidents by **Data source** = **Microsoft Defender for Identity** and verify that incidents from the simulated attacks are visible.

      ![](../media/E1T4S6.png)

### Task 5: Review and Run Advanced Hunting Queries for Identity Signals

In this task you will use advanced hunting queries in the Defender portal to detect identity-based threats.

1. In the Microsoft Defender portal, click **Hunting** in the left-hand navigation pane and select **Advanced hunting** from the menu.

      ![](../media/E1T5S1.png)

1. In the query editor, paste the following KQL query to detect Lateral movement attacks:

   ```kql
   AlertInfo
   | where Category == "LateralMovement"
   | summarize AlertCount = count() by Title, DetectionSource, bin(Timestamp, 1h)
   | order by AlertCount desc
   ```
   - Click **Run query** to execute.

      ![](../media/E1T5S2.png)

1. Review the results table for Lateral Movement attacks, 

      ![](../media/E1T5S3.png)

1. Click **Save** in the top-right corner.
   - Name the query as `Lateralmovementattacks`.
   - Select **Save as query** and click **Save**.

      ![](../media/E1T5S4.png)
