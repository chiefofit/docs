## Preface
The following notes are command lines with options selected for common tasks in a linux system.
For more in-depth knowledge on each linux commands, it would be beneficial to dig further on each of these tools.
A good start would be invoking the `-h` / `--help` or the `man` pages for each of these tools.

For brevity, we use a theoritical user named `John Wick`, with usernames and credential files derived from his name and relevant skills.


## Screen capturing the desktop

```sh
sudo apt-get install byzanz
```

Install ScreenRuler/Kruler as well

Record using
```sh
byzanz-record --duration=30 --x=2 --y=50 --width=1095 --height=595 out.gif
byzanz-record --duration=15 --x=2 --y=50 --width=1095 --height=595 out.gif
```


## Generating password in linux
```sh
openssl rand -base64 <length>
```

example:

```sh
openssl rand -base64 10
XpNmDvixAx+o0Q==

```


```sh
sudo apt-get install pwgen
```

```
pwgen

goh8Ahwe HiMooqu0 eelie1So Oheey5ig Igh7uo1a gieDieC9 ESeroh9I Iu9xeedu
Xooghe9d zi2Ujaej xei1Ceec Pe4ahphi tah7eoHi ex0Exoh7 Yu3cee4g zieJoo5v
iy1jaeSi biPovie9 Eeghai7d tei4Oori En7chae6 aiX1phai fuof4Sei We9Baime


```



## Find in files

### Search for the word `bazaar_v8` in all files ending in .rs

```sh

reset && grep -rnw ./**/*.rs -e "bazaar_v8"
```

### Non-recursive, replace `bazaar_v6` with `bazaar_v8` in files ending in `.rs` in this directory only:

```sh
sed -i -- 's/bazaar_v6/bazaar_v8/g' *.rs

```

### Search function call of `some_function`, and remove the second argument names None.
```sh
sed -i -- 's/some_function(\(.*\), None)/some_function(\1)/g' *.rs

```

### Inject the host IP into docker-compose.yml file
```sh
sed -i -e "s/host.docker.internal/$HOST_IP/g" docker-compose.yml
```

### Recursively search replace
```sh
find . -type f -name "*.rs" -print0 | xargs -0 sed -i 's/foo/bar/g'
```


## Inspecting .so libraries in linux
```sh
nm -gC target/release/libstringtools.so | grep data
readelf -Ws target/release/libstringtools.so
objdump -TC target/release/libstringtools.so

```



## Remove locks in debian base distro (ubuntu, debian, pop)
```
#!/bin/sh
sudo rm /var/lib/dpkg/lock
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
```

## Login to ssh using pem files
```sh
ssh -i sentiment.pem root@54.555.555.555
```

## Monitor temperature sensors in linux
```sh
sudo apt install lm-sensors
sudo sensors-detect
sudo service kmod start
sensors
```

## Monitor hdd temperature
```sh
sudo apt install hddtemp
```


## kill matching process name (don't need to be exact match)

```sh
pkill <process name>
pkill diwata
pkill chrome
```

## Execute a command to a remote machine via ssh
```
ssh user@host '<command>'
ssh wick@web01.jwick.continental.io 'echo $PATH'
```

## generate keystore

```
keytool -genkey -v -keystore jwick.keystore -alias jwick -keyalg RSA -keysize 2048 -validity 10000
```

## Creating a debian executable package .deb
Once you have a binary, simply follow the debian packaging guide.
The first thing to do is to check if you depend on any C libraries or try to run it on a clean install.
Your directory tree should look like this:
```
helloworld
├── usr
│    └── share
│          └── your_binary_folder
│                   └── your_binary
└── DEBIAN
     └── control
```
The first thing is the control file, which is used for managing dependencies:

```
Package: helloworld
Version: 0.0.1-0
Section: misc
Priority: extra
Architecture: all
Depends: [LIBRARIES HERE]
Installed-Size: 8
Maintainer: Your Name <yourname@gmail.com>
Homepage: www.yourhomepage.com
Description: [DESCRIPTION]
```

That's it. Then you can use `dpkg -b ./helloworld helloworld.deb` to package it.
If you want a desktop entry, you need a logo in PNG format and a `.desktop` file. Your tree should now look like this:

