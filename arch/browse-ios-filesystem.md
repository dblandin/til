# Browse iOS filesystem

As is often the case, the Arch Wiki has a great page for all things iOS and Arch
Linux.

https://wiki.archlinux.org/index.php/IOS#Connecting_to_a_device

Install the ifuse, libimobiledevice and ideviceinstaller-git (AUR) packages.

```
sudo pacman -S ifuse libimobiledevice
yay -S ideviceinstaller-git
```

Pair the phone:

```
idevicepair pair
```

You may need to run this command again after trusting you system via the iOS
device.

```
sudo mkdir /media/iphone
sudo chown devon:devon /media/iphone
ifuse /media/iphone/
```

Now you can browser the filesystem at `/media/iphone`

Unmount the filesystem before unplugging:

```
fusermount -u /media/iphone
```
