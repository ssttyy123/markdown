---
title: IDEA在Maven项目中使用Lombok build失败
date: 2025-02-18 18:10:00
tags: 
  - HTB
  - Windows
  - Powershell
---
_注1：每一个主机（除了第一个）的密码都是上一个主机的问题答案。_
_注2：使用 powershell 的 cmdlet 命令需要输入 powershell 进入 powershell 命令行环境。_

## Question 1
The flag will print in the banner upon successful login on the host via SSH.
通过SSH成功登录主机后，该 flag 将打印在 banner 中。

---

使用 ssh 命令通过提供的用户和密码登录，在输入密码之前的 banner 中有一串提示文字，其中包含了 flag 。
```text
#################################################################
#                   _    _           _   _                      #
#                  / \  | | ___ _ __| |_| |                     #
#                 / _ \ | |/ _ \ '__| __| |                     #
#                / ___ \| |  __/ |  | |_|_|                     #
#               /_/   \_\_|\___|_|   \__(_)                     #
#                                                               #
#  You are entering into a secured area! Your IP, Login Time,   #
#   Username has been noted and has been sent                   #
#              D0wn_the_rabbit_H0!3 to the server               #
#                       administrator!                          #
#   This service is restricted to authorized users only. All    #
#            activities on this system are logged.              #
#  Unauthorized access will be fully investigated and reported  #
#        to the appropriate law enforcement agencies.           #
#################################################################
```
可以发现 flag 为 D0wn_the_rabbit_H0!3 。

## Question 2
Access the host as user1 and read the contents of the file "flag.txt" located in the users Desktop.
以user1身份访问主机，并读取位于用户桌面中的文件“flag.txt”的内容。

---

在终端中使用 cd 命令进入 Desktop 文件夹，使用 type 命令输出文件 flag.txt 的内容。
```cmd
cd Desktop
dir
type flag.txt
```
输出内容 **Nice and Easy!** 为 flag。

## Question 3
If you search and find the name of this host, you will find the flag for user2.
如果您搜索并找到此主机的名称，您将找到user2的 flag 。

---

使用 systeminfo 命令查询主机属性
```text
Host Name:                 ACADEMY-ICL11
OS Name:                   Microsoft Windows 11 Pro
OS Version:                10.0.22000 N/A Build 22000
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Member Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          htb-student
Registered Organization:   
Product ID:                00331-20309-59368-AA413
Original Install Date:     6/14/2022, 7:21:28 PM
System Boot Time:          2/17/2025, 12:21:32 AM
System Manufacturer:       VMware, Inc.
System Model:              VMware7,1
System Type:               x64-based PC
Processor(s):              2 Processor(s) Installed.
                           [01]: AMD64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2445 Mhz
                           [02]: AMD64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2445 Mhz
BIOS Version:              VMware, Inc. VMW71.00V.23553139.B64.2403260936, 3/26/2024
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory:     4,095 MB
Available Physical Memory: 2,587 MB
Virtual Memory: Max Size:  4,799 MB
Virtual Memory: Available: 3,546 MB
Virtual Memory: In Use:    1,253 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    greenhorn.corp
Logon Server:              N/A
Hotfix(s):                 4 Hotfix(s) Installed.
                           [01]: KB5020617
                           [02]: KB5012170
                           [03]: KB5019961
                           [04]: KB5017850
Network Card(s):           2 NIC(s) Installed.
                           [01]: vmxnet3 Ethernet Adapter
                                 Connection Name: Ethernet0
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.129.0.1
                                 IP address(es)
                                 [01]: 10.129.204.9
                                 [02]: fe80::9d2b:cee6:9a5e:2ae6
                                 [03]: dead:beef::4dba:3f68:4053:1c5a
                                 [04]: dead:beef::b189:28ee:d932:8911
                                 [05]: dead:beef::15d
                           [02]: vmxnet3 Ethernet Adapter
                                 Connection Name: Ethernet1
                                 DHCP Enabled:    No
                                 IP address(es)
                                 [01]: 172.16.5.43
                                 [02]: fe80::29a1:326d:ed0a:4bbf
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
```
或者加入筛选指令使得输出更改简单
```powershell
systeminfo | Select-String "Host Name"
```
输出
```text
Host Name:                 ACADEMY-ICL11
```
可得到 flag 为 **ACADEMY-ICL11** 。

## Question 4
How many hidden files exist on user3's Desktop?
user3 的桌面上存在多少个隐藏文件？

---

使用命令查询和统计隐藏文件数量
```powershell
cd Desktop
(Get-ChildItem -File -Attributes Hidden).Count
```
输出 **101** 即为 flag 。

## Question 5
User4 has a lot of files and folders in their Documents folder. The flag can be found within one of them.
User4 的 Documents 文件夹中有很多文件和文件夹。flag 可以在其中一个中找到。

---

使用 Get-ChildItem 的递归查询参数 -Recurse 配合 Where-Object 筛选文件大小得到该 flag 文件位置，并且使用 Get-Content 命令读取。
```powershell
cd Documents
Get-ChildItem -Path . -File -Recurse | Where-Object { $_.Name -like "*flag*" -and $_.Length -gt 0 } | ForEach-Object { Get-Content $_.FullName }
```
输出结果为 **Digging in The nest** 为 flag 。

## Question 6
How many users exist on this host? (Excluding the DefaultAccount and WDAGUtility)
此主机上有多少用户？（不包括 DefaultAccount 和 DAGUtility）

---

使用 Get-LocalUser 命令并且统计，因为排除了2个用户则需要减去 2 。
```powershell
(Get-LocalUser).Count - 2
```
输出 **14** 即为 flag 。

