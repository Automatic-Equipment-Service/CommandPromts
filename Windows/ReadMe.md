# **Windows Single Line Commands**
There will be some extra .bat files in this directory simply to make some things easier
<br/>

## **Batch/CMD Commands**
- Use this to Add a Azure AD user directly to a local system group, Specifically for Azure Joined Devices
   - Net localgroup Administrators /add "AzureAD\FULL EMAIL ADDRESS"
- Add User account to Windows with group and password
   - net user USERNAME PASSWORD /add && net localgroup administrators USERNAME /add && net user
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
- System File Checker. Usefull to see if windows has any broken files in the OS level
   - SFC
      - /scannow : will check the integrity of all protected system files. If a problem is found, the files will be repaired with backed-up system files.
      - /verifyonly : Check the integrity but don’t repair the files.
      - /scanfile : Scan the integrity of specific files and fix if corrupted.
      - /verifyfile : Verify the integrity of specific files but don’t repair them
      - /offbootdir : Use this to do repairs on an offline boot directory.
      - /offwindir : Use this to do repairs on an offline Windows directory.
      - /offlogfile : Specify a path to save a log file with scan results.
- Check Disk is an obvious one
   - chkdsk [volume letter] [args]
      - /f : Fixes errors on the disk. The disk must be locked. If chkdsk cannot lock the drive, a message appears that asks you if you want to check the drive the next time you restart the computer.
      - /x : Forces the volume to dismount first, if necessary. All open handles to the drive are invalidated. /x also includes the functionality of /f.
      - /r : Locates bad sectors and recovers readable information. The disk must be locked. /r includes the functionality of /f, with the additional analysis of physical disk errors.
         - Combining f x r is usually the best combo to run when you need to run Check Disk against C:\
- Exporting Drivers from Windows Installation. There are two commands. The first will make a list of all drivers and the second will actually copy the drivers from windows to a chosen folder. Might be easier to find which one you need using the list first
      - Installing the exported driver should be as simple as right click/install on the inf file inside the export
   - dism.exe /Online /Get-Drivers > [DIRECTORY TO MAKE LIST IN]\driverlist.txt
   - dism /online /export-driver /destination:[LOCATION TO STORE FILES IN]
<br/>
<br/>

## **Powershell Commands**