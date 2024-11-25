
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
   - Use a previously created user account in Active Directory (you may have created this during a previous lab).

3. **Attempt to log in with the wrong password**:  
   - Try to log in to the **Client-1** machine using the selected user account.
   - Enter the wrong password **10 times** to simulate failed login attempts.

### Step 3: Configure Group Policy to Lock Out the Account After 5 Attempts

1. **Configure Account Lockout Threshold**:
   - On **DC-1**, open the **Group Policy Management Console**.
     - Press **Win + R**, type `gpmc.msc`, and press **Enter**.

2. **Edit the Default Domain Policy**:
   - In the Group Policy Management Console, navigate to **Forest > Domains > YourDomainName > Default Domain Policy**.
   - Right-click on **Default Domain Policy** and click **Edit**.

3. **Configure Account Lockout Settings**:
   - Navigate to:  
     `Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy`.
   - Double-click on **Account Lockout Threshold**.
   - Set the value to **5 invalid logon attempts**.
   - Click **OK**.

4. **Update Group Policy**:
   - Run `gpupdate /force` in the **Command Prompt** to apply the changes.

### Step 4: Test Account Lockout

1. **Attempt to log in with wrong credentials**:
   - On **Client-1**, attempt to log in using the same user account **6 times** with an incorrect password.

2. **Observe the account lockout**:
   - After 5 failed login attempts, the account should now be locked out.
   - Go back to **DC-1** and open **Active Directory Users and Computers**.
   - Locate the user account and observe that it is marked as **locked out**.

3. **Unlock the Account**:
   - In **Active Directory Users and Computers**, right-click on the locked-out user account and select **Unlock Account**.

4. **Reset the password**:
   - Right-click on the account again and select **Reset Password**.
   - Set a new password and click **OK**.

5. **Attempt to log in**:
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
