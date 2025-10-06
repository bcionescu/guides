# Syncthing with Raspberry Pi 5

![Raspberry Pi 5 with GeeekPi Argon Neo 5 case](/backups/images/raspberry-pi-inside-case.JPG)

```
DISCLAIMER: I take no responsibility for any potential security issues, or data loss. Please do your own research when implementing any guide.
```

I love backups. Whenever I create a project, I can make copies of it and store said copies in multiple locations, so that, in the event of data loss, I can still recover everything.

There are basic things you can do, such as using a cloud service and maintaining local backups; however, it would be preferable to have backups on a remote device that I control and manage.

I've always wanted to use a Raspberry Pi for such a purpose. In terms of what technology to use, there are multiple choices. For this current project, I opted for [Syncthing](https://syncthing.net/), which enables data synchronization across multiple devices without requiring a static IP.

Each device has a unique ID, and Syncthing's relay system tracks IP changes, ensuring that devices stay connected and that your data remains synced.

## Operating System

In terms of OS, I wanted something as lightweight as possible, as I will be running it on my Raspberry Pi 5. In reality, I'm underusing the hardware. Here is what `htop` looked like during a sync.

![htop during sync](/backups/images/htop-during-sync.png)

First, as I am on macOS, I installed the Raspberry Pi Imager via `brew` with the following command:

```bash
brew install --cask raspberry-pi-imager
```

I selected my Raspberry Pi model, `Raspberry Pi OS Lite` as the Operating System, and picked a microSD card as the device to image. Then I hit `Cmd + Shift + X` to bring up the hidden options.

![Hidden options in Raspberry Pi Imager](/backups/images/hidden_options.png)
This allowed me to configure a few things, such as giving it my WiFi credentials and setting up SSH. This way, I was able to remote in to it immediately after it booted.

## Storage

Before we proceed to the next step, I'd like to discuss storage. The OS itself runs off a 128GB microSD card, but for storing my actual backups, I wanted a lot more.

I had a spare 4TB Crucial SSD lying around from an older project, and fortunately, my Raspberry Pi was able to detect and use it. Some SSD models do not work with Raspberry Pis, so I got lucky.

![GeeekPi Argon Neo 5 M.2 SSD slot](/backups/images/raspberry-pi-case-m2-ssd.jpg)

In terms of the case, I'm using the GeeekPi Argon Neo 5, which comes with a cooler, as well as an M.2 slot—sweet! Also, it looks like something the Empire would build, does it not?

**Dun dun dun... dun-dun-dun... dun-dun-dun ;)**

## SSH

Now that the Raspberry Pi was up and running, I didn't want to use an external keyboard or monitor; I just wanted to SSH straight into it.

```bash
ssh pi@raspberrypi.local
```

This worked because I set up SSH during the imaging process. Also, make sure to replace `pi` with the username you chose, assuming you went the username and password route, rather than the SSH key route.

If you can't SSH directly into it for some reason, you can try pinging it instead to get its IP address. 

```bash
ping raspberrypi.local
```

Alternatively, you can use `nmap` to try and find it, but that goes beyond the scope of this project.

Once I was in, the first thing I did was perform some updates.

```bash
sudo apt update && sudo apt upgrade -y
```

## Installing Syncthing

