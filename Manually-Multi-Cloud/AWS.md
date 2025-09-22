# 🔹🔹 Implement a Backup Solution Using AWS EC2 Instance 🔹🔹

### Step 1: Create Backup Vault

 
## Step 1: Create a Backup Vault

1. Sign in to the AWS Management Console.
2. Navigate to: **Console** → **AWS Backup** → **Backup vaults** → **Create vault**
3.  Configure the vault settings:
   - **Vault name:** myBackupVault
   - **Encryption key:** Use the default AWS managed key or select/create a Customer Managed Key (CMK)
4. Click Create vault to save.

### Summary: Prepare AWS Backup Vault
- **Service:** AWS Backup
- **Vault name:** myBackupVault
- **Encryption:** Default AWS KMS key or a CMK

![](./Photos/aws/aws-1.jpeg)
### Step 2: Create Backup Plan

1. In the AWS Backup console, go to Backup Plans → Create backup plan.
2. Choose Build a new plan or Start with a template, depending on your preference.
3. Enter a name for your backup plan, e.g., DailyBackupPlan.
4. Define the backup rule with the following settings:

- **Rule name:** DailyBackupRule
- **Backup frequency:** Daily at 9:00 PM UTC (adjust if needed)
- **Backup window:** e.g., 1 hour
- **Retention period:** 30 days
- **Backup vault:** Select myBackupVault
5. Click Create plan (or Save plan) to finalize.

### Summary:  Create Backup Plan
- Go to **Backup Plans** → **Create**.
- Name: `DailyBackupPlan`
  **Add rule:**  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Rule Name:** `DailyBackupRule`  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Frequency:** Daily @ 9:00 PM UTC  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Retention:** 30 days  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Vault:** `myBackupVault`
  
![](./Photos/aws/aws-2.jpeg)

## Step 3: Assign Resources to Backup Plan
1. In the AWS Backup console, go to Backup Plans and select your plan (e.g., `DailyBackupPlan`).
2. Click **Assign resources**.
3. Enter an assignment name, e.g., BackupEC2Resources.
4. Select Resource type: EC2 instance.
5. Specify the EC2 instance ID(s) to back up, e.g., i-0123456789abcdef0.
6. Click Assign resources to complete the process.

Once assigned, AWS Backup will automatically create snapshots of the selected EC2 instance's attached EBS volumes according to the defined schedule and retention settings in the backup plan.

![](./Photos/aws/aws-3.jpeg)
### Summary: Assign Resources

- Select DailyBackupPlan → Assign resources
- **Name:** `BackupEC2Resources`
- **Type:** EC2 instance
- **Instance ID:** `i-0123456789abcdef0`
---

## Step 4: Run an On-Demand Backup Job
1. Navigate to: **Backup Plan** in AWS Backup Console → **Create on-demand backup**
2. Click Create OnDemand backup .
3. Specify the following settings:
  - **Resource type:** EC2 instance  
  - **ID:** `i-0123456789abcdef0`  
  - **Vault:** `myBackupVault`
4. Click Create backup job to initiate an immediate snapshot.
5. Monitor the backup job progress under the **Backup Jobs** section.  

![](./Photos/aws/aws-4.jpeg)
![](./Photos/aws/aws-5.jpeg)

## Step 5: Automate Snapshot Creation Using AWS Backup (Handled by Backup Plan)
- The AWS Backup service automatically creates snapshots and backup copies based on assigned backup plans—no additional automation tools are needed.
- For custom automation scenarios, you can use AWS Lambda with Amazon EventBridge (formerly CloudWatch Events) to trigger EC2 snapshots. However, the native AWS Backup service is recommended for simplicity and maintainability.

AWS Backup manages daily snapshot creation according to the schedules defined in your backup plan:
- When you assign an EC2 instance to a backup plan, AWS Backup automatically creates snapshots of the attached EBS volumes based on your specified schedule and retention settings.
- This removes the need for custom automation scripts or manual triggers using Lambda and EventBridge.
### Summary: Automation (Snapshots)
     Recommended: Handled automatically by AWS Backup plans
     Optional (Custom): Use AWS Lambda + EventBridge if advanced or non-standard logic is required

## Step 6: Restore from Backup
1. Navigate to Protected Resources or Backup Vault in the AWS Backup console.
2. Select the backup you want to restore.
3. Click Restore.
4. Choose your restore options, such as restoring to the original EC2 instance or launching a new one.
5. Confirm the restore action and monitor the progress.
 
 ###  Summary: Restore EC2
      Go to Protected Resources → Backup Vault
      Select the backup → Click Restore
      Choose restore options (original instance or new EC2 instance)
      Confirm and monitor the restore process

![](./Photos/aws/aws-6.jpeg)
![](./Photos/aws/aws-7.jpeg)

### Summary
**AWS Backup** → Vault + Plan + Assign Resources + (Optional) On-Demand Backup
