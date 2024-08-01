<br>

<h1 align="center">Microsoft Entra ID Logging (Tenant Level)</h1>

<br>

<p align="center">
<img width="900" src="https://github.com/user-attachments/assets/757f9ce2-0dcd-4e73-a036-1634c2039876" alt="Banner"/>
<br />

<br />

In this lab we’re going to continue Ingesting Logs into our Central Log Repository, aka our Log Analytics Workspace.

We’re going to bring in the **Logs from Microsoft Entra ID**, specifically the **Audit Logs** as well as the **Sign-in Logs**.

<br>

>   <details close> 
>   
> **<summary> 📝 Refresher</summary>**
> 
> Just as a reminder, there are 3 Tiers of Logging in Azure:
> 
> ![azure portal](https://github.com/user-attachments/assets/adbf8dbb-a391-48e4-af06-4886ade26ae6)
> 
> 1. There’s the **Tenant Level Logging** ➜ which are the **Microsoft Entra ID Logs** ➜ which consist of **Sign-in Logs** and **Audit Logs**.
>   
>     - Whenever someone tries to sign in or log in to Microsoft Entra ID, a Log is created.
>   
>     - Or whenever an action is performed on a User Account or a Group, that counts as being Tenant Level Logging.
>   
>     - And these are the Logs we’re going to ingest into the Log Analytics Workspace in this Lab.
> 
> <br>
>   
> 2. In subsequent Labs we’re going to ingest the **Activity Logs**.
>    
>     - Activity Logs are basically for whenever you create or delete resources, or do anything on the Azure Portal with any of those Resources.
>
> <br>
> 
> 3. And then the **Resource Level Logs** ➜ these are Logs within the actual **Dataplane of the Resources**.
>  
>     - So this might be something like the Sign-in Logs inside of the actual Windows VM for example, or the Syslog Logs inside of the Linux VM.
>   
>     - Or doing things like actually manipulating files inside of the Azure Storage Account for example – those are all considered Resource Level Logs.
> 
> <br>
> 
>   </details>

<br>

### ➡️ High-Level Steps of what we’re going to do in this Lab:

1. Create Diagnostic Settings to Ingest Microsoft Entra ID Logs ➜ Sign-in and the Audit Logs

2. Create a Dummy User

3. Perform some Actions & Observe Logging:
    - Mass Sign-in Failures (***Sign-in Logs***)
    - Assignment of “Global Administrator” to User (***Audit Log***)

<br>

✔️ We’re going to Perform these Actions and see what kind of Logs were Created on the backend.

✔️ Then we'll make sure they’re being brought into our Log Analytics Workspace.  

<br>

<br>

<details close> 
<summary> <h2> 1️⃣ Create Diagnostic Settings to ingest Azure AD Logs</h2> </summary>
<br>

> So getting right into things ➜ the first thing we’re going to do is **Create the Diagnostic Settings** inside of **Microsoft Entra ID**.
> 
> This will allow us to **Export the Logs** into our **Log Analytics Workspace**.

<br>

We’ll go to the **"Azure Portal"** ➜ and open **"Microsoft Entra ID"**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We’ll then click on the **"Diagnostic Settings"** Blade:

➜ We can see all the different types of Logs that we can bring in.

➜ But for this Lab we’re just going to concern ourselves with the **Audit Logs** and the **SignInLogs**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

Click on ➕ **Add diagnostic setting** to create a new Diagnostic Setting:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

- The **"Diagnostic setting name"** can be anything ➜ I’ll just name mine ```ds-audit-signin```

- For the **Logs’ Categories** ➜ check ☑️ **AuditLogs** and ☑️ **SignInLogs**

- **"Destination details"** ➜ check ☑️ **Send to Log analytics workspace** ➜ select ```LAW-Cyber-Lab-01```
    - ⚠️ Make sure it’s going to your actual LAW ➜ don’t send it to the *DefaultWorkspace*

- Click 💾 Save

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can confirm that our **Diagnostic Setting** was successfully created:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

>   <details close> 
>   
> **<summary> 📋 Summary</summary>**
> 
> We’re basically taking our time to ingest all of the Logs for all of our Resources into our LAW.
> 
> Previously we did the VMs and the NSGs, next we’re doing Microsoft Entra ID, and in the subsequently labs we’re going to finish up with the Activity Logs and with the rest of our Dataplane Logs.
>
>   </details>

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2> 2️⃣ Verify that the 2 Tables were created in the Log Analytics Workspace</h2> </summary>
<br>

> We can check the Workspace just to make sure the actual Tables have been created, which will ultimately hold the Logs.

<br>

We’ll go to our **Log Analytics Workspace** ➜ and click on the **"Tables"** blade:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

There should be 2 tables called **"SignInLogs"**  and **"AuditLogs"**  ➜ so we’ll search for them:


WE’LL COME BACK TO THIS!!!!!!!!!!!!!!!!!!!!!


<br>

<br>

<br>

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Create a Dummy User</h2> </summary>
<br>

> We’ll go back to Microsoft Entra ID and create a User called **"dummy_user"**.
>
> Doing this should generate an Audit Log.

<br>

Go to **"Microsoft Entra ID"** ➜ and click on the **"Users"** blade:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We’ll Add a User by clicking on **"Create a user"**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

- We can Name it ```dummy_user```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"** to Create the New User:

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

Once the New User is Created ➜ we'll open a New Private Browsing Tab ➜ And go to **portal.azure.com**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

Now we'll attempt to Log in with the New User's Credentials.

<br>

>   <details close> 
>   
> **<summary> 💡 </summary>**
>   
> Creating the User should have generated an **AuditLog**
> 
> And the attempting to Sig In with the User's crendentials should generated a **SignInLog**
> 
>   </details>

<br>

It'll have us change our **Password** ➜ so we'll just change it to ```Cyberlab123!```

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

✅ So we were able to Log In as the **Dummy User**:

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Assign the Dummy User the Role of Global Administrator</h2> </summary>
<br>

>   <details close> 
>   
> **<summary> 💡 Summary</summary>**
>   
> If you remember from the previous lab ➜ it is a big deal to assign someone in your company the **Global Administrator Role**.
>   
> Usually that will envolve some kind of change-management and more than one person overseeing it.
> 
> Or at least it will envolve more than one person knowing that that assignment is going on.
> 
>   </details>

<br>

Go back to the **"Users"** page in the Azure Portal ➜ and click on the ```dummy_user```

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

💡 Assigning **Global Admin** to this Dummy User should generate another **AuditLog**

So we can go to the **"Assigned roles"** blade ➜ and click on ➕ **Add assignments**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

search for ```global administrator``` ➜ select ☑️ **Global Administrator** ➜ and **"Add"** the Role:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can verify that the Dummy User was succcessfully assigned the **Global Administrator Role**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

  </details>

<h2></h2>

<details close> 
<summary> <h2>5️⃣ Delete the Dummy User</h2> </summary>
<br>

>   <details close> 
>   
> **<summary> 💡 Summary</summary>**
>   
> This will generate another **Audit Log**.
>   
> We should be able to eventually see all thsi actions we took in side of our Log analytics Workspace.
> 
>   </details>

<br>

We'll just go back to the **"Microsoft Entra ID"** page ➜ clik on the **"Users"** Blade

Then inside the **"dummy_user"** ➜ **"Overview"** Blade ➜ click on 🗑️ **Delete** ➜ Press **"OK"**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ That should have also created an **Audit Log** in the AuditLogs table.

  </details>

<h2></h2>

<details close> 
<summary> <h2>6️⃣ Observe AuditLogs in Log Analytics Workspace</h2> </summary>
<br>

Let's go back to our Log Analytics Workspace ```LAW-Cyber-Lab-01```

Clik on the **"Logs"** Blade ➜ and we'll Run the Query ```AuditLogs```

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can basically see the "trail" of what we did with the Dummy User:
1. Add User ➜ Created the Dummy User

2. Change User Password ➜ Changed the Password of the Dummy User after Login In using an Incognito Window.

3. Add member to role ➜ Assigned the Global Administrator Role to the Dummy User

4. Delete user ➜ Deleted the Dummy User

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>7️⃣ Simulate Brute Force Attack against Microsoft Entra ID</h2> </summary>
<br>

> We're going to Create an "Attacker User" to perform Brute-Force Attacks against our Microsoft Entra ID.
> 
> Produce 10 to 11 Failed Logins in the Azure Portal.

<br>

Back to the **Azure Portal** and to our **Log analytics Workspace** ➜ on the left we'll click on the **"Logs"** blade.

💡 This is where we can start **Querying the Logs**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

>   <details close> 
>   
> **<summary> 📝 Explanation</summary>**
> 
> Basically this is where we can practice **KQL** (Kusto Query Language) ➜ which is similar to **SQL** (Structured Query Language).
> 
> Azure will create what's called a **Table**.
> 
> You can think of a **Table** as something similar to an Excel Spreadsheet, but in this case it's like a Database to Store our Logs.
> 
>   </details>

<br>

To make sure the Logs are coming in from all 3 sources, we'll Query the different **Tables**.

1. The Table that is used to store the **Linux Logs** is called **Syslog**.

We should be able to type ```Syslog``` ➜ and click the ▶️ **"Run"** Button to Query the Syslog Log:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ Looks like the **Linux VM Logs** are actually coming in ➜ so we know it's working.

<br>

<h2></h2>

<br>

2. The Table (aka the Excel Spreadsheet) that is used to hold the Logs from our Windows VMs is called **SecurityEvent**

So next we're going to inspect the ```SecurityEvent``` Logs:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can see the SecurityEvent Logs are coming in ➜ which is great!

<br>

<h2></h2>

<br>

3. The last Table is used to hold the NSG Logs ➜ **AzureNetworkAnalytics_CL**

Again ➜ inside the LAW's **"Logs"** Blade ➜ Paste the command ```AzureNetworkAnalytics_CL``` and Run the Query:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ And we can see that the NSG Logs did come in.

<br>

<h2></h2>

  </details>

<br>

<br>

<br>

# Summary

<br>

Just to recap, in this lab we:

1. Created an **Azure Storage Account** for the Network Security Groups’ Flow Logs.

2. Enabled **NSG Flow Logs**.

3. Created **Data Collection Rules** to specify which Logs to collect from our Windows and Linux VMs.

4. Manually installed the **Log Analytics Agent** locally on the Virtual Machines.

5. **ueried the Log Analytics Workspace** for our VMs Logs and for our NSGs Logs ➜ to see if they were showing up there.

<br>

After completing this lab, we are now successfully sending Logs from both the VMs and the NSGs into the Log Analytics Workspace.

This is why we can Query things like Syslog and see all the Linux VM Logs.

In subsequent labs we’ll do the same thing for our other resources as well as for our **Microsoft Entra ID Logs**.


<br />

<br />

<br />  

<br /> 

<br />

<br />  

<br /> 

<br />

<br />

 