```
helloworld
├── usr
│    └── share
│          └── your_binary_folder
│                   └── your_binary
│          └── pixmaps
│                   └── my_icon.png
│          └── applications
│                   └── your_binary.desktop
└── DEBIAN
     └── control
```

In the desktop file, you need to reference the icon.
```
[Desktop Entry]
Name=Hello World
Name[­de­]=Hallo Welt
Comment=My user-visible description
Comment[­de­]=Localized string
Exec=/usr/share/your_binary_folder/your_binary
Icon=my_icon
Terminal=false
Type=Application
Categories=Development;
StartupNotify=false
Watch out for spaces. I took this tutorial from here, however the site is in German. You can google the individual strings to find out what they do.
Then you can give your friend the .deb file. Usually Ubuntu has a graphical installer where you can just hit "install". You can sign it and publish it, but that is beyond my knowledge of how to do.
The application will be installed on his computer under /usr/share/your_binary_folder. Make sure the name doesn't conflict with anything and that the Exec entry is correct.
```

### Rsyncing files

```sh
rsync -avzhP src/folder root@45.55.7.231:~/
```

## Using authenticator via the command-line
```sh
sudo apt install oathtool
oathtool --totp -b <your_secret>
oathtool --totp -b BBZYNASDLJJH6Z5O
```

## Recovering encrypted home folder in ubuntu
```sh
sudo ecryptfs-recover-private /media/ubuntu/<disk-uuid>/.ecryptfs/wick/.Private

sudo ecryptfs-recover-private /media/ubuntu/f8c802c7-a920-4c47-9678-1a8d0be0e721/.ecryptfs/wick/.Private
INFO: Found [/media/ubuntu/f8c802c7-a920-4c47-9678-1a8d0be0e721/.ecryptfs/wick/.Private].
Try to recover this directory? [Y/n]: y
INFO: Found your wrapped-passphrase
Do you know your LOGIN passphrase? [Y/n] y
INFO: Enter your LOGIN passphrase...
Passphrase:
Inserted auth tok with sig [a52dcfe47f5d11f6] into the user session keyring
INFO: Success!  Private data mounted at [/tmp/ecryptfs.6HvWydq4]
```


## Checking performance of rust app
```
sudo perf stat ./target/debug/backend
```

## echo into terminals with color

```sh

echo -e "\e[1mbold\e[0m"
echo -e "\e[3mitalic\e[0m"
echo -e "\e[4munderline\e[0m"
echo -e "\e[9mstrikethrough\e[0m"
echo -e "\e[31mHello World\e[0m"
echo -e "\x1B[31mHello World\e[0m"
echo -e "\e[1mbold\e[0m"
echo -e "\e[3mitalic\e[0m"
echo -e "\e[4munderline\e[0m"
echo -e "\e[9mstrikethrough\e[0m"
echo -e "\e[31mHello World\e[0m"
echo -e "\x1B[31mHello World\e[0m"

```


## Speak what ever is the message that is send in the notication api

```
dbus-monitor "interface='org.freedesktop.Notifications'" | grep --line-buffered "string" | grep --line-buffered -e method -e ":" -e '""' -e urgency -e notify -v | grep --line-buffered '.*(?=string)|(?<=string).*' -oPi | grep --line-buffered -v '^\s*$' | xargs -I '{}' espeak {}
```

## Supress the UI of the notification that gets displayed in the screen whenever there is a notification comming
```
dbus-monitor "interface='org.freedesktop.Notifications'" | grep --line-buffered "string" | grep --line-buffered -e method -e ":" -e '""' -e urgency -e notify -v | grep --line-buffered '.*(?=string)|(?<=string).*' -oPi | grep --line-buffered -v '^\s*$' | xargs -I '{}' pkill notify-osd
```

## Kill the dbus-monitor
```
pkill dbus-monitor
```

## send notification message by
```
notify-send "Hello, How are you?"
```

Linux commands
```
file logo.png
iotop
powertop
nethogs
```


## Recursively Delete files with extension in 1 go
```
find . -name "*.bak" -type f
find . -name "*.bak" -type f -delete
```


