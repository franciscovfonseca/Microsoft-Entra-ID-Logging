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

<details close> 
<summary> <h4> 💡 High-Level Steps of what we’re going to do in this Lab</h4> </summary>

<br>

    1. Create Diagnostic Settings to Ingest Microsoft Entra ID Logs ➜ Sign-in and the Audit Logs

    2. Create a Dummy User

    3. Perform some Actions & Observe Logging:
        
        - Mass Sign-in Failures (Sign-in Logs)
        - Assignment of “Global Administrator” to User (Audit Log)

<br>

✅ We’re going to Perform these Actions and see what kind of Logs were Created on the backend.

✅ Then we'll make sure they’re being brought into our Log Analytics Workspace.  

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2> 1️⃣ Create Diagnostic Settings to ingest Azure AD Logs</h2> </summary>
<br>

> So getting right into things ➜ the first thing we’re going to do is **Create the Diagnostic Settings** inside of **Microsoft Entra ID**.
> 
> This will allow us to **Export the Logs** into our **Log Analytics Workspace**.

<br>

We’ll go to the **"Azure Portal"** ➜ and open **"Microsoft Entra ID"**

![azure portal](https://github.com/user-attachments/assets/9a5e6422-6307-49cd-a783-678965e43e9a)

We’ll then click on the **"Diagnostic Settings"** Blade:

![azure portal](https://github.com/user-attachments/assets/49abd42e-bcfb-406e-8a7d-1f5b4f5e3431)

➜ We can see all the different types of Logs that we can bring in.

➜ But for this Lab we’re just going to concern ourselves with the **Audit Logs** and the **SignInLogs**

Click on ➕ **Add diagnostic setting** to create a new Diagnostic Setting:

![azure portal](https://github.com/user-attachments/assets/61fa3911-9378-405f-945f-abe300ae0e63)

- The **"Diagnostic setting name"** can be anything ➜ I’ll just name mine ```ds-audit-signin```

- For the **Logs’ Categories** ➜ check ☑️ **AuditLogs** and ☑️ **SignInLogs**

- **"Destination details"** ➜ check ☑️ **Send to Log analytics workspace** ➜ select ```LAW-Cyber-Lab-01```
    - ⚠️ Make sure it’s going to your actual LAW ➜ don’t send it to the *DefaultWorkspace*

- Click 💾 Save

![azure portal](https://github.com/user-attachments/assets/3eafe5fb-ff3d-4e9a-8ae8-0206b0ba947a)

✅ We can confirm that our **Diagnostic Setting** was successfully created:

![azure portal](https://github.com/user-attachments/assets/3cad6cee-4ee7-4fee-bbe0-415b16290d13)

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

There should be 2 tables called **"SignInLogs"**  and **"AuditLogs"**  ➜ so we’ll search for them:

![azure portal](https://github.com/user-attachments/assets/928a921a-3c47-48d0-bd3e-6e292b2f9fab)

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

![azure portal](https://github.com/user-attachments/assets/efe992b0-6bfd-4c3e-a8ab-0334a2cd9085)

We’ll Add a User by clicking on **"Create a user"**

![image](https://github.com/user-attachments/assets/0b5b50f5-7d74-4b98-bfcb-c26f3484ed16)

- We can Name it ```dummy_user```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"** to Create the New User:

![image](https://github.com/user-attachments/assets/b292f9c4-e08a-4637-8d54-e8ed0ae90847)

Once the New User is Created ➜ we'll open a **New Private Browsing Tab** ➜ And go to **portal.azure.com**

![image](https://github.com/user-attachments/assets/6259e712-0d35-4dd5-ac5e-5420444b3e38)

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

![image](https://github.com/user-attachments/assets/0f1f5acd-eb5e-4162-88eb-457005c263cf)

It'll have us change our **Password** ➜ so we'll just change it to ```Cyberlab123!```

![image](https://github.com/user-attachments/assets/4a1715de-757c-45de-8268-912a9f601d55)

✅ So we were able to Log In as the **Dummy User**:

![image](https://github.com/user-attachments/assets/822e7c4a-a0f3-4a9d-b98a-5c64b00bad8d)

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

![image](https://github.com/user-attachments/assets/9f944eb3-0957-44f1-b360-e57ea1274f20)

💡 Assigning **Global Admin** to this Dummy User should generate another **AuditLog**

So we can go to the **"Assigned roles"** blade ➜ and click on ➕ **Add assignments**

![azure portal](https://github.com/user-attachments/assets/90260749-6672-4430-9599-daf040f3a503)

search for ```global administrator``` ➜ select ☑️ **Global Administrator** ➜ and **"Add"** the Role:

![azure portal](https://github.com/user-attachments/assets/8b5dbea6-8dad-4bbe-8da2-8d6f9610a52a)

✅ We can verify that the Dummy User was succcessfully assigned the **Global Administrator Role**:

![azure portal](https://github.com/user-attachments/assets/3a5a9b39-4574-432e-9c32-090b9c893fe6)

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

![azure portal](https://github.com/user-attachments/assets/6af1ec9b-6c5a-417a-afda-8a15a11300f5)

✅ That should have also created an **Audit Log** in the AuditLogs table.

  </details>

<h2></h2>

<details close> 
<summary> <h2>6️⃣ Observe AuditLogs in Log Analytics Workspace</h2> </summary>
<br>

Let's go back to our Log Analytics Workspace ```LAW-Cyber-Lab-01```

Clik on the **"Logs"** Blade ➜ and we'll Run the Query ```AuditLogs```

![azure portal](https://github.com/user-attachments/assets/f7ff5ad4-d055-468f-81b2-cc56b88ea500)

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
> We'll then attempt to login 10 times from the Portal just to Generate some Logs and then we'll inspect those Failed SigInLogs.

<br>

Go to **"Microsoft Entra ID"** ➜ clik on the **"Users"** Blade

![azure portal](https://github.com/user-attachments/assets/4ee48bdf-2603-4892-8f97-c916300a66c3)

 We're then going to **"Create new User"**:

![azure portal](https://github.com/user-attachments/assets/55c9e4dc-792a-4271-a24c-87a1d960dcbb)

- We can name this New User ```attacker```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"**

![image](https://github.com/user-attachments/assets/bbaf0bef-ce61-452e-abae-992dc7a121c1)

Once the New User is Created ➜ we'll open a **New Private Browsing Tab** ➜ And go to **portal.azure.com**

![image](https://github.com/user-attachments/assets/a5f4e2bc-44b9-4dca-9086-3108a10f9856)

We'll attempt to properly Log in once with the Credentials of the ```attacker``` user:

![image](https://github.com/user-attachments/assets/e1a7e093-24c0-4c38-9f3b-a4690639b957)

And then we'll **Reset the Password** to ```Cyberlab123!``` and Sign In.

![azure portal](https://github.com/user-attachments/assets/7df7a0b2-2d8b-4919-b569-65609024efea)

We can confirm that we Successfully Signed In with the User **"attacker"**

We can then close the Browsing Window ➜ and that in it of itself will terminate the session (*Log out*)

![azure portal](https://github.com/user-attachments/assets/9a161972-0260-4339-a95a-c9ad9be265ba)

We're now going to Fail some Sign Ins with this **"attacker"** User on purpose.

Open another **Private Browsing Window** ➜ And go to **portal.azure.com**

We'll attempt to Login 10 times with a **Wrong Password**:

![azure portal](https://github.com/user-attachments/assets/6a577350-4eef-45ac-9cf9-f19823e80806)

After that ➜ try to Login with the **Correct Password** ➜ to **Generate a Successfuly SignInLog**:

![azure portal](https://github.com/user-attachments/assets/3f567f32-1a46-4998-a102-6ad8a2c6ccb8)

  </details>

<h2></h2>

<details close> 
<summary> <h2>8️⃣ Observe SigninLogs in Log Analytics Workspace</h2> </summary>
<br>

We'll now go back to our **Log Analytics Workspace** ➜ and check the ```SignInLogs``` ➜ **Run the Query**

![azure portal](https://github.com/user-attachments/assets/b1f9d64c-52eb-478d-b6b8-11e3591bb0b5)

✅ You can go through the **"ResultDescription"** Tab an Identify all the **Invalid** Login Attempts:

![azure portal](https://github.com/user-attachments/assets/7362a9ec-50a8-4850-b2b2-3733fd33f48e)

<h2></h2>

  </details>

<br>

<br>

<br>

# Summary

<br>

This was a long Lab, but it was very important to understand the AuditoLogs and the SignInLogs that were coming in from **Microsoft Entra ID**.

<br>

In the Next Lab we're going to start looking at the **Subscription Level Logs** ➜ which is going to be the **Activity Log**.

<br>

💡 The Activity Logs involve all the **Creating and Changing of Resources** inside of the **Azure Portal**.


<br />

<br />

<br />  

<br /> 

<br />

<br />  

<br /> 

<br />

<br />

 
