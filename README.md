# Linux 筆記與教學 (Linux Notes & Tutorials)

這是一個用來記錄 Linux 系統管理、除錯與實用操作的筆記空間。這裡將作為總目錄（首頁），整理各種實用的 Linux 指令與教學。

## 目錄

* [系統管理](#系統管理)
* [儲存與磁碟管理](#儲存與磁碟管理)
  * [復原 LVM 磁碟區 (LVM Volume Recovery)](./docs/lvm-recovery.md)
  * [從其他機器復原錯誤操作的 LVM (LVM Recovery for Misoperation)](./docs/lvm-recovery-for-Misoperation.md)
* [網路與安全性](#網路與安全性)
* [疑難排解](#疑難排解)

---

## 儲存與磁碟管理

* [📂 復原 LVM 磁碟區 (LVM Volume Recovery)](./docs/lvm-recovery.md) - 當 LVM Metadata 損毀或遺失時，透過系統自動備份檔進行復原的操作步驟。
* [📂 從其他機器復原錯誤操作的 LVM (LVM Recovery for Misoperation)](./docs/lvm-recovery-for-Misoperation.md) - 當 `fdisk` 操作不當導致 LVM metadata 損毀時，藉由相同組態的機器備份檔進行修復的步驟。
