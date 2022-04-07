# **Linux Single Line Commands**

Most of these commands are ones that we have been using in Ubuntu and some other debian flavors. If one of the commands is specific to another flavor it will be noted with the command.

## **Ubuntu/Debian**

- Changing a password
  - `passwd`
    - to change your password
  - `sudo passwd [USERNAME]`
    - to change other users password. this should work with root too, but you may need to switch to root with "su" before running "passwd" on its own there
- IP information
  - `ip [arg]`
    - `address` : same as ipconfig on windows
    - `address` : same as ipconfig on windows`
- Disableing or enableing firewall entirely
  - `sudo ufw` [disable/enable]
- Checking Release Version
  - OS Information
    - `cat /etc/os-release`
  - Install Version
    - `cat /etc/*_version`
- Changing System Hostname, this will be multiple steps
  - Use the following to list current Hostname if needed. Depending on the environment one of these might not work
    - `hostnamectl`
    - `echo "$HOSTNAME"`
  - `sudo hostnamectl set-hostname newNameHere`
  - `sudo nano /etc/hosts`
    - or your choice of text editor
    - Replace any reference to your original hostname with the New hostname you entered earlier
  - `sudo reboot`
    - some variants of ubuntu may not require a reboot or the hosts file edits but just check to make sure
- Removing the Need to enter a password for sudo in Ubuntu 16.04 and seems to work in 20.04
  - `sudo visudo`
    - and change "sudo" group to look like this
      - `%sudo   ALL=(ALL) NOPASSWD: ALL`
- List Current User
  - `whoami`
- add read write perminions to file or folder
  - `chmod a+rw` [file/foldername]
  - `chmod -R a+rw` [file/foldername]
    - this is recursive to cover files folders inside of the directory
    - you can add an "x" to the rw to allow for execution of the relevant file or folder
    - "a" is for all users and groups
  - `chmod +x`
        - this will add execute permissions to a file
- Setting the Time in Debian
  - `date --set 1998-11-02`
  - `date --set 21:08:00`
  - `hwclock --systohc`
    - This will sync the system clock to the hardware clock to make the time persist over reboots. Shouldn't be needed in debian if the system is shutdown properly because it autosaves the time when it restarts.
- Finding UID and GID
  - `id -i`
  - `id -g`

## Programs

- cifs-utils
  - `sudo mount -t cifs -o user=<Username>,uid=<UID>,gid=<GID> "//<HOST>/<PATH>" <Local Mount Point>`
  - fstab example
    - `place holdertext`

## External Links

- [Linux Command Library](https://linuxcommandlibrary.com/)
  - [Oneliners](https://linuxcommandlibrary.com/basic/oneliners.html)
