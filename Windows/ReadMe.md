# **Windows Single Line Commands**

This is a list of Useful commands to run in windows. There is no single use for these it is mostly just a collection for easier reference. There will be some extra .bat files in this directory simply to make some things easier.  

## **Nice to Knows**

- Running any command with a >filename.txt will log all output of the command to a file for a review
- Running any command with a >>filename.txt will add output of run command to a file for review, this is usfull to continue to add info to the same log file as a previous run
- The only way to run commands as admin in windows is via a "Run As Administrator" version of Command Prompt

## **Useful Folder Locations**

- Windows 7 / 10 Per Users Startup Folder
  - `%appdata%\Microsoft\Windows\Start Menu\Programs\Startup`
    - via run you can use `Shell:startup`
- Windows 7 / 10 All Users Startup Folder
  - `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`
    - via run you can use `Shell:common startup`

## **Batch/CMD Commands**

- Shutdown Command
  - `shutdown /s /t XXXX`
    - Fully Shutsdown the System in `XXXX` seconds, the `/s` is to indicate Shutdown rather then restart
  - `shutdown /r /t XXXX`
    - Restarts the system in `XXXX` seconds, the `/r` is what indicates a restart
  - Both commands can omit the `/t` argument to restart in *30 to 60 secs* (never timed it)
  - using a `/t 1` will basically force it to happen immedatly
- Use this to Add a Azure AD user directly to a local system group, Specifically for Azure Joined Devices.
  - `Net localgroup Administrators /add "AzureAD\FULL EMAIL ADDRESS"`
- Add User account to Windows with group and password and add them to Administrators group
  - `net user [USERNAME] [PASSWORD] /add && net localgroup administrators [USERNAME] /add && net user`
- Changing a Users password via the Net user command is as simple as dropping the `/add` from base above command string
- Toggling the Expiration of user passwords (Only for Local Accounts)
  - `wmic UserAccount set PasswordExpires=False`
    - this will toggle for all accounts
  - `wmic UserAccount where Name="user name" set PasswordExpires=False`
    - this will toggle for the specific account, username needs to be exact. Run net user to list the accounts first.
- List User UUIDs
  - `WMIC useraccount get name,sid`
- Change System Name
  - `WMIC ComputerSystem where Name="%computername%" call Rename Name="NEW NAME"`
- Get Windows Version ##
  - This is to pull the Windows 10 Feature Update Name that the Current System is
    - `REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | findstr ReleaseId`
  - To pull the "Build Version" use
    - `REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | findstr CurrentBuildNumber`
- Run the `clearprintqueue.bat` in order to clear the print queue on a windows machine. Specifically usefull when a print stops responding and just starts filling the queue with dud files. The file does need to be run as admin seeing as it needs to disable services.
- System File Checker. Usefull to see if windows has any broken files in the OS level. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/sfc)
  - `SFC`
    - `/scannow` : will check the integrity of all protected system files. If a problem is found, the files will be repaired with backed-up system files.
    - `/verifyonly` : Check the integrity but don’t repair the files.
    - `/scanfile` : Scan the integrity of specific files and fix if corrupted.
    - `/verifyfile` : Verify the integrity of specific files but don’t repair them
    - `/offbootdir` : Use this to do repairs on an offline boot directory.
    - `/offwindir` : Use this to do repairs on an offline Windows directory.
    - `/offlogfile` : Specify a path to save a log file with scan results.
- Check Disk is an obvious one. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/chkdsk)
  - `chkdsk [volume letter] [args]`
    - `/f` : Fixes errors on the disk. The disk must be locked. If chkdsk cannot lock the drive, a message appears that asks you if you want to check the drive the next time you restart the computer.
    - `/x` : Forces the volume to dismount first, if necessary. All open handles to the drive are invalidated. `/x` also includes the functionality of `/f`.
    - `/r` : Locates bad sectors and recovers readable information. The disk must be locked. `/r` includes the functionality of `/f`, with the additional analysis of physical disk errors.
      - Combining f x r is usually the best combo to run when you need to run Check Disk against C:\
- Exporting Drivers from Windows Installation. There are two commands. The first will make a list of all drivers and the second will actually copy the drivers from windows to a chosen folder. Might be easier to find which one you need using the list first
  - Installing the exported driver should be as simple as right click/install on the inf file inside the export
  - `dism.exe /Online /Get-Drivers > [DIRECTORY TO MAKE LIST IN]\driverlist.txt`
  - `dism /online /export-driver /destination:[LOCATION TO STORE FILES IN]`