## Load testing in Linux
```
sudo apt install apache2-utils

#Suppose we want to see how fast Yahoo can handle 100 requests, with a maximum of 10 requests running concurrently:

ab -n 100 -c 10 http://www.yahoo.com/

ab -n 100 -c 10 http://localhost:8000

```

## Remove the word string with leading <space> on the line

```
echo "  string Hello world!" | sed -e 's/^ *string\s//g'
Hello world!
```

## Unit separator (handyhack)
1c  FS  ␜  ^\  File Separator
1d  GS  ␝  ^]  Group Separator
1e  RS  ␞  ^^  Record Separator
1f  US  ␟  ^_  Unit Separator


## converting svg image to png using inkscape cli
```
inkscape -z bob.svg -e bob.png
```


## Remove untracked files in git repository
```
git clean -f -d
git clean -d -f -f
git reset --hard HEAD
```



## Using letsencrypt to generate an ssl certificate for a domain name we own
Example, if we own the
domain name: some-domain.xyz, some-other-domain.xyz
using email: wick@sub.hush.com

Install certbot: sudo apt install certbot
```
$> sudo certbot certonly --manual --preferred-challenges=dns --email wick@sub.hush.com --agree-tos -d some-domain.xyz
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Obtaining a new certificate
Performing the following challenges:
dns-01 challenge for some-domain.xyz

-------------------------------------------------------------------------------
NOTE: The IP of this machine will be publicly logged as having requested this
certificate. If you're running certbot in manual mode on a machine that is not
your server, please ensure you're okay with that.

Are the OK with your IP being logged?
-------------------------------------------------------------------------------
(Y)es/(N)o: Y

-------------------------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.some-domain.xyz with the following value:

b8ZXF0f4yVZxvxfXLjYWCnpEOidBU1LjnxPpGqMiT11

Before continuing, verify the record is deployed.
-------------------------------------------------------------------------------
Press Enter to Continue
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/some-domain.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/some-domain.xyz/privkey.pem
   Your cert will expire on 2019-11-01. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

## Checking if the txt record has propagated
```sh
nslookup -type=TXT _acme-challenge.some-domain.xyz
```
## Or by using google api
```
curl "https://dns.google.com/resolve?name=_acme-challenge.some-domain.xyz&type=TXT"
```

Using the certificates to be serve in rust webserver with rust-tls
```sh
cargo run --example tlsserver -- --port 8443 --certs ~/some-domain.xyz/cert1.pem --key ~/some-domain.xyz/privkey1.pem http
# to use only protocol version 1.3
cargo run --example tlsserver -- --port 8443 --certs ~/some-domain.xyz/cert1.pem --key ~/some-domain.xyz/privkey1.pem --auth /etc/ssl/certs/ca-certificates.crt --protover 1.3 http
$>rustls/rustls-mio/target/debug/examples$ sudo ./tlsserver --port 443 --certs ~/some-domain.xyz/cert1.pem --key ~/some-domain.xyz/privkey1.pem echo
```

Add an entry to /etc/hosts/
127.0.0.1	some-domain.xyz

Then you can open the browser to https://some-domain.xyz


## Encrypting a file hello.txt into hello.enc and decrypting hello.enc to hello.decr

openssl enc -aes-256-cbc -pbkdf2 -iter 20000 -in hello.txt -out hello.enc -k lemeowpwd

openssl enc -d -aes-256-cbc -pbkdf2 -iter 20000 -in hello.enc -out hello.decr -k lemeowpwd

## Adding the current user to wireshark group
```
sudo usermod -aG wireshark $(whoami)
```

## Adding the current user to docker group
```
sudo usermod -aG docker $(whoami)
```


## Adjust brightness in the terminal programatically
```
sudo apt install lxqt-config
lxqt-config-brightness -s 50
```
```
# in the morning at 8 or 9
lxqt-config-brightness -s 40
# in the evening at 4pm
lxqt-config-brightness -s 20
```

# Display the current date in Utc timezone in rfc-3339 format
```
date --utc --rfc-3339=ns
2019-09-13 04:34:37.094444983+00:00
```

## One-line for loop scripting in linux
for i in {0..10}; do echo "hello"; done;


### Find out how much is the size of the directory
```sh
du -sh Downloads
```

### Find out how much storage is used for all the directory inside the current working directory
```sh
du -csh *
```

## Show the installed fonts in the system
```sh
fc-list
```

## Jobs: fg, bg, CTRL-Z
- Execute a command appended with `&` to spawn in the background
- `fg` to put the last job in the foreground
    - `fg %1` to move background job number 1 to foreground, it can also be done with `fg 1`
- CTRL-Z to put the job in background again.
- `jobs` to list down the current running jobs in the terminal

## Show process in a nice tree structure
ps --ppid 2 -p 2 --deselect -fo pid,ppid,command

## Calculate stuffs in the terminal
```
bc
bc <<< 3.14*10-1.4
```

## Creating a bootable linux USB device using dd

```
## list down the devices
sudo fdisk -l

