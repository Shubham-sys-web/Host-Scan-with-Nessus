# **Host Scanning with Nessus**

![](https://miro.medium.com/v2/resize:fit:512/1*mnbiEMGJacgiP4J4htp2Aw.jpeg)

**first Download vulnerable target machine link is below;**

[**Windows XP Original (x86-x64) MSDN ISO Files - SP0-SP1-SP2-SP3 - (EN-DE-RU-TR) : Original Files…Windows XP Original 32/64bit MSDN ISO Files SP0, SP1, SP2, SP3 - (English, German, Russian,...**
archive.org](https://archive.org/details/windows-xp-all-sp-msdn-iso-files-en-de-ru-tr-x86-x64?source=post_page-----93fa7b776d1f---------------------------------------)

Setup in Vmware this target machine

![](https://miro.medium.com/v2/resize:fit:700/1*oBVuR_fSfbWckZEEL9bRnA.png)

**open cmd prompt and run ipconfig to see our target ip and disable firewall, and enable remote desktop and smb**

![](https://miro.medium.com/v2/resize:fit:700/1*7EzuQze1-qsyH6LJ9QmyTg.png)

then go to the nessus and click on new basic scan without password

![](https://miro.medium.com/v2/resize:fit:700/1*SP2ZHtuxFoItazOOrYjJAA.png)

f

![](https://miro.medium.com/v2/resize:fit:700/1*M-XVz3KSeGTqVOekXAox_A.png)

You can see the result

![](https://miro.medium.com/v2/resize:fit:700/1*E_lAOfR-h9ZzpDiZshx0BQ.png)

Then I do Advance scan with username and with password

first enable and disable these things to more depth analysis and check vulnerability

# **🧠 Enable Remote Registry Access**

```
sc config RemoteRegistry start= auto
net start RemoteRegistry
```

Allows remote tools (like Nessus or Metasploit) to access Windows Registry.

# **🧩 Enable DCOM (Distributed COM)**

```
sc config DcomLaunch start= auto
net start DcomLaunch
```

Required for WMI or RPC-based remote access.

# **🔥 Disable Windows Firewall**

```
netsh firewall set opmode disable
```

Disables the built-in firewall to allow all connections.

# **🔓 Allow Null Sessions**

```
reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" /v RestrictNullSessAccess /t REG_DWORD /d 0 /f
```

Enables null SMB sessions (used by enumeration tools).

# **👤 Allow Anonymous Access**

```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v RestrictAnonymous /t REG_DWORD /d 0 /f
```

Allows unauthenticated users to access system info.

# **🔑 Allow Blank Password Logins**

```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v LimitBlankPasswordUse /t REG_DWORD /d 0 /f
```

Disables the restriction that blocks blank password logins.

# **📁 Share Hidden Admin Drives with Full Access**

```
net share Admin$=C:\Windows /GRANT:Everyone,FULL
net share C$=C:\ /GRANT:Everyone,FULL
net share IPC$ /GRANT:Everyone,FULL
```

Shares Admin$, C$, and IPC$ with full permissions — important for remote shell or exploits.

# **👨‍💻 Create Admin Backdoor User**

```
net user testuser "" /add
net localgroup administrators testuser /add
```

Creates a user `testuser` with no password and adds to **Administrators**.

# **🔐 Set Weak Password**

```
net user testuser 123456
```

Sets password `123` to make scanning and login easier.

# **🖥️ Enable RDP (Remote Desktop)**

```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

Allows Remote Desktop connections to the system.

🧰 Enable Remote Admin Access Through Firewall

```
netsh firewall set service remoteadmin enable
```

Enables required ports for remote admin tools like RDP and WMI.

# **🔁 Restart System to Apply Changes**

```
shutdown -r -t 0
```

Reboots system instantly to activate all configurations.

**I start advance scan**

![](https://miro.medium.com/v2/resize:fit:700/1*WSZ0B2YwfmhwoKdPHB7PfA.png)

the set username and password

![](https://miro.medium.com/v2/resize:fit:700/1*6yYZ4slTjUA4g7eJ1Zv1Bw.png)

then run after finish you can see the more vulnerability than before

![](https://miro.medium.com/v2/resize:fit:700/1*OnzrYBj_DLdz_xpPokDIzg.png)

You can see the here **CVSS**(Common Vulnerability Scoring System)

![](https://miro.medium.com/v2/resize:fit:700/1*HoMMupGyDHVzqJR1HMfWkQ.png)

**You can also see the remediation**

![](https://miro.medium.com/v2/resize:fit:700/1*o2y1ojcO9YK30M2aApzsUA.png)

If you comparison in both situation you can see the huge difference between them;

**basic scan total vulnerability is 23**

![](https://miro.medium.com/v2/resize:fit:700/1*GYwl7TOhwLoPA4dXQiaLUQ.png)

**In advance scan total vulnerability is 28**

![](https://miro.medium.com/v2/resize:fit:700/1*bv7h2xi_-YpfurfNKN1nGw.png)

You can export as this pdf,xml,csv format and you see the clearly critical vulnerability and submit the report your organization as a detailed pdf format
