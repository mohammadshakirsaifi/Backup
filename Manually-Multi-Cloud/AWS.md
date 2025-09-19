# 🔹 2. Backup Solution Using AWS EC2 Instances
### Step 1: Create Backup Vault
- Console → AWS Backup → Backup vaults → Create vault.
- Name: myBackupVault
- Encryption: Default AWS key or CMK

### Step 2: Create Backup Plan
- Go to Backup Plans → Create.
- Name: DailyBackupPlan.
Add rule:
Name: DailyBackupRule
Frequency: Daily @ 9:00 PM UTC
Retention: 30 days
Vault: myBackupVault

### Step 3: Assign Resources

- Select DailyBackupPlan → Assign resources.
Name: BackupEC2Resources.
Type: EC2 instance.
Instance ID: i-0123456789abcdef0.

### Step 4: Run On-Demand Backup

- Backup Plan → Create on-demand backup.
- Resource type: EC2 instance
ID: i-0123456789abcdef0
Vault: myBackupVault
Monitor under Backup Jobs.

### Step 5: Automation (Snapshots)

- Handled automatically by AWS Backup Plans.
- Custom: Use Lambda + EventBridge if needed.

### Step 6: Restore EC2

- Go to Protected resources → Backup vault.
- Select backup → Restore.
- Choose restore options (new instance or original).


### Summary
- AWS → AWS Backup (Vault + Plan + Assign).
