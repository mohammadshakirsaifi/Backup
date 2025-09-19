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
