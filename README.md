# WORKING WITH EBS

## Aim

To create and configure an Amazon Elastic Block Store (EBS) volume, attach and mount it to an Amazon EC2 instance, create a snapshot backup, and restore the snapshot to a new EBS volume.

---

## Algorithm / Steps

1. Create a new Amazon EBS volume with a size of 1 GiB.
2. Select the same Availability Zone as the EC2 instance.
3. Attach the EBS volume to the EC2 instance using `/dev/sdb`.
4. Connect to the EC2 instance using AWS Systems Manager Session Manager.
5. Check the available storage using `df -h`.
6. Create an `ext3` file system on the EBS volume.
7. Create the `/mnt/data-store` directory.
8. Mount the EBS volume to `/mnt/data-store`.
9. Configure `/etc/fstab` for automatic mounting.
10. Verify that the EBS volume is successfully mounted.
11. Create `file.txt` inside the mounted EBS volume.
12. Verify the contents of the created file.
13. Create an EBS snapshot named `My Snapshot`.
14. Delete `file.txt` from the original EBS volume.
15. Create a new EBS volume from the snapshot.
16. Attach the restored volume to the EC2 instance using `/dev/sdc`.
17. Create the `/mnt/data-store2` directory.
18. Mount the restored volume to `/mnt/data-store2`.
19. Verify that `file.txt` has been successfully restored.

---

## Program

### 1. Check Available Storage

```bash
df -h
```

### 2. Create an ext3 File System

```bash
sudo mkfs -t ext3 /dev/sdb
```

### 3. Create a Mount Directory

```bash
sudo mkdir /mnt/data-store
```

### 4. Mount the EBS Volume

```bash
sudo mount /dev/sdb /mnt/data-store
```

### 5. Configure Automatic Mounting

```bash
echo "/dev/sdb   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
```

### 6. View the File System Configuration

```bash
cat /etc/fstab
```

### 7. Verify the Mounted Volume

```bash
df -h
```

### 8. Create a File in the EBS Volume

```bash
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
```

### 9. Read the File

```bash
cat /mnt/data-store/file.txt
```

### 10. Delete the File

```bash
sudo rm /mnt/data-store/file.txt
```

### 11. Verify File Deletion

```bash
ls /mnt/data-store/
```

### 12. Create a Mount Directory for the Restored Volume

```bash
sudo mkdir /mnt/data-store2
```

### 13. Mount the Restored EBS Volume

```bash
sudo mount /dev/sdc /mnt/data-store2
```

### 14. Verify Snapshot Restoration

```bash
ls /mnt/data-store2/
```

Expected output:

```text
file.txt
```

---

## Outputs

### Output 1: EBS Volume Created

The AWS EC2 Volumes page shows the newly created `My Volume` EBS volume with a size of 1 GiB.

<img width="1717" height="913" alt="Screenshot 2026-07-28 084035" src="https://github.com/user-attachments/assets/881c6d48-53ba-4f39-bbe7-13cb99a53025" />


---

### Output 2: EBS Volume Attached to EC2 Instance

The `My Volume` EBS volume is successfully attached to the `Lab` EC2 instance and is in the `In-use` state.

<img width="1721" height="920" alt="Screenshot 2026-07-28 084314" src="https://github.com/user-attachments/assets/e5f77421-f1a7-4986-896c-389d235879ff" />


---

### Output 3: EBS Volume Mounted Successfully

The `df -h` command displays the mounted EBS volume at `/mnt/data-store`.

<img width="1917" height="918" alt="Screenshot 2026-07-28 085343" src="https://github.com/user-attachments/assets/7cac8996-548f-47be-b754-37fa3e015601" />


---

### Output 4: File Created and Verified

The file `file.txt` is successfully created inside the EBS volume and the stored text is displayed.

```text
some text has been written
```

<img width="1916" height="907" alt="Screenshot 2026-07-28 085728" src="https://github.com/user-attachments/assets/b97d4602-1c31-4e09-8546-2952e11d5d14" />



---

### Output 5: EBS Snapshot Created

The AWS EC2 Snapshots page shows `My Snapshot` with the snapshot creation completed successfully.

<img width="1917" height="932" alt="Screenshot 2026-07-28 085615" src="https://github.com/user-attachments/assets/abb516f5-4e7c-4858-8c3b-6aa8bf6acc93" />
<img width="1917" height="920" alt="Screenshot 2026-07-28 085649" src="https://github.com/user-attachments/assets/212a5e6f-83e0-492e-802d-9f5bca98663b" />


---

### Output 6: Snapshot Restored Successfully

The snapshot is restored to a new EBS volume named `Restored Volume`. After attaching and mounting the restored volume, the deleted `file.txt` is successfully recovered.

```text
file.txt
```
<img width="1917" height="917" alt="Screenshot 2026-07-28 090251" src="https://github.com/user-attachments/assets/4c419e9b-c72d-40e7-bb59-7dcf56943d9f" />




---

## Result

Thus, an Amazon EBS volume was successfully created and attached to an Amazon EC2 instance. The volume was formatted with an `ext3` file system, mounted, and used for storing data. An EBS snapshot was successfully created as a backup, and a new EBS volume was restored from the snapshot. The previously deleted `file.txt` was successfully recovered, demonstrating the backup and restore functionality of Amazon EBS.

