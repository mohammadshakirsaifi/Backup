🔹 1. Backup Solution Using Azure VMs
Step 1: Create a Recovery Services Vault

Sign in to Azure Portal.

Go to Backup Center → + Add → choose Recovery Services Vault.

Fill details:

Subscription: MySubscription

Resource group: BackupRG

Vault name: myBackupVault

Region: Same as VM (e.g., Central India)

Click Review + Create → Create.

Step 2: Configure Backup Policy

In the vault, go to Backup Policies → + Add.

Define schedule:

Frequency: Daily @ 9:00 PM

Retention: 30 days

(Optional) Add weekly/monthly/yearly rules.

Save as DailyBackupPolicy.

Step 3: Enable Backup for VM

In vault → Backup.

Workload location: Azure

What to back up?: Virtual Machine

Select vault: myBackupVault.

Click + Backup, select VM (e.g., myAppVM).

Assign policy: DailyBackupPolicy.

Click Enable Backup.

Azure installs backup extension automatically.

Step 4: Run On-Demand Backup

Vault → Backup Items → Azure Virtual Machine.

Select VM (e.g., myAppVM).

Click Backup Now.

Monitor status under Backup Jobs.

Step 5: Automate Snapshot with Logic Apps

Go to Logic Apps → + Create.

Name: VMSnapshotScheduler

RG: BackupRG

Location: Central India

Plan: Consumption

In Designer:

Trigger: Recurrence → daily @ 9 PM

Action: Azure Resource Manager → Create or Update Resource

Resource type: Microsoft.Compute/snapshots

Name: concat(variables('VMName'),'-snapshot-',utcNow('yyyyMMddHHmmss'))

Properties:

{
  "creationData": {
    "createOption": "Copy",
    "sourceResourceId": "<Your OS disk Resource ID>"
  },
  "sku": { "name": "Standard_LRS" }
}


Save & enable Logic App.

Step 6: Restore from Backup or Snapshot

Vault → Backup Items → VM → Restore VM.

Select restore point.

Choose restore to original or new VM.

Confirm restore.

✅ Summary
Azure → Recovery Services Vault + Policy + Logic App (optional).
