1. Login to AWS Console

Use the provided console URL and credentials.

2. Open IAM
Search IAM
Open Identity and Access Management (IAM)
3. Create Role

Go to:

IAM → Roles

Click:

Create role
4. Select Trusted Entity

Choose:

Trusted entity type: AWS service

Use case:

EC2

Click:

Next
5. Attach Policy

Search for:

iampolicy_ammar

Select the checkbox next to iampolicy_ammar.

Click:

Next
6. Name the Role

Role name:

iamrole_ammar

Click:

Create role
7. Verify

Go to:

IAM → Roles

Search:

iamrole_ammar

Open the role and verify:

Trusted entity: EC2
Attached policy: iampolicy_ammar

✅ Task completed.