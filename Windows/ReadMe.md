# **Windows Single Line Commands**
This is a list of Useful commands to run in windows. There is no single use for these it is mostly just a collection for easier reference. There will be some extra .bat files in this directory simply to make some things easier.
<br/>

### **Nice to Knows**
- Running any command with a >filename.txt will log all output of the command to a file for a review
- Running any command with a >>filename.txt will add output of run command to a file for review, this is usfull to continue to add info to the same log file as a previous run
- The only way to run commands as admin in windows is via a "Run As Administrator" version of Command Prompt

## **Batch/CMD Commands**
- Use this to Add a Azure AD user directly to a local system group, Specifically for Azure Joined Devices.
   - Net localgroup Administrators /add "AzureAD\FULL EMAIL ADDRESS"
- Add User account to Windows with group and password and add them to Administrators group
   - net user [USERNAME] [PASSWORD] /add && net localgroup administrators [USERNAME] /add && net user
- List User UUIDs
   - WMIC useraccount get name,sid
- Change System Name 
   - WMIC ComputerSystem where Name="%computername%" call Rename Name="NEW NAME"
- Get Windows Version ##
   - This is to pull the Windows 10 Feature Update Name that the Current System is
      - REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | findstr ReleaseId
   - To pull the "Build Version" use
      - REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | findstr CurrentBuildNumber
- Run the "clearprintqueue.bat" in order to clear the print queue on a windows machine. Specifically usefull when a print stops responding and just starts filling the queue with dud files. The file does need to be run as admin seeing as it needs to disable services.
- System File Checker. Usefull to see if windows has any broken files in the OS level. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/sfc)
   - SFC
      - /scannow : will check the integrity of all protected system files. If a problem is found, the files will be repaired with backed-up system files.
      - /verifyonly : Check the integrity but don’t repair the files.
      - /scanfile : Scan the integrity of specific files and fix if corrupted.
      - /verifyfile : Verify the integrity of specific files but don’t repair them
      - /offbootdir : Use this to do repairs on an offline boot directory.
      - /offwindir : Use this to do repairs on an offline Windows directory.
      - /offlogfile : Specify a path to save a log file with scan results.
- Check Disk is an obvious one. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/chkdsk)
   - chkdsk [volume letter] [args]
      - /f : Fixes errors on the disk. The disk must be locked. If chkdsk cannot lock the drive, a message appears that asks you if you want to check the drive the next time you restart the computer.
      - /x : Forces the volume to dismount first, if necessary. All open handles to the drive are invalidated. /x also includes the functionality of /f.
      - /r : Locates bad sectors and recovers readable information. The disk must be locked. /r includes the functionality of /f, with the additional analysis of physical disk errors.
         - Combining f x r is usually the best combo to run when you need to run Check Disk against C:\
- Exporting Drivers from Windows Installation. There are two commands. The first will make a list of all drivers and the second will actually copy the drivers from windows to a chosen folder. Might be easier to find which one you need using the list first
      - Installing the exported driver should be as simple as right click/install on the inf file inside the export
   - dism.exe /Online /Get-Drivers > [DIRECTORY TO MAKE LIST IN]\driverlist.txt
   - dism /online /export-driver /destination:[LOCATION TO STORE FILES IN]
- Clearing Print Spooler Queue
   - there is a [bat file for this in this repo](https://github.com/Automatic-Equipment-Service/CommandPromts/blob/main/Windows/clearprintqueue.bat) that needs to be run as administrator but here is the single line version that also needs to be run as admin
      - net stop spooler && del %systemroot%\System32\spool\printers\* /Q /F /S && net start spooler
- Built in SSH Utilities based on OpenSSH at time of entry. [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
   - ssh [USER]@[IPADDRESS OR HOSTNAME]
   - If you need to remove a key because of a host change or any other reason, then run the following command
      - ssh-keygen -R "IPADDRESS OR HOSTNAME"
- Toggle all windows Firewall profiles on or off
   - NetSh Advfirewall set allprofiles state [On | Off]
   - Netsh Advfirewall show allprofiles
- Take Owership of a Folder of File
   - takeown /F <filename>
   - takeown /f <foldername> /r /d y
<br/>

## **Powershell Commands**