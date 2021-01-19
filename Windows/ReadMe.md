#Windows Single Line Commands
- Use this to Add a Azure AD user directly to a local system group
   - Net localgroup Administrators /add "AzureAD\<FULL EMAIL ADDRESS>"
- Add User account to Windows with group and password
   - net user <USERNAME> <PASSWORD> /add && net localgroup administrators <USERNAME AGAIN> /add && net user
- List User UUIDs
   - wmic useraccount get name,sid
- Change System Name 
   - WMIC ComputerSystem where Name="%computername%" call Rename Name="<NEW NAME>"
