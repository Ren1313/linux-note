# 復原 LVM 磁碟區 (LVM Volume Recovery)

當 LVM (Logical Volume Manager) 發生 Metadata 損毀、遺失，或者不小心誤刪 Volume Group (VG) / Logical Volume (LV) 時，通常可以透過系統自動備份的設定檔來嘗試復原。

LVM 建立或變更時，預設會將架構異動備份在 `/etc/lvm/archive/` 與 `/etc/lvm/backup/` 內。

## 1. 確認並尋找備份檔案

首先，檢查系統上的 LVM 備份檔案，找出你需要還原的時間點設定檔（VG 的名稱通常會包含在檔名中）：

```bash
sudo ls -l /etc/lvm/archive/
sudo ls -l /etc/lvm/backup/
```

你也可以直接查閱這些文字檔來確認裡面記錄的磁碟結構是否是你要救回的狀態。

## 2. 使用 `vgcfgrestore` 進行試運行 (Dry-run)

**強烈建議** 在真正還原之前，先加上 `--test` 參數來測試是否能成功解析與對應到正確的實體磁碟 (PV)：

```bash
sudo vgcfgrestore --test -f /etc/lvm/archive/<vg_name>_xxxxx.vg <vg_name>
```

## 3. 執行 VG 設定還原

如果測試步驟沒有錯誤，就可以開始進行真正的 Metadata 還原：

```bash
sudo vgcfgrestore -f /etc/lvm/archive/<vg_name>_xxxxx.vg <vg_name>
```

## 4. 重新掃描與啟用 Logical Volume

還原 VG 的架構設定後，內部的 LV 狀態預設可能是 `inactive`（未啟用），需要掃描並手動啟用它們：

```bash
# 掃描目前的 LVM 狀態
sudo vgscan
sudo lvscan

# 啟用指定的 VG（或是使用 sudo vgchange -ay 啟用所有找到的 VG）
sudo vgchange -ay <vg_name>
```

## 5. 檢查檔案系統並掛載 (Mount)

啟用成功後，檢查設備是否已經出現在系統中，並嘗試重新掛載：

```bash
# 檢查磁碟狀態列表
lsblk
sudo lvs

# (選擇性) 確認檔案系統無損，以 ext4/xfs 為例
# sudo fsck -y /dev/<vg_name>/<lv_name> 

# 將復原的 LV 掛載到指定目錄
sudo mount /dev/<vg_name>/<lv_name> /mnt
```

> ⚠️ **注意事項：** 
> * 透過 `vgcfgrestore` 主要是復原 LVM 的 **「配置資料 (Metadata)」**。
> * 如果底層的實體磁碟分區 (PV, Physical Volume) 已經被格式化 (mkfs) 或被磁區覆寫指令 (如 dd) 破壞了真實資料，單純還原 Metadata 是無法救回裡面的檔案的。
