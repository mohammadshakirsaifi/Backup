# 🔹 3. Backup Solution Using GCP Compute Engine VMs

## Step 1: Create Cloud Storage Bucket
1. Go to: **Console** → **Cloud Storage** → **Buckets** → **Create**
2. Fill in the following:
   - **Name:** `my-backup-bucket`
   - **Location:** Regional (e.g., `asia-south1`)
   - **Storage Class:** Standard
---

## Step 2: Cloud Function for Snapshots
1. Go to: **Console** → **Cloud Functions** → **Create**
2. Provide the following:
   - **Name:** `createVMSnapshot`
   - **Region:** `asia-south1`
   - **Trigger:** HTTP
   - **Runtime:** Python 3.9

3. **Function Code:**

   ```python
   from googleapiclient import discovery
   from datetime import datetime

   PROJECT = 'your-gcp-project-id'
   ZONE = 'asia-south1-a'
   DISK_NAME = 'my-vm-instance-pd-standard'

   def create_snapshot(request):
       compute = discovery.build('compute', 'v1')
       snapshot_name = f"{DISK_NAME}-snapshot-{datetime.utcnow().strftime('%Y%m%d%H%M%S')}"
       snapshot_body = { 'name': snapshot_name }
       request = compute.disks().createSnapshot(
           project=PROJECT, zone=ZONE, disk=DISK_N_

### Step 3: Schedule with Cloud Scheduler
1. Go to: Console → Cloud Scheduler → Create job
2. Configure:
 - **Name:** daily-vm-snapshot-job
 - **Frequency:** 0 21 * * * (daily @ 9 PM UTC)
 - **Target:** HTTP → Trigger the Cloud Function

### Step 4: Native Snapshot Schedule (Simpler Option)
1. Go to: Console → Compute Engine → Snapshots → Snapshot schedules
2. Create a schedule:
  - **Name:** daily-snapshot-schedule
  - **Frequency:** Daily @ 9 PM IST (≈15:30 UTC)
  - **Retention:** 30 days

### Step 5: Restore VM from Snapshot
1. Go to: Console → Snapshots
2. Select the snapshot → Create Disk
3. Use the disk as:
 - Boot disk for a new VM, or
 - Attach to an existing VM

### ✅ Summary

#### GCP Backup Approaches:
  - Automated Snapshots via Cloud Functions + Scheduler
  - OR simpler Native Snapshot Schedules
  - Easy restore using created snapshots


