# Fix MyAsus in WinRE

* Ensure that BitLocker is turned off
* Download [DiskGenius](https://www.diskgenius.com/download.php)
* Ensure you have RECOVERY, RESTORE, MYASUS partition (if you don't have, [try this](#restore-recovery-restore-myasus-partition))
   * RECOVERY partition should have `Recovery/WindowsRE/ReAgent.xml`
   * RESTORE partition should have `Recovery/RecoveryImage/ASUS.swm`
   * MYASUS partition should have `AsusWinRE/AsusWinRE.idx`
     <img width="902" alt="Screenshot 2024-06-16 123050-min" src="https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/94beb1de-0168-4ce1-8e3a-a0e7ac78e007">
   * In some cases the name of partition might be different or multiple partitions exist in that case explore all partition through DiskGenius and look for the files mentioned above
* Assign proper label to following partitions (case sensitive)
   * To set label, in DiskGenius select the partition you want to rename right click --> set volume name --> type the proper name in the textbox --> hit ok
     <img width="883" alt="Screenshot 2024-06-16 123304-min" src="https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/3501f269-4ae3-42c4-837c-c749e5b3dadf">
   * Set the label of `C:` drive to `OS`
   * Make sure the label of recovery, restore, myasus is correctly set to `RECOVERY, RESTORE, MYASUS`
   * Set the name of system partition to `SYSTEM` (sometimes has a blank label, this partition should contain `EFI` folder and is around 100MB in size)
  
   <img width="446" alt="Screenshot 2024-06-16 122623-min" src="https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/d297818a-6839-43dc-9613-2d709230b468">
* Link Windows Recovery Environment to `RECOVERY` partition
   * Search for command prompt and run it as administrator and execute the following commands:
      * `diskpart`
      * `list disk`
      * `select disk #`  (probably disk 0)
      * `list vol`
      * `select vol #`  (select the `RECOVERY` partition)
      * `assign letter=x`
      * `exit`
    
      <img width="554" alt="Screenshot 2024-06-16 122859-min" src="https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/1c414d4d-2dd4-4f48-b7e7-01e10003d092">
   * Next, navigate in Windows Explorer to `C:\windows\system32\Recovery` DELETE or RENAME the file `ReAgent.xml`
   * Go back to the elevated command prompt and execute the following commands:
      * `reagentc /disable`  (just in case it's enabled with the wrong image, likely not)
      * `reagentc /setreimage /path x:\Recovery\WindowsRE /target C:\Windows`
      * `reagentc /enable`
      * `diskpart`
      * `list disk`
      * `select disk #`  (probably disk 0)
      * `list vol`
      * `select vol #`  (select the `RECOVERY` partition)
      * `remove letter=x`
      * `exit`
     
      <img width="564" alt="Screenshot 2024-06-16 122918-min" src="https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/35aa1f7e-728e-4593-a69e-8c3e15adc6bb">
* Now go to WinRE (by holding shift and hitting restart) in troubleshoot there should be MyAsus in WinRE option!
  ![b864020e-5000-493c-8695-f3c28fbc4fcb-min](https://github.com/mjishnu/Fix-MyAsus-In-WinRE/assets/83004520/87d3c5bf-d743-4018-bc3d-9dc307e9b64e)

credits:

[https://www.reddit.com/r/ASUS/comments/r6rd94/comment/k5hbaqi](https://www.reddit.com/r/ASUS/comments/r6rd94/comment/k5hbaqi) [https://www.reddit.com/r/ZephyrusG14/comments/nvvldu/comment/h167f0e](https://www.reddit.com/r/ZephyrusG14/comments/nvvldu/comment/h167f0e)

## Restore RECOVERY, RESTORE, MYASUS partitions

* For this to work we need to acquire donor backup images of MYASUS, RESTORE, RECOVERY partitions ideally from someone with the same device.
   * To create backup image ask the donor to follow this guide: [How to Backup Partition to Image File](https://www.diskgenius.com/manual/partition-backup.php)
   * If no donor available and you have RESTORE partition intact you can try using this backup image. The file format, partition size and link are given below (**EXPERIMENTAL NOT TESTED**) 
      * MYASUS partition \[FAT32, 200MB\]: [https://drive.google.com/file/d/1utA8hrVOf7s10cC5dd0eqMBdIXuT9id6/view?usp=sharing](https://drive.google.com/file/d/1utA8hrVOf7s10cC5dd0eqMBdIXuT9id6/view?usp=sharing) 
      * RECOVERY partition \[NTFS, 700 MB\] (**HIGHLY EXPERIMENTAL**) : [https://drive.google.com/file/d/1fb5E8ffKV2C-t8K5khG5SrvRO9xrmik5/view?usp=sharing](https://drive.google.com/file/d/1fb5E8ffKV2C-t8K5khG5SrvRO9xrmik5/view?usp=sharing)
* [Create partitions](https://softwarekeep.com/en-in/blogs/how-to/how-to-create-partitions-on-windows-10) with same size and file format as the donor (ask donor for the size and file format of the partition) for me RESTORE partition is around 22GB (22\*1024 MB)
* On the newly created partition restore the backup image: [Restore Partition from Backup Image File](https://www.diskgenius.com/manual/restore-partition-from-img.php)
