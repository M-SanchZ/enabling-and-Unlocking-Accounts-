
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
   - If you can not locate the account user's name, right-click on 
     

5. **Reset the password**:
   - Right-click on the account again and select **Reset Password**.
   - Set a new password and click **OK**.

6. **Attempt to log in**:
   - On **Client-1**, log in with the new password and confirm that you can access the account successfully.

### Step 5: Enabling and Disabling Accounts

1. **Disable the Account**:
   - On **DC-1**, go to **Active Directory Users and Computers**.
   - Right-click on the same user account and select **Disable Account**.

2. **Attempt to log in**:
   - On **Client-1**, attempt to log in with the disabled account.
   - You should see an error message that indicates the account is disabled.

3. **Re-enable the Account**:
   - On **DC-1**, right-click on the disabled user account and select **Enable Account**.

4. **Attempt to log in**:
   - On **Client-1**, attempt to log in with the re-enabled account.
   - Confirm that you are able to log in successfully.

### Step 6: Observing Logs

1. **Observe Logs on the Domain Controller**:
   - On **DC-1**, open **Event Viewer** by pressing **Win + R**, typing `eventvwr.msc`, and pressing **Enter**.
   - Navigate to:  
     `Windows Logs > Security`
   - Here you can view various logs related to authentication and account lockouts.
     
2. **Observe Logs on the Client Machine**:
   - On **Client-1**, open **Event Viewer** by pressing **Win + R**, typing `eventvwr.msc`, and pressing **Enter**.
   - Navigate to:  
     `Windows Logs > Security`
   - Review the logs related to login attempts and account status.

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