## format the disk/ usb device
sudo mkfs.vfat /dev/<disk_device_name> -I

## Duplicate the data from the iso image to the device
sudo dd if=$HOME/OS_IMAGES/ubuntu19.10.iso of=/dev/<disk_device_name>

## Format a disk using dd
sudo dd if=/dev/urandom of=/dev/<disk_device_name> bs=4k

## Example
sudo dd if=/dev/urandom of=/dev/nvme0n1 bs=4k
sudo dd if=/dev/urandom of=/dev/sda1

## Erase the disk nvme0n1 at 100MB per write and show progress status
sudo dd if=/dev/urandom of=/dev/nvme0n1 bs=100M status=progress

## Writing 0 to the disk
sudo dd if=/dev/zero of=/dev/nvme0n1 bs=100M status=progress
```

## Use different audio device for each application
https://askubuntu.com/questions/448618/how-to-play-audio-from-different-applications-on-different-output-devices#448619
```
sudo apt install pavucontrol
```

## disk usage
aside from `top`, `htop`
there is also
- `atop`
- `iotop`
usage:
```
    sudo -aP iotop
```

# Launch a game forcing the Vulkan device with specific GPU
```
VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/nvidia_icd.json ./launcher
```

## Installing nvidia driver in newly installed ubuntu
- Download the driver from nvidia site using the firefox browser that goes with ubuntu.
- edit `/etc/default/grub` file and comment out the config that says TIMEOUT to allow the grub bootloader menu show before booting into ubuntu
- Then choose the recovery option in the bootloader.
- Drop to root shell.
    - remove the previously installed nvidia drivers using `sudo apt-get remove --purge '^nvidia-.*'`
    - Navigate to the downloaded nvidia driver
    - run the downloaded nvidia driver with `chmod u+x NVIDIA.*run` and `./NVIDIA*.run`
- Disable the nouveau driver
-
Put this into `/etc/modprobe.d/blacklist-nvidia-nouveau.conf` file
```
blacklist nouveau
options nouveau modeset=0
```

Update the kernel initframfs using
```sh
sudo update-initramfs -u
sudo reboot
```

## Convert jpg to png in the terminal
```sh
find . -name "*.jpg" -exec mogrify -format png {} \;
```

## Optimize png image using
```
pngcrush -reduce -brute input.png output.png
```

## Create a favicon file out of png image
```
convert -resize x16 -gravity center -crop 16x16+0+0 input.png -flatten -colors 256 -background transparent output/favicon.ico
```

## To automatically login to an ssh server
- Copy your ~/.ssh/id_rsa.pub to the remote server ~/.ssh/authorized_keys

## Run a script at startup
  - make a file in `/etc/init.d/nameofscript.sh`
  - Add it to the boot sequence
```
sudo update-rc.d /etc/init.d/nameofscript.sh defaults
```

## Run a script at startup via systemd
- Create a file in /etc/systemd/system/your_script_here.service
```
[Unit]
Description=Your Script Name here
After=default.target

[Service]
ExecStart=/PATH/TO/Script.sh

[Install]
WantedBy=default.target

```
- Reload the systemd configuration using
```
systemctl daemon-reload
```

- Enable the service
```
systemctl enable your_script_here
```

- To try and start the service
```
systemctl start your_script_here
```

- To see the status of the service
```
systemctl status your_script_here
```