## Question 7
For this level, you need to find the Registered Owner of the host. The Owner name is the flag.
对于此级别，您需要找到主机的注册所有者。所有者名称就是 flag 。

---

使用 systeminfo 命令查询 Registered Owner 即可
```powershell
systeminfo | Select-String "Registered Owner"
```
输出
```text
Registered Owner:          htb-student
```
其中的注册所有者 **htb-student** 即为 flag 。

## Question 8
For this level, you must successfully authenticate to the Domain Controller host at 172.16.5.155 via SSH after first authenticating to the target host. This host seems to have several PowerShell modules loaded, and this user's flag is hidden in one of them.
对于此级别，您必须在首次向目标主机进行身份验证后，通过 SSH 在 172.16.5.155 向域控制器主机成功进行身份验证。此主机似乎加载了多个 PowerShell 模块，其中一个模块中隐藏了此用户的 flag 。

---

在登录的终端使用 ssh 登录 AD 域主机 172.16.5.155 。并且使用 Get-Module 查询安装的 powershell 模块。
```powershell
# 原始登录的主机 ssh 登录的密码为上题答案 htb-student
ssh user7@172.16.5.155

# 主机 172.16.5.155
powershell
Get-Module
```
输出
```text
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     0.0        Flag-Finder                         Get-Flag
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PSReadLin...
```
从中可以发现一个叫做 Flag-Finder 的模块，其中有一个 Get-Flag 的命令，运行该命令可以获取 flag。
```text
The  
Flag you are looking for is {Modules_make_pwsh_run!}
```
其中 flag 为 **Modules_make_pwsh_run!**

## Question 9
This flag is the GivenName of a domain user with the Surname "Flag".
此 flag 是 Surname 为“Flag”的域用户的 GivenName。

---

使用 Get-ADUser 命令配合 like 筛选 Surname 为 Flag 的用户。
```powershell
Get-ADUser -Filter 'Surname -like "*Flag*"'
```
输出为
```text
DistinguishedName : CN=Rick F. Flag,CN=Users,DC=greenhorn,DC=corp
Enabled           : False
GivenName         : Rick
Name              : Rick F. Flag
ObjectClass       : user
ObjectGUID        : de3769db-7161-46a0-bf99-cfe766cb8bcd
SamAccountName    : RFlag
SID               : S-1-5-21-1480833693-1324064541-2711030367-1113
Surname           : Flag
UserPrincipalName : RFlag@greenhorn.corp
```
可以获取 GivenName 为 **Rick** 即为 flag。

## Question 10
Use the tasklist command to print running processes and then sort them in reverse order by name. The name of the process that begins with "vm" is the flag for this user.
使用 tasklist 命令打印正在运行的进程，然后按名称按相反顺序对其进行排序。以“vm”开头的进程名称是此用户的 flag 。

---

使用命令 tasklist 之后，使用 Sort-Object 命令排序，并且使用 Where-Object 命令筛选出 "vm" 开头的进程名。
```powershell
tasklist | Sort-Object -Descending | Where-Object { $_ -like "vm*" }
```
输出为
```text
vmtoolsd.exe                  3164 Services                   0     21,668 K
vm3dservice.exe               5036 Console                    1      6,596 K
vm3dservice.exe               3572 Console                    1      6,828 K
vm3dservice.exe               3112 Services                   0      6,336 K
```
可以获取到第一个为 **vmtoolsd.exe** 即为 flag 。

## Question 11
What user account on the Domain Controller has many Event ID (4625) logon failures generated in rapid succession, which is indicative of a password brute forcing attack? The flag is the name of the user account.
域控制器上的哪个用户帐户连续快速生成许多事件ID（4625）登录失败，这表明存在密码暴力攻击？ flag 是用户帐户的名称。

---

首先需要先使用 user10 登录之前的 AD 域主机（172.16.5.155），使用 Get-WinEvent 命令查询 Security 日志中的 4625 事件，并且分组计数其中的登录失败目标账户的用户名。根据查询，返回的事件对象列表中的 Properties 参数的第 5 个为目标用户名。
下列是根据 AI 给出的事件ID为 4625 的 Properties 结构：

| 索引 | 属性名                       | 示例值                                                       |
|----|---------------------------|-----------------------------------------------------------|
| 0  | SubjectUserSid            | S-1-5-18                                                  |
| 1  | SubjectUserName           | SYSTEM                                                    |
| 2  | SubjectDomainName         | NT AUTHORITY                                              |
| 3  | SubjectLogonId            | 0x3e7                                                     |
| 4  | TargetUserSid             | S-1-0-0                                                   |
| 5  | **TargetUserName**        | **[user10@greenhorn.corp](mailto:user10@greenhorn.corp)** |
| 6  | TargetDomainName          | -                                                         |
| 7  | Status                    | 0xc000006d                                                |
| 8  | FailureReason             | %%2313                                                    |
| 9  | SubStatus                 | 0xc000006a                                                |
| 10 | LogonType                 | 3                                                         |
| 11 | LogonProcessName          | NtLmSsp                                                   |
| 12 | AuthenticationPackageName | NTLM                                                      |
| 13 | WorkstationName           | WORKSTATION1                                              |
| 14 | IpAddress                 | 192.168.1.100                                             |
| 15 | IpPort                    | 0                                                         |

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security';Id=4625} | Group-Object {$_.Properties[5].value} | Select-Object Count,Name | Sort-Object Count -Descending
```
返回结果为
```text
Count Name            
----- ----            
  181 justalocaladmin 
   11 Administrator   
    8 htb-student     
    4 user1           
    3 user2           
    2                 
    2 user7           
    1 user6           
    1 FakeUser
```
可以得到最多访问的用户为 **justalocaladmin** 即为 flag 。
