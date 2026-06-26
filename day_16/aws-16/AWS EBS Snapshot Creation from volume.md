# AWS EBS Snapshot Creation

## Task
Create a snapshot of existing EBS volume `xfusion-vol` in `us-east-1` region.

## Requirements
- Snapshot Name: `xfusion-vol-ss`
- Description: `xfusion Snapshot`
- Snapshot status should be `Completed`

## Steps (GUI)

1. Login to AWS Console.
2. Select region:

us-east-1 (N. Virginia)


3. Open:

EC2 → Elastic Block Store → Snapshots


4. Click:

Create snapshot


5. Select:

Resource type: Volume


6. Choose volume:

xfusion-vol


7. Enter description:

xfusion Snapshot


8. Click:

Create snapshot


9. Open created snapshot.

10. Add tag:

Key: Name
Value: xfusion-vol-ss


11. Wait until snapshot status becomes:


Completed


## Result

EBS snapshot `xfusion-vol-ss` created successfully from volume `xfusion-vol`.