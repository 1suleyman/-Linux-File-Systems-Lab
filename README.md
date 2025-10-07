# 🐧 Linux File Systems Lab

In this lab, I learned how to **inspect existing file systems, create new file systems, mount them, and make mounts persistent across reboots**.

---

## 📋 Lab Overview

**Goal:**

* Identify disks with file systems
* Check file system types using `blkid`
* Create and mount ext4 file systems
* Make mount points persistent across reboots

**Learning Outcomes:**

* Use `df -h` and `lsblk` to inspect disk mounts
* Determine the type of file system on a disk using `blkid`
* Create ext4 file systems with `mkfs.ext4`
* Mount file systems at specific mount points
* Edit `/etc/fstab` to persist mounts across reboots

---

## 🛠 Step-by-Step Journey

### Step 1: Identify File Systems

**Command:**

```bash
df -h /dev/vdb
df -h /dev/vdc
```

* Checks whether the disks have a file system created.
* Example output: `/dev/vdc` has a file system, `/dev/vdb` shows `udev` (no user data file system).

---

### Step 2: Determine File System Type

**Command:**

```bash
sudo blkid /dev/vdc
```

* Example output shows the type: `ext2`.

---

### Step 3: Create a New File System

**Task:** Create an **ext4** file system on `/dev/vdb` and mount it at `/mnt/data`.

**Commands:**

```bash
sudo mkfs.ext4 /dev/vdb
sudo mkdir -p /mnt/data
sudo mount /dev/vdb /mnt/data
```

* `mkfs.ext4` formats the disk
* `mkdir -p` ensures the mount directory exists
* `mount` mounts the disk to the directory

---

### Step 4: Verify the Mount

```bash
df -h /mnt/data
```

* Confirms the new file system is mounted correctly

---

### Step 5: Make Mount Persistent Across Reboots

**Edit `/etc/fstab`**:

```bash
sudo vi /etc/fstab
```

* Add the following line:

```
/dev/vdb  /mnt/data  ext4  rw  0  0
```

* `rw` – read/write access
* `0 0` – dump and fsck options (both set to zero)

**Save and exit:** `ESC :wq!`

---

## ✅ Key Commands Summary

| Task                              | Command / Notes                 |
| --------------------------------- | ------------------------------- |
| Check if a disk has a file system | `df -h /dev/vdb`                |
| List mounted file systems         | `lsblk`                         |
| Determine file system type        | `sudo blkid /dev/vdc`           |
| Create ext4 file system           | `sudo mkfs.ext4 /dev/vdb`       |
| Create mount point                | `sudo mkdir -p /mnt/data`       |
| Mount a disk                      | `sudo mount /dev/vdb /mnt/data` |
| Verify mount                      | `df -h /mnt/data`               |
| Persist mount on reboot           | Add entry to `/etc/fstab`       |

---

## 💡 Notes / Tips

* Always use `sudo` for disk operations.
* `udev` represents virtual devices and **does not have a user file system**.
* Without `/etc/fstab`, mounts will not persist after reboot.
* Use `blkid` to identify the **file system type** before mounting.

---

## 📌 Lab Summary

| Step                                  | Status | Key Takeaways                               |
| ------------------------------------- | ------ | ------------------------------------------- |
| Check existing file systems           | ✅      | `/dev/vdc` has ext2, `/dev/vdb` unformatted |
| Determine file system type            | ✅      | Used `blkid` → ext2                         |
| Create ext4 file system on `/dev/vdb` | ✅      | Successfully formatted                      |
| Mount file system                     | ✅      | Mounted at `/mnt/data`                      |
| Make mount persistent                 | ✅      | Added to `/etc/fstab`                       |

---

## ✅ References

* [Linux `df` command](https://man7.org/linux/man-pages/man1/df.1.html)
* [Linux `blkid` command](https://man7.org/linux/man-pages/man8/blkid.8.html)
* [Linux `mkfs.ext4` command](https://man7.org/linux/man-pages/man8/mkfs.ext4.8.html)
* [Persisting mounts with `/etc/fstab`](https://wiki.archlinux.org/title/Fstab)
