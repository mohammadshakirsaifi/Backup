🔹 3. Backup Solution Using GCP Compute Engine VMs
Step 1: Create Cloud Storage Bucket

Console → Cloud Storage → Buckets → Create.

Name: my-backup-bucket

Location: Regional (e.g., asia-south1)

Storage class: Standard

Step 2: Cloud Function for Snapshots

Console → Cloud Functions → Create.

Name: createVMSnapshot

Region: asia-south1

Trigger: HTTP

Runtime: Python 3.9

Code:

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
        project=PROJECT, zone=ZONE, disk=DISK_NAME, body=snapshot_body
    )
    response = request.execute()
    return f"Snapshot {snapshot_name} creation started."

Step 3: Schedule with Cloud Scheduler

Console → Cloud Scheduler → Create job.

Name: daily-vm-snapshot-job

Frequency: 0 21 * * * (daily 9 PM UTC)

Target: HTTP → Cloud Function trigger

Step 4: Native Snapshot Schedule (Simpler Option)

Console → Compute Engine → Snapshots → Snapshot schedules.

Create schedule:

Name: daily-snapshot-schedule

Frequency: Daily @ 9 PM IST (≈15:30 UTC)

Retention: 30 days

Step 5: Restore VM from Snapshot

Console → Snapshots → Select snapshot → Create disk.

Use disk as:

Boot disk for new VM, OR

Attach to existing VM.

✅ Summary
GCP → Snapshots via Cloud Scheduler/Functions or native snapshot schedules.