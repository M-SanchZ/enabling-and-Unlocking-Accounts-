
![image](https://github.com/user-attachments/assets/d5241593-b121-44ee-b8df-6150bd72a7cb)






# Lab: Managing Account Lockouts, Enabling and Disabling Accounts, and Observing Logs in Active Directory

This tutorial guides you through several common tasks in Active Directory, such as dealing with account lockouts, configuring group policy for account lockout, enabling and disabling accounts, and observing logs on both the Domain Controller and client machines.

## Prerequisites
- You should have access to the **DC-1** and **Client-1** virtual machines (VMs) in your Azure portal.
- Ensure that the VMs are running and that you have the necessary permissions to make changes in Active Directory.

### Step 1: Turn on the DC-1 and Client-1 VMs (If they are off)

1. **Login to Azure Portal**:  
   Go to the [Azure Portal](https://portal.azure.com/).

2. **Navigate to Virtual Machines**:  
   In the left sidebar, click on **Virtual machines**.

3. **Turn on the VMs**:  
   - If **DC-1** and **Client-1** are turned off, click on each VM, then click **Start** to power them on.

### Step 2: Dealing with Account Lockouts

1. **Log in to DC-1**:  
   - Open **Remote Desktop Connection (RDP)** or use another method to log in to the **DC-1** machine.

2. **Pick a random user account**:
   - We are logged in as  `jane_admin` on **DC-1** machine
   - Use a previously created user account in Active Directory (you may have created this during a previous lab).
   - For this lab we use: `lak.kut` as a random user account
     
     ![image](https://github.com/user-attachments/assets/a235070e-c5e7-4a7a-9b0e-c2cb500e12d2)
     ![image](https://github.com/user-attachments/assets/dd6f32f7-6021-40f1-9e8a-a81b9a4d04fa)
     ![image](https://github.com/user-attachments/assets/6cba7368-53e5-4688-a29b-1ef4846df7a0)




4. **Attempt to log in with the wrong password**:  
   - Try to log in to the **Client-1** machine using the selected user account.
   - Enter the wrong password **10 times** to simulate failed login attempts.
   - If you notice when you input the correct credentials for `lak.kut` you will be able to log in, this is due to Group policies not being Configure
   - In the next step we will learn how to configure group polices to lock out accounts after various log in attemts.

### Step 3: Configure Group Policy to Lock Out the Account After 5 Attempts

1. **Configure Account Lockout Threshold**:
   - On **DC-1**, still logged in as  `jane_admin`, open the **Group Policy Management Console**.
     - Press **Win + R**, type `gpmc.msc`, and press **Enter**.
       ![image](https://github.com/user-attachments/assets/8f9819ab-55da-4b51-9c11-cb78b2a6522a)
       ![image](https://github.com/user-attachments/assets/94583726-12ca-4257-95d7-85d385182146)
       ![image](https://github.com/user-attachments/assets/40aa66a0-6c34-4de0-99e7-a02fdde99b4c)




2. **Edit the Default Domain Policy**:
   - In the Group Policy Management Console, navigate to **Forest > Domains > YourDomainName > Default Domain Policy**.
   - Right-click on **Default Domain Policy** and click **Edit**.
     ![image](https://github.com/user-attachments/assets/624ff421-c612-431d-a46f-64b3030c9569)
     ![image](https://github.com/user-attachments/assets/f5513892-e0b7-44a1-a91e-035b531a0e1c)
     ![image](https://github.com/user-attachments/assets/0d73d90c-f838-49f2-ad9a-b2c6e6a9302d)




3. **Configure Account Lockout Settings**:
   - Navigate to:  
     `Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy`.
     ![image](https://github.com/user-attachments/assets/997f8001-5e89-49ab-bca2-82d5447fd0c5)
     ![image](https://github.com/user-attachments/assets/99871e2a-0340-4dd7-aff0-fe53db2337ac)
     ![image](https://github.com/user-attachments/assets/2edfe3cd-df8c-49f8-b96c-cf4d8c114103)
     ![image](https://github.com/user-attachments/assets/05041de9-bcd6-4dcb-89bf-7de9aff44080)
     ![image](https://github.com/user-attachments/assets/67c89766-5d1c-4b97-892b-e36a8e379783)
     ![image](https://github.com/user-attachments/assets/876204eb-e555-4edb-b6bf-1e403650effd)

   - Double-click on **Account Lockout duration**.
    ![image](https://github.com/user-attachments/assets/dc9f7330-385e-4820-b952-84b6e664911e)
    ![image](https://github.com/user-attachments/assets/28d870be-7d80-4359-9fcb-1d7f27344295)
    ![image](https://github.com/user-attachments/assets/12c0b8d0-0caf-428c-adf6-54fc25fdab8e)
    ![image](https://github.com/user-attachments/assets/3c2de6ed-923a-480f-a202-b6315ae4fa0b)




   - Double-click on **Account Lockout Threshold**.
   - Set the value to **5 invalid logon attempts**.
   - Click **OK**.
   ![image](https://github.com/user-attachments/assets/40d2117e-0dc8-4db5-9eb7-c1059e4cec18)
   ![image](https://github.com/user-attachments/assets/1b775940-9e6b-4049-890b-5858e6bb62cc)
   ![image](https://github.com/user-attachments/assets/3d6e247b-06e7-407a-a513-fa00b83f617d)
   ![image](https://github.com/user-attachments/assets/3a169da4-3275-47c1-8138-88414acd0ed0)

     - Checking if Domain Policies configuration after changes  **Default Domain Policy**.
     ![image](https://github.com/user-attachments/assets/88e32c47-cd65-45ba-9b6f-f9320eb32988)


   











4. **Update Group Policy**:
   - Log in to **Client-1** as `jane_admin` to force accept the new policy.
   - We could wait for it to auto apply after sometime, but for the sake of this lab we will force the policy update.
   - Run `gpupdate /force` in the **Command Prompt** to apply the changes.
     
     ![image](https://github.com/user-attachments/assets/482ab8ae-eb1a-4882-899a-f69d27a9309f)
     ![image](https://github.com/user-attachments/assets/53b1011a-81c7-4f51-a080-3ed708452158)
     ![image](https://github.com/user-attachments/assets/28d60ff2-3133-454c-aa43-fbbe8cf87b7b)
     ![image](https://github.com/user-attachments/assets/a3158b25-930a-47ca-92a1-127e96c65270)





### Step 4: Test Account Lockout

1. **Attempt to log in with wrong credentials**:
   - On **Client-1**, attempt to log in using the same user account **6 times** with an incorrect password.

2. **Observe the account lockout**:
   - After 5 failed login attempts, the account should now be locked out.
   - Go back to **DC-1** and open **Active Directory Users and Computers**.
   - Locate the user account and observe that it is marked as **locked out**.
     ![image](https://github.com/user-attachments/assets/1bc8897e-43e8-457d-a3a3-90ee93f438f1)


3. **Unlock the Account**:
   - Log into **DC-1** as `jane_admin` 
   - In **Active Directory Users and Computers**, right-click on the locked-out user account and select **Unlock Account**.
   - If you can not locate the account user's name, right-click on `mydomain.com`  and click on FIND
   - Check the box that reads  **Unlock account** and APPLY changes before clicking Ok
     
     ![image](https://github.com/user-attachments/assets/e03c35cc-63a1-4206-ab99-1abe3af9c62c)
     ![image](https://github.com/user-attachments/assets/c21a3ce6-2008-4c2b-9edc-1904917cbec8)
     ![image](https://github.com/user-attachments/assets/1654f3ea-296d-41d7-a888-7d85b7f2b906)
     ![image](https://github.com/user-attachments/assets/275d0222-bada-4ec3-b2f8-5c36e28ef6fa)
     ![image](https://github.com/user-attachments/assets/a7cfaa31-1989-4d6a-b5e5-1b840697a2ee)
     ![image](https://github.com/user-attachments/assets/ec3bd41d-5dda-49ce-bf45-5aa2cd04faac)






     

5. **Reset the password**:
   - Right-click on the account again and select **Reset Password**.
   - Set a new password and click **OK**.
     ![image](https://github.com/user-attachments/assets/f5cc2c3a-db0d-4311-a713-f1e84b6be04f)
     ![image](https://github.com/user-attachments/assets/fe7d2c64-c95f-4a6b-a77c-a685d1bd7a66)
     ![image](https://github.com/user-attachments/assets/d385d378-c5cc-408c-9cb9-710a5e07b366)
     ![image](https://github.com/user-attachments/assets/08a05410-e193-47cb-bb91-fef8e2e4c913)





6. **Attempt to log in**:
   - On **Client-1**, log in with the new password and confirm that you can access the account successfully.
     ![image](https://github.com/user-attachments/assets/27bd61ef-3a43-4e1f-9034-ec09221fb9f4)
     ![image](https://github.com/user-attachments/assets/d8e0e923-cffb-42bb-9dbf-8ce6db65ff2e)


     

### Step 5: Enabling and Disabling Accounts

1. **Disable the Account**:
   - On **DC-1**, go to **Active Directory Users and Computers**.
   - Right-click on the same user account and select **Disable Account**.
     ![image](https://github.com/user-attachments/assets/0daffa12-b06c-46c5-91b9-837a528a6800)
     ![image](https://github.com/user-attachments/assets/b2a044d7-4b75-4034-8f06-8ed51cdc7f42)


     

2. **Attempt to log in**:
   - On **Client-1**, attempt to log in with the disabled account.
   - You should see an error message that indicates the account is disabled.
     ![image](https://github.com/user-attachments/assets/49cd316f-a21c-4c7c-b7d9-084fe1a1bb92)
     ![image](https://github.com/user-attachments/assets/46d62b7e-7585-47ad-9b31-06fdc583310a)



3. **Re-enable the Account**:
   - On **DC-1**, right-click on the disabled user account and select **Enable Account**.
     ![image](https://github.com/user-attachments/assets/36be3b7d-6748-4321-97a1-a166a22ae029)
     ![image](https://github.com/user-attachments/assets/d5015ef7-c4ef-4354-9e06-41a4620ce4c1)



4. **Attempt to log in**:
   - On **Client-1**, attempt to log in with the re-enabled account.
   - Confirm that you are able to log in successfully.

### Step 6: Observing Logs

1. **Observe Logs on the Domain Controller**:
   - On **DC-1**, open **Event Viewer** by pressing **Win + R**, typing `eventvwr.msc`, and pressing **Enter**.
   - Navigate to:  
     `Windows Logs > Security`
   - Here you can view various logs related to authentication and account lockouts.
     
     ![image](https://github.com/user-attachments/assets/ec4e21cf-aa53-49a7-bc04-18f984dbea7c)

     
2. **Observe Logs on the Client Machine**:
   - We are logged in as `lak.kut` and will not be able to access Event Viewer, so we'll Open Event Viewer and run as administrator `jane_admin`
   - As `jane_admin` 
   - On **Client-1**, open **Event Viewer** by pressing **Win + R**, typing `eventvwr.msc`, and pressing **Enter**.
   - Navigate to:  
     `Windows Logs > Security`
   - Review the logs related to login attempts and account status.
     ![image](https://github.com/user-attachments/assets/fafbfda0-458a-4c46-9671-c06565eb18db)


### Step 7: Clean-Up and Conclusion

- **Finish the Lab**: Ensure you’ve completed all the steps, including logging out and saving any necessary changes.
- **Do not delete the VMs** in Azure as we will use them for upcoming labs.
- **Save Money**: If you are done for the day, go to the **Azure Portal** and stop the **DC-1** and **Client-1** VMs to save costs. 
   - Navigate to **Virtual Machines**, click on each VM, and click **Stop**.

---

## Additional Resources:
- [How to Configure Account Lockout Threshold in Group Policy](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/account-lockout-threshold)
- [Precursor to Cybersecurity and Security Operations](https://joshmadakor.tech/cyber)

---
