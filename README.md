# Home SOC Lab

A small home SOC lab I built to learn how endpoint activity can be collected, searched, and investigated using Kali, Windows, Sysmon, and Wazuh.

## Lab Setup

The lab uses three VMs on an isolated UTM Host Only network:

* Kali — attacker machine — `192.168.50.10`
* Windows-Soc — Windows endpoint — `192.168.50.20`
* Debian — Wazuh server — `192.168.50.30`

Kali is used to generate controlled activity against the Windows endpoint. Sysmon records activity on Windows, and the Wazuh agent sends Windows telemetry to the Debian Wazuh server for investigation.

## What I Practiced

* Setting up an isolated multi-VM network
* Configuring and troubleshooting Windows and Linux networking
* Installing and configuring Sysmon and the Wazuh agent
* Searching Windows security events in Wazuh
* Understanding Sysmon process, network, DNS, and file creation events
* Using PIDs, parent processes, timestamps, usernames, and IP addresses to connect events
* Investigating remote access through WinRM and Evil-WinRM

## Investigations

I worked through two main investigations:

1. Windows to Kali network activity, using Sysmon Event IDs 3 and 22 to connect network and DNS activity.
2. Kali to Windows remote access through WinRM, using Windows Security events and Sysmon to trace the remote session.

The investigations and supporting evidence are documented in [investigations.md](notes/investigations.md).

## Notes

* [Lab and Network Setup](notes/lab%20and%20network%20setup.md)
* [Development Notes](notes/progress_notes.md)
* [Investigation Notes](notes/investigations.md)

## Screenshots

Screenshots of the main investigation findings are included in the `screenshots` folder.
