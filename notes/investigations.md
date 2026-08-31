# Investigations

## Windows to Kali Network Activity

I started a Python HTTP server on Kali and connected to it from the Windows VM. Sysmon Event ID 3 showed the network connection and gave me the process, PID, destination IP, and port.

I also added kali-lab.local to the Windows hosts file and pointed it to 192.168.50.10. Sysmon Event ID 22 then showed the DNS query for kali-lab.local. I was able to match the DNS and network events using the timestamps and other information.

## WinRM Remote Access

I used Evil-WinRM from Kali to connect to the Windows VM and ran whoami to confirm the account. I then ran systeminfo from the remote session.

Windows Security Event ID 4624 showed the remote logon with Logon Type 3. I also found Event ID 4672 for the Yasin account.

Sysmon Event ID 1 showed systeminfo.exe being created with PID 5476. The parent process was wsmprovhost.exe with PID 9124, which connected the process back to the WinRM session.

I also found Sysmon Event ID 11 showing wsmprovhost.exe creating a temporary PowerShell policy-test file.

I could see the systeminfo.exe Event ID 1 directly in Sysmon but could not find that exact event in Wazuh. Other related events were showing up in Wazuh.
