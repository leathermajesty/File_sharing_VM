
## On VM (linux)
```
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
```
## On Host (windows) VM settings

Click on, Devices -> Insert Guest Additions CD Image

## On VM (linux)
```
sudo mkdir -p /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
sudo ./VBoxLinuxAdditions.run
reboot -f
```

## On Host (windows) VM settings

Goto Settings -> Shared folders -> edit share

check in 
	Auto-mount
	make-permanent 

Fill
	Folder Path:- <Windows_path>
	Folder Name:- <Name_on_Window>
	Mount point:- /mnt/cdrom
