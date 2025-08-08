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

✍️ *Author:* Your Name  
📅 *Date:* YYYY-MM-DD  
