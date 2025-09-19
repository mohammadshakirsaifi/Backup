# 🔹 1. Backup Solution Using Azure VMs

## Step 1: Create a Recovery Services Vault

1. Sign in to the **Azure Portal**.
2. Navigate to: **Backup Center** → **+ Add** → Choose **Recovery Services Vault**.
3. Fill in the following details:
   - **Subscription:** `MySubscription`
   - **Resource Group:** `BackupRG`
   - **Vault Name:** `myBackupVault`
   - **Region:** Same as VM (e.g., `Central India`)
4. Click **Review + Create** → **Create**.

---

## Step 2: Configure Backup Policy

1. In the vault, go to: **Backup Policies** → **+ Add**.
2. Define the schedule:
   - **Frequency:** Daily @ 9:00 PM
   - **Retention:** 30 days
   - *(Optional)* Add weekly/monthly/yearly rules.
3. Save the policy as: `DailyBackupPolicy`.

---

## Step 3: Enable Backup for VM

1. In the vault, go to: **Backup**
2. Configure the following:
   - **Workload Location:** Azure  
   - **What to back up?:** Virtual Machine  
   - **Select Vault:** `myBackupVault`
3. Click **+ Backup** and select the VM (e.g., `myAppVM`).
4. Assign the policy: `DailyBackupPolicy`.
5. Click **Enable Backup**.

> ℹ️ Azure installs the backup extension automatically.

---

## Step 4: Run On-Demand Backup

1. Go to: **Vault** → **Backup Items** → **Azure Virtual Machine**
2. Select the VM (e.g., `myAppVM`).
3. Click **Backup Now**.
4. Monitor status under **Backup Jobs**.

---

## Step 5: Automate Snapshot with Logic Apps

1. Go to: **Logic Apps** → **+ Create**
2. Fill the following:
   - **Name:** `VMSnapshotScheduler`
   - **Resource Group:** `BackupRG`
   - **Location:** `Central India`
   - **Plan:** `Consumption`

### In the Logic App Designer:

- **Trigger:**  
  `Recurrence` → Daily @ 9:00 PM

- **Action:**  
  `Azure Resource Manager` → **Create or Update Resource**

#### Resource Details:
- **Resource Type:** `Microsoft.Compute/snapshots`
- **Name:**
  ```text
  concat(variables('VMName'), '-snapshot-', utcNow('yyyyMMddHHmmss'))
  ```
- Properties:
 ```text
  {
  "creationData": {
    "createOption": "Copy",
    "sourceResourceId": "<Your OS disk Resource ID>"
  },
  "sku": {
    "name": "Standard_LRS"
  }
}
```

### Step 6: Restore from Backup or Snapshot

- Go to: Vault → Backup Items → VM → Restore VM
- Select a restore point
#### Choose to restore to:
- Original VM, or
- New VM
- Confirm restore.

### ✅ Summary
Azure Backup Flow:

- Recovery Services Vault
- Backup Policy
- (Optional) Logic App for snapshots
- On-demand or scheduled backups
- Easy restore from portal
