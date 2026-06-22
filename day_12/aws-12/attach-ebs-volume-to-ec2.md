# Attach Existing EBS Volume to EC2 Instance (AWS)

## Problem Statement

The DevOps team has an existing EC2 instance and an EBS volume in AWS.

Requirement:

-   Attach the existing volume to the existing EC2 instance.
-   Volume name: `datacenter-volume`
-   Instance name: `datacenter-ec2`
-   Region: `us-east-1`
-   Device name should be: `/dev/sdb`

------------------------------------------------------------------------

## Solution (AWS Console GUI Steps)

### Step 1: Login to AWS Console

Open AWS Console and login.

Make sure region is:

    US East (N. Virginia) - us-east-1

------------------------------------------------------------------------

### Step 2: Open EC2 Dashboard

1.  Search EC2 from AWS services.
2.  Open EC2 Dashboard.

------------------------------------------------------------------------

### Step 3: Go to Volumes

From left menu:

    Elastic Block Store
            |
            └── Volumes

------------------------------------------------------------------------

### Step 4: Select Existing Volume

Find:

    datacenter-volume

Select the volume.

------------------------------------------------------------------------

### Step 5: Attach Volume

Click:

    Actions → Attach volume

------------------------------------------------------------------------

### Step 6: Provide Attach Details

Instance:

    datacenter-ec2

Device name:

    /dev/sdb

Click:

    Attach volume

------------------------------------------------------------------------

## Verification

Go to:

    EC2 → Volumes

Expected status:

    State: In-use

Attachment:

    Instance: datacenter-ec2

    Device:
    /dev/sdb

------------------------------------------------------------------------

## Optional Linux Verification

Login to EC2 server and run:

``` bash
lsblk
```

Expected:

    xvdb

Note:

AWS may display `/dev/sdb` as `/dev/xvdb` inside Linux.

------------------------------------------------------------------------

## Result

EBS volume `datacenter-volume` successfully attached to EC2 instance
`datacenter-ec2` with device name `/dev/sdb`.
