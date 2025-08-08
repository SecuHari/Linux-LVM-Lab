# 🚀 LVM Setup on VirtualBox (RHEL Linux)

[![Made with ❤️ in Linux](https://img.shields.io/badge/Made%20with-Linux-blue?style=for-the-badge&logo=linux)]()
[![VirtualBox](https://img.shields.io/badge/VirtualBox-Setup-blue?style=for-the-badge&logo=virtualbox)]()

---

## 📋 Table of Contents
- [About This Task](#about-this-task)
- [Step 1 – Add New Hard Disk in VirtualBox](#step-1--add-new-hard-disk-in-virtualbox)
- [📸 Screenshots & GIF](#-screenshots--gif)
- [Next Steps](#next-steps)

---

## 📖 About This Task
This is the **first step** in setting up **LVM** on a RHEL Linux VM.  
Here, we will add a **new virtual hard disk** in VirtualBox which will later be used to create a Physical Volume (PV) for LVM.

---

## 🛠️ Step 1 – Add New Hard Disk in VirtualBox

1. **Open VirtualBox**  
   Launch VirtualBox on your host system.

2. **Select Your RHEL VM**  
   Click on the VM where you want to add the disk.

3. **Go to Settings**  
   Click the ⚙️ **Settings** icon or right-click → **Settings**.

4. **Open the Storage Tab**  
   From the left panel, select **Storage**.

5. **Select Controller: SATA**  
   Under **Controller: SATA**, click the small **Add Hard Disk** icon (💿 + ➕).

6. **Choose “Add New Attachment”**  
   From the menu, select **Hard Disk**.

7. **Create a New Hard Disk**  
   Click **Create New Disk** → Click **Next**.

8. **Specify Disk Size**  
   Enter the desired size (e.g., `2 GB`) → Click **Create**.

---

## 📸 Screenshots & GIF

> 💡 Add your screenshots and GIF here

1. **VM Storage Settings**  
   ![Storage Settings](images/1.png)

2. **Disk Creation Wizard**  
   ![Disk Creation Wizard](images/2.png)

3. **Animated GIF of the Process**  
   ![Add Disk Animation](3.png)
   ![Add Disk Animation](4.png)
   ![Add Disk Animation](5.png)
   ![Add Disk Animation](6.png)
   ![Add Disk Animation](7.png)
   ![Add Disk Animation](8.png)
---

## ⏭️ Next Steps
- Detect the newly added disk inside RHEL (`lsblk`)
- Create **Physical Volume → Volume Group → Logical Volume**
- Create a filesystem and mount it

---



# Partition Creation Using `fdisk`

This document describes the steps to create a partition using the `fdisk` utility in Linux.

## Step 1: Check the Disk
```bash
fdisk /dev/sdb
```
**Output:**
```
Welcome to fdisk (util-linux 2.40.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0x34cebb40.
```
![Screenshot Step](9.png)

---

## Step : View Help Menu
Type `m` inside `fdisk` to see the available commands and their descriptions.
```bash
Command (m for help): m
```
This displays the full help menu for `fdisk`.

![Screenshot Step](10.png)

---

## Step : Create a New Partition
Since we need a **primary** partition, type `n` and select the **primary** option when prompted.
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-4194303, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-4194303, default 4194303):
```
This creates a new primary partition of size **2 GiB**.

![Screenshot Step](11.png)

---

## Step : Change Partition Type to LVM
By default, the partition type is `Linux`. To change it to **Linux LVM**, press `t`:
```bash
Command (m for help): t
Selected partition 1
Hex code or alias (type L to list all):
```
Type `L` to list all types and find **Linux LVM** (`8e`).
```bash
Hex code or alias (type L to list all): 8e
Changed type of partition 'Linux' to 'Linux LVM'.
```

![Screenshot Step](12.png)

---

## Step : Save and Exit
Type `w` to write the changes to disk and exit:
```bash
Command (m for help): w
```

![Screenshot Step](13.png)

---

## Step : Verify the Partition
Run:
```bash
fdisk -l
```
**Output:**
```
Disk /dev/sdb: 2 GiB, 2147483648 bytes, 4194304 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xe635c378

Device     Boot Start     End Sectors Size Id Type
/dev/sdb1        2048 4194303 4192256   2G 8e Linux LVM
```

![Screenshot Step](14.png)

---

**✅ Summary:**  
We successfully created a **2 GiB primary partition** on `/dev/sdb`, changed its type to **Linux LVM**, and verified it.

# Creating a Physical Volume (PV) and Volume Group (VG) in LVM

This guide describes how to convert an existing partition into a Physical Volume (PV) and then create a Volume Group (VG) for use with LVM.

---

## Step 1: Create Physical Volume (PV)
Once the partition (`/dev/sdb1`) is ready, initialize it as a Physical Volume so that it can be used by LVM as storage capacity.

**Command:**
```bash
pvcreate /dev/sdb1
```
**Output:**
```
Physical volume "/dev/sdb1" successfully created.
```
![Screenshot](15.png)

---

## Step 2: Display Physical Volume Details
To check details of the created Physical Volume:
```bash
pvdisplay /dev/sdb1
```
**Example Output:**
```
"/dev/sdb1" is a new physical volume of "<2.00 GiB"
--- NEW Physical volume ---
PV Name               /dev/sdb1
VG Name               
PV Size               <2.00 GiB
Allocatable           NO
PE Size               0   
Total PE              0
Free PE               0
Allocated PE          0
PV UUID               GvYnc7-CcoK-ldFD-UOZT-MC0N-YiOm-TSdfGb
```
![Screenshot Step](16.png)

---

## Step 3: Create a Volume Group (VG)
Create a new Volume Group named `vgapps` using the Physical Volume `/dev/sdb1`.

**Command:**
```bash
vgcreate vgapps /dev/sdb1
```
**Output:**
```
Volume group "vgapps" successfully created
```
![Screenshot Step](17.png)

---

## Step 4: Display Volume Group Details
To verify and view details of the newly created Volume Group:
```bash
vgdisplay vgapps
```
**Example Output:**
```
--- Volume group ---
VG Name               vgapps
System ID             
Format                lvm2
Metadata Areas        1
Metadata Sequence No  1
VG Access             read/write
VG Status             resizable
MAX LV                0
Cur LV                0
Open LV               0
Max PV                0
Cur PV                1
Act PV                1
VG Size               <2.00 GiB
PE Size               4.00 MiB
Total PE              511
Alloc PE / Size       0 / 0   
Free  PE / Size       511 / <2.00 GiB
VG UUID               Qs9FVW-Q3Z9-cbWw-vLeA-f43X-hxl3-2Pn7QW
```
![Screenshot Step](18.png)

---

**✅ Summary:**  
- Converted `/dev/sdb1` into a Physical Volume.  
- Created a Volume Group named `vgapps`.  
- Verified details of both PV and VG using `pvdisplay` and `vgdisplay`.