The only app I need to install for now is Syncthing, which can be easily done by following the [official instructions](https://apt.syncthing.net/).

## Port Forwarding

Now, Syncthing uses a GUI. Since this is a headless Linux setup and we're only using SSH, we'll need to perform some port forwarding to access both the MacBook's GUI and the Raspberry Pi's GUI as well.

Exit the SSH channel and run the following command on your main machine.

```bash
ssh -L 8385:localhost:8384 pi@raspberrypi.local
```

Now, on your main machine, if you go to `http://localhost:8384`, you'll see the GUI for the local Syncthing instance. If you then open a new tab and go to `http://localhost:8385`, you'll see the GUI for the Raspberry Pi instance.

![two instances of Syncthing on the same machine via SSH Port Forwarding](/backups/images/final-side-by-side.png)

## Connect the machines

Now that we have Syncthing running on both machines, it's time to connect them. If you go on your main machine, you'll see an `Add Remote Device` in the bottom right, in Syncthing.

![syncthing find remote device](/backups/images/find_device.png)

Once you click it, it should immediately locate the other instance, provided both devices are connected to the same network. Before you connect them, go to the Raspberry Pi instance, click on `Actions`, in the top right, and then click `Show ID`. This will allow you to confirm the unique ID of your Raspberry Pi instance.

If they match, invite the other instance to sync with you. Ensure you give it a relevant name, like `Raspberry Pi`, especially if you intend to sync with multiple machines.

Once this is done, go to the other instance and confirm the invitation.

![syncthing confirm connection to remote device](/backups/images/connect_device.png)

## Encryption

Before we share any files, let's discuss encryption.

Unfortunately, running headless Linux on a Raspberry Pi can complicate the encryption process. If you encrypt the entire drive with LUKS, you'll need to type in the password if the device reboots for any reason.

I want something reliable that will reboot and resume operation as quickly as possible in the event of a power outage. However, at the same time, I want my data to be safe.

Ultimately, I reached a compromise. I use `borgbackup` for my backups. Not only is it space-efficient, as it utilizes deduplication, but it's also encrypted. This means that I could store the backup files in plain text, and it would not be a problem, as their actual contents are encrypted.

If you're curious how I use `borgbackup`, feel free to check out my [guide](borg-automation.md).

Even so, in the end, I still decided to encrypt the SSD with LUKS. We'll go over how to do that in a sec. This means that if there is a power cut, I can SSH into the Raspberry Pi, unlock the SSD, and then mount it.

For my needs, this is a good compromise. Now, let's get to how to encrypt the drive.

## Volumes

Running `lsblk` produced the following output.

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
mmcblk0     179:0    0 119.1G  0 disk
├─mmcblk0p1 179:1    0   512M  0 part /boot/firmware
└─mmcblk0p2 179:2    0 118.6G  0 part /
nvme0n1     259:0    0   3.6T  0 disk
```

Before I began, I needed to install `cryptsetup`, as my OS did not have it.

```bash
sudo apt install cryptsetup
```

Then, I prepared `nvme0n1` by first encrypting it with LUKS.

```bash
sudo cryptsetup luksFormat /dev/nvme0n1
```

Next up, I unlocked the encrypted device.

```bash
sudo cryptsetup open /dev/nvme0n1 borg
```

This created `/dev/mapper/borg`. You don't have to call it `borg`, it's just what I called it.

Next up, I created a file system.

```bash
sudo mkfs.ext4 /dev/mapper/borg
```

The following output was then produced by `lsblk`.

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
mmcblk0     179:0    0 119.1G  0 disk
├─mmcblk0p1 179:1    0   512M  0 part  /boot/firmware
└─mmcblk0p2 179:2    0 118.6G  0 part  /
nvme0n1     259:0    0   3.6T  0 disk
└─borg      254:0    0   3.6T  0 crypt
```

Next, I created a mount point and mounted the drive.

```bash
sudo mkdir /mnt/borg
sudo mount /dev/mapper/borg /mnt/borg
```

Finally, for Syncthing to work with the drive, it needed the right permissions.

```bash
sudo chown -R pi:pi /mnt/borg
```

If you later want to unmount and close the device, you run this.

```bash
sudo umount /mnt/borg
sudo cryptsetup close borg
```

## Syncthing

I went ahead and shared the directory from my main machine, accepted it on the Raspberry Pi, and added the path. Since I'm saving it to my `borg` mount, my path was simply `/mnt/borg`.

![syncthing mount path](/backups/images/select_location.png)

Depending on the size of the sync, it may take a little while to initialize, but once it does, you're off and running.

When both machines are on the same network, Syncthing will sync your files locally. When you're out and about, it will use its relays to keep track of IPs and sync the files that way.

## Connectivity

Upon inspecting the connection speed, I realised it was relatively low. I ran the `iw dev wlan0 link` command to obtain some information, and I received the following output.

```bash
Connected to [REDACTED] (on wlan0)
	SSID: [REDACTED]
	freq: 2437
	RX: 1633565546 bytes (3921009 packets)
	TX: 197485871 bytes (1853118 packets)
	signal: -65 dBm
	rx bitrate: 14.4 MBit/s
	tx bitrate: 72.2 MBit/s

	bss flags:	short-slot-time
	dtim period:	1
	beacon int:	100
```

This is not great. It defaulted to 2.4 GHz, as I'm on Wi-Fi. I needed to ensure that it connects to the 5 GHz band. To achieve this, I needed to modify a configuration file.

First, I installed Vim, although one could also use nano.

```bash
sudo apt install vim
```

Then, I opened the following file.

```bash
sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
```

Once open, I added the following line and rebooted the Raspberry Pi.

```bash
freq_list=5180 5200 5220 5240 5260 5280 5300 5320 5500 5520 5540 5560 5580 5600 5620 5640 5660 5680 5700 5720 5740
```

Once the machine was back up, I received the following output when I ran `iw dev wlan0 link`.

```bash
Connected to [REDACTED] (on wlan0)
	SSID: [REDACTED]
	freq: 5620
	RX: 41523 bytes (161 packets)
	TX: 24816 bytes (106 packets)
	signal: -66 dBm
	rx bitrate: 390.0 MBit/s
	tx bitrate: 292.5 MBit/s

	bss flags:
	dtim period:	1
	beacon int:	100
```

Much better!

## Persistence

Once I disconnected from the SSH tunnel, I realised that the transfer had stopped. Interesting. I needed to ensure that everything continued to run smoothly even when I was not connected.

The first step was to create a `systemd` service file for Syncthing.

```bash
sudo vim /etc/systemd/system/syncthing.service
```

Then, I pasted the following in the file.

```bash
[Unit]
Description=Syncthing - Open Source Continuous File Synchronization
Documentation=http://syncthing.net/docs/
After=network-online.target
Wants=network-online.target

[Service]
User=pi
ExecStart=/usr/bin/syncthing -no-browser -home=/home/pi/.config/syncthing
Restart=on-failure
RestartSec=3
Environment=STNORESTART=1
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
```

The `.config` Syncthing directory did not exist, so I created it. After saving the file, I reloaded the `systemd` manager.

```bash
sudo systemctl daemon-reload
```

Next, I enabled and started the Syncthing service.

```bash
sudo systemctl enable syncthing
sudo systemctl start syncthing
```

Once I did so, I ran `sudo systemctl status syncthing`, and received the following output.

```bash
● syncthing.service - Syncthing - Open Source Continuous File Synchronization
     Loaded: loaded (/etc/systemd/system/syncthing.service; enabled; preset: enabled)
     Active: active (running) since Sat 2025-06-14 19:29:23 BST; 5s ago
       Docs: http://syncthing.net/docs/
   Main PID: 1775 (syncthing)
      Tasks: 18 (limit: 4770)
        CPU: 525ms
     CGroup: /system.slice/syncthing.service
             ├─1775 /usr/bin/syncthing -no-browser -home=/home/pi/.config/syncthing
             └─1784 /usr/bin/syncthing -no-browser -home=/home/pi/.config/syncthing
```

Once I dropped the SSH connection, Syncthing continued to work in the background.

Success!