- Clearing Print Spooler Queue
  - there is a [bat file for this in this repo](https://github.com/Automatic-Equipment-Service/CommandPromts/blob/main/Windows/clearprintqueue.bat) that needs to be run as administrator but here is the single line version that also needs to be run as admin
    - `net stop spooler && del %systemroot%\System32\spool\printers\* /Q /F /S && net start spooler`
- Built in SSH Utilities based on OpenSSH at time of entry. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
  - `ssh [USER]@[IPADDRESS OR HOSTNAME]`
  - If you need to remove a key because of a host change or any other reason, then run the following command
    - `ssh-keygen -R "IPADDRESS OR HOSTNAME"`
- Toggle all windows Firewall profiles on or off
  - `NetSh Advfirewall set allprofiles state [On | Off]`
  - `Netsh Advfirewall show allprofiles`
- Take Owership of a Folder of File
  - `takeown /F <filename>`
  - `takeown /f <foldername> /r /d y`
- Setting Power Options from CMD
  - `powercfg`
    - Sample

      ```{ .bat }
      powercfg /change standby-timeout-ac 0
      powercfg /change standby-timeout-dc 0
      powercfg /change monitor-timeout-ac 15
      powercfg /change monitor-timeout-dc 15
      powercfg /change hibernate-timeout-ac 0
      powercfg /change hibernate-timeout-dc 20
      ```

      This sample would be good for laptops that need to stay on at night. This will Keep the laptop on but let the screen go to sleep after 15 mins but keep it on while wall powered (AC). But while on battery (DC) will still time out then set the device to go to sleep after 20 mins.
- Changing IP settings from CMD
  - WARNING if you change the IP before the DNS you will loose internet access (big concern if running it on a remote machine)
  - `netsh` is the tool to use
    - Seeing as these commands need the "Name" of the interface run the following to see a list
      - `netsh interface show interface`
    - To Set to dhcp run the following using the interface name of the adapter that you are looking to change.
      - `netsh interface ipv4 set dnsservers name="YOUR INTERFACE NAME" source=dhcp`
      - `netsh interface ipv4 set address name=”YOUR INTERFACE NAME” source=dhcp`
    - To Static IP an adapter run the following (reminder that you should change DNS then IP, if you are doing this remotely you will loose access to the device because of loss of internet. You may also want to do an external DNS first if you don't have a local resolver)
      - `netsh interface ipv4 set dns name="YOUR INTERFACE NAME" static 1.1.1.1`
      - `netsh interface ipv4 set dns name="YOUR INTERFACE NAME" static 1.0.0.1 index=2`
      - `netsh interface ipv4 set address name="YOUR INTERFACE NAME" static IP_ADDRESS SUBNET_MASK GATEWAY`
    - To add additional IPs to a nic
      - `netsh interface ipv4 add address "YOUR INTERFACE NAME" IP_ADDRESS SUBNET_MASK GATEWAY`

## **Powershell Commands**

- Allow Powershell execution [See Here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.1) for More detail
  - `set-executionpolicy remotesigned`
- Enable WSL (windows sub-system linux) | (run as administrator)
  - `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
    - This installs WSL 1 which does not support gui based linux. [Follow this link for details on finishing the update to WSL 2 if needed](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
- Changing Network Adapter Priority
  - `Get-NetIPInterface`
    - This Should List Current adapters and their Index ID on the left
  - `Set-NetIPInterface -InterfaceIndex "IndexID for the wanted Adapter" -InterfaceMetric "New Metric Value"`
    - The Lower the Metric Value the Higher the priority (like golf, lower better)

## **Powershell Modules**

- Windows Update Module
  - You can install it using the following
    - `Install-Module -Name PSWindowsUpdate -Force`
    - `Import-Module PSWindowsUpdate`
  - You may need change the powershell execution policy if you can't install or import the module.
  - Commands
    - `Get-WindowsUpdate`
      - This will search for all windows updates that are avaiable for the system
    - `Install-WindowsUpdate`
      - This will prompt you for all updates that are avaiable to be installed

___

## **Connectwise**

Ok this sections is a little more focused because we use [Connectwise](https://www.connectwise.com/) Products in our daily business. So This section will mostly be used for Control and Automate Stuff from [Connectwise](https://www.connectwise.com/). So TLDR is that this section will probably be useless to other people but at the same time will be quicker to us then digging through the wonders of the "Connectiwse University" (its a pain to navigate, and find what you need)

### **Control**

This section is for Connectwise Control (Screenconnect) and will mostly only be used on the sudo command prompt that is built into its "Host Information" page for each device. [See Here](https://docs.connectwise.com/ConnectWise_Control_Documentation/Get_started/Host_page/Run_a_command_from_the_Host_page) for running commands from Control.

- Send a Powershell command, Prefix your command with
  - `#!ps`
- Send a bash command (defaults to bash for linux and mac hosts)
  - `#!bash`
- Send a Windows CMD (Defaults to this for windows)
  - `#!cmd`
- Modify Time out to wait on the Host to run the command (this example is 1.5 minutes of pause)
  - `#timeout=90000`
- Modify the Max Length of the Returned characters (default at present is 5000, example bumps to 50,000)
  - `#maxlength=50000`

___

## **SQL Related**

- *SQL Server Configuration Manager* Cannot connect to WMI (Only encountered this once)
  - In *CMD Admin Prompt* run the following and replace `<number>` with the version of sql server in use
    - `mofcomp "%programfiles(x86)%\Microsoft SQL Server\<number>\Shared\sqlmgmproviderxpsp2up.mof`
  - Open the Services Manager (`services.msc`)
  - Restart `Windows Management Instrumentation` Service
  - Relaunch *SQL Server Configuration Manager*
