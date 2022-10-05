# Create LVM from scratch

**List all physical disks**

*lsblk*

**Create physical volume (pv)**

*pvcreate /dev/sdb*

**Verify the PV status**

*pvscan*<br/>
*pvs*<br/>
*pvdisplay /dev/sdb*

**Create volume group (vg) means adding physical volume (pv) to vg.**

*vgcreate vg01 /dev/sdb*

**Verify the volume** 

*group status*<br/>
*vgscan*<br/>
*vgs*<br/>
*vgdisplay vg01*

**Create a logical volume (lv) from volume group (vg)**

*lvcreate -l +100%FREE -n lv01 vg01*<br/>
*lvcreate -L 50G -n lv01 vg01*

**Verify the logical volume**

*volume*<br/>
*lvscan*<br/>
*lvs*<br/>
*lvdisplay /dev/vg01/lv01*

**Format the lvm partition with ‘ext4’ file system by using mkfs command**

*mkfs.ext4 /dev/vg01/lv01*

**Create a mount point using mkdir command and mount it**

*mkdir /data*<br/>
*mount /dev/vg01/lv01 /data*

**Verify the mount** 

*df -Th*

**Permanent mounting append following entry in /etc/fstab file.**

*echo '/dev/vg01/lv01  /data  ext4  defaults 0 0' | sudo  tee -a /etc/fstab*

# Add disks to existed LVM

**Create physical volumes (pv) on new disk /dev/sdc**

*pvcreate /dev/sdc*

**Run the following command to verify**

*lvmdiskscan -l*

**Add newly created pv named /dev/sdc to an existing lv**

*vgextended vg01 /dev/sdc*

**Extend more space of logical volume:**

***- Add 10GB:***

*lvextend -L+10G /dev/vg01/lv01*

***- Add 100% of new disk to current LVM:***

*lvextend -l +100%FREE /dev/vg01/lv01*

**Resize lvm after expand more size**

*resize2fs -p /dev/vg01/lv01*

**Verify results**

*df -H*


