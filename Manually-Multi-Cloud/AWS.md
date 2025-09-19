# 🔹 2. Backup Solution Using AWS EC2 Instances

### Step 1: Create Backup Vault
- Navigate to: **Console** → **AWS Backup** → **Backup vaults** → **Create vault**

- Set the following:
  - **Name:** `myBackupVault`
  - **Encryption:** Default AWS key or CMK

### Step 2: Create Backup Plan
- Go to **Backup Plans** → **Create**.
- Name: `DailyBackupPlan`
  
  **Add rule:**  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Name:** `DailyBackupRule`  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Frequency:** Daily @ 9:00 PM UTC  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Retention:** 30 days  
  &nbsp;&nbsp;&nbsp;&nbsp;• **Vault:** `myBackupVault`

### Step 3: Assign Resources

- Select `DailyBackupPlan` → **Assign resources**.

---

### Name: `BackupEC2Resources`  
**Type:** EC2 instance  
**Instance ID:** `i-0123456789abcdef0`

---
### Step 4: Run On-Demand Backup

- Navigate to: **Backup Plan** → **Create on-demand backup**
- Set the following:
  - **Resource type:** EC2 instance  
  - **ID:** `i-0123456789abcdef0`  
  - **Vault:** `myBackupVault`
  
- Monitor the progress under **Backup Jobs**

### Step 5: Automation (Snapshots)
- Handled automatically by AWS Backup Plans.
- Custom: Use Lambda + EventBridge if needed.

### Step 6: Restore EC2
- Go to Protected resources → Backup vault.
- Select backup → Restore.
- Choose restore options (new instance or original).

### Summary
**AWS Backup** → Vault + Plan + Assign Resources + (Optional) On-Demand Backup
