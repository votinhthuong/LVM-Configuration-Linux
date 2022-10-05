List all physical disks

*lsblk*

Create physical volume (pv)

*pvcreate /dev/sdb*

Verify the PV status

*pvscan
pvs
pvdisplay /dev/sdb

Create volume group (vg) means adding physical volume (pv) to vg.

*vgcreate vg01 /dev/sdb*

Verify the volume 

*group status
vgscan
vgs
vgdisplay vg01*

Create a logical volume (lv) from volume group (vg)

*lvcreate -l +100%FREE -n lv01 vg01
lvcreate -L 50G -n lv01 vg01*

Verify the logical volume

*volume
lvscan
lvs
lvdisplay /dev/vg01/lv01*

Format the lvm partition with ‘ext4’ file system by using mkfs command

*mkfs.ext4 /dev/vg01/lv01*

Create a mount point using mkdir command and mount it

*mkdir /data
mount /dev/vg01/lv01 /data*

Verify the mount 

*df -Th*

Permanent mounting append following entry in /etc/fstab file.

*echo '/dev/vg01/lv01  /data  ext4  defaults 0 0' | sudo  tee -a /etc/fstab*
