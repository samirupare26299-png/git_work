# Attach Existing Network Interface (ENI) to a Running EC2 Instance — AWS Console

**Task:** Attach a network interface to an EC2 instance and confirm its status shows as attached.
**Method:** AWS Management Console (GUI)
**Instance State:** Running (no stop/start required)
**Interface Used:** Existing ENI (not newly created)

---

## 1. Locate the Existing Network Interface

1. Open the **AWS Console** → **EC2** → **Network Interfaces** (under *Network & Security* in the left sidebar).
2. Identified the target ENI in the list — confirmed its **Status** was `available` (i.e., not already attached to another instance).
3. Noted the **Network Interface ID** (`eni-xxxxxxxx`) and confirmed it was in the **same Availability Zone** as the target EC2 instance (a hard AWS requirement — an ENI can only attach to an instance in its own AZ).

---

## 2. Attach the Network Interface to the Instance

1. With the ENI selected, clicked **Actions → Attach**.
2. In the attach dialog, selected the target **EC2 instance** (instance was left **running** — attaching a secondary ENI does not require stopping the instance).
3. Confirmed the attachment by clicking **Attach**.

> **Note:** The *primary* network interface (`eth0`) cannot be detached/reattached while an instance is running. This task involved a **secondary** ENI, which can be hot-attached to a running instance without downtime.

---

## 3. Verify the Attachment

1. Navigated to **EC2 → Instances**, selected the target instance.
2. Opened the **Networking** tab on the instance details page.
3. Confirmed the newly attached ENI now appeared in the **Network interfaces** list for that instance, alongside the primary interface.
4. Status reflected the interface as attached to the instance (visible directly in the instance's Networking tab rather than needing to recheck the Network Interfaces page separately).

---

## Summary

| Step | Action | Result |
|------|--------|--------|
| 1 | Located existing ENI in EC2 console | Confirmed `available` status, correct AZ |
| 2 | Attached ENI to running instance via Actions → Attach | Attachment accepted, no instance restart needed |
| 3 | Verified via instance's Networking tab | ENI listed under the instance's network interfaces — attachment confirmed |

## Key Notes / Gotchas

- ENI and EC2 instance **must be in the same Availability Zone** — attachment will fail otherwise.
- Secondary ENIs can be attached/detached on a **running** instance; only the primary interface requires the instance to be stopped.
- Verification can be done from either:
  - **EC2 → Network Interfaces** page (check the *Status* and *Instance ID* columns), or
  - **EC2 → Instances → [instance] → Networking tab** (used in this task).
