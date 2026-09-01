# Development Notes

## August 11, 2026

- Set up the Windows 11 SOC endpoint on UTM's Host Only Network.
- Configured the Windows endpoint with a static lab IP: 192.168.50.20/24.
- Had a lot of trouble with connectivity between Windows and Kali but was able to figure it out.

## August 16, 2026

- Started learning how Sysmon Event IDs can be correlated instead of looking at each event separately.
- Generated a TCP connection from Windows to the Kali VM using a Python HTTP server.
- Found the connection in Sysmon Event ID 3 and identified the process, PID, destination IP, and port.
- Used the Windows hosts file to map the name kali-lab.local to 192.168.50.10.
- Found the matching Sysmon Event ID 22 DNS query and saw kali-lab.local resolved to 192.168.50.10.
- Was able to connect the DNS query and network connection with the related process using the timestamps and event information.

## August 17, 2026
- Started the first controlled remote execution event using WinRM.
- Had some issues with getting WinRM configured because of the Windows network profile.
- Used Evil-WinRM on Kali to establish a remote PowerShell session to my Windows VM.
- Ran whoami and hostname remotely from Kali.
- Used Sysmon Event ID 1 to trace the remote execution and found the related processes and was able to connect them together through their PIDs and parent process info.

## August 21, 2026
- Finished setting up the Debian 13 VM that will be used as the Wazuh server.
- Configured Debian with the static lab IP 192.168.50.30/24.
- Spent a couple nights troubleshooting VM to VM connection between Kali, Windows, and Debian.
- Used ip addr, ip route, and ip neigh to check Debian's interface, routing, and neighbor state.
- Verified that Kali, Windows, and Debian can now communicate with each other over the isolated 192.168.50.0/24 lab network.
- The core network setup is now finished and I'm ready to start setting up Wazuh on Debian.

## August 23, 2026
- Finished getting the Windows VM connected to the Wazuh server.
- Installed the Wazuh agent on Windows and got Windows-Soc showing as active in the dashboard.
- Tested Wazuh by creating and deleting a temporary Windows account called wazuhtest.
- Found the account activity in Wazuh Threat Hunting and opened the full event details.
- Tried some other tests just to understand Wazuh more.

## August 24, 2026
- Got the Windows SOC agent working again after it disconnected.
- Had some issues with the Wazuh manager going down, but restarted the Wazuh services and got the dashboard working again.
- Used Kali and Evil-WinRM to successfully connect remotely to the Windows SOC VM.
- Ran Get-Process and Get-Service through the remote session.
- Found the matching Windows Security Event ID 4624 in Wazuh.
- The event showed a successful remote login for Yasin from the Kali VM (192.168.50.10) with Logon Type 3.
- Started learning how to read Wazuh events by focusing on the important fields instead of trying to understand every field shown.

## August 29, 2026
- Started a final controlled WinRM investigation using Kali and the Windows-Soc VM.
- Used Evil-WinRM to remotely connect to Windows with the Yasin account.
- Ran whoami to confirm the account being used and then ran systeminfo as a simple host discovery action.
- Found the systeminfo.exe process in Windows Sysmon Event ID 1.
- The event showed PID 5476 and parent PID 9124, with wsmprovhost.exe as the parent process.
- This allowed me to connect the remote WinRM session to the systeminfo.exe process. 
- Also found Windows Security Event ID 4672 showing special privileges assigned to the Yasin logon.
- Had trouble finding the specific systeminfo.exe Event ID 1 in Wazuh even though it was clearly present in Windows Sysmon.
- Finished the main technical work for the home SOC lab and moved on to documenting the investigation.