---
title: Building a Virtual Machine (VM) Homelab
date: 2024-02-01 20:18:30 -0700
categories: [Projects, Virtualization]
tags: [projects, virtualization, VirtualBox]
---

Over Christmas break, I decided to build my own virtual machine (VM) homelab in VirtualBox. I used the concepts, configurations, and network design idea from [**Building Virtual Machine Labs: A Hands-on Guide (Second Edition)**](https://leanpub.com/avatar2) by Tony Robinson (of course, with my own modifications as well). Here is a quick rundown of the VMs in my homelab.
- **VirtualBox:** [VirtualBox](https://www.virtualbox.org/) is a free, open-source type 2 hypervisor for Windows, Mac, and Linux operating systems running x86/x64 CPU architectures that support virtualization.
- **pfSense/Suricata:** [pfSense](https://www.pfsense.org/) is a free, open-source firewall based on the FreeBSD operating system. [Suricata](https://suricata.io/) is a free, open-source intrusion detection and prevention system (IDPS) for Linux operating systems. In my homelab, the Suricata instance is an installed package on the pfSense firewall.
- **Kali Linux:** [Kali Linux](https://www.kali.org/) is a free, open-source, Debian-based Linux distribution made for penetration testers and ethical hackers. Essentially, it's the script kiddie's best friend.
- **Ubuntu/Splunk:** [Ubuntu](https://ubuntu.com/) is a free, mostly open-source, and Debian-based Linux distribution that is commonly used for its flexibility. [Splunk](https://www.splunk.com/) is an enterprise SIEM solution. I installed Splunk and configured the free license group as well as integrated pfSense and Suricata logs into the *network* index.
- **Windows Server 2022/Windows 10 Education:** [Windows](https://www.microsoft.com/en-us/windows) is a series of operating systems developed and owned by Microsoft for personal computers and enterprises. I obtained software licenses from [Azure Dev Tools for Teaching](https://azureforeducation.microsoft.com/devtools), which is free because I am a Grand Canyon University student.
- **Fedora Server 39/Fedora KDE Plasma Desktop 39:** [Fedora Linux](https://fedoraproject.org/) is a free, open-source Linux distribution developed by the Fedora Project, which is primarily sponsored by Red Hat. You can think of Fedora Linux as the playground for software development updates for CentOS and Red Hat Enterprise Linux.
- **Flare-VM:** [Flare-VM](https://github.com/mandiant/flare-vm) is an overlay for malware analysis and reverse engineering on Microsoft Windows 10+.
- **SIFT Workstation/REMnux:** [SIFT Workstation](https://www.sans.org/tools/sift-workstation/) is an incident response and digital forensics toolkit for Linux. [REMnux](https://remnux.org/) is a malware analysis and reverse engineering toolkit for Linux. I installed both toolkits on Ubuntu Desktop.

Additionally, my VM homelab is subnetted into three networks.
- **172.16.1.0/24:** Local Area Network (LAN)
  - pfSense/Suricata (LAN interface/172.16.1.1)
  - VirtualBox Host-Only Ethernet Adapter (172.16.1.2)
  - Ubuntu/Splunk (172.16.1.3)
  - Kali Linux (172.16.1.4)
- **172.16.2.0/24:** Sysadmin Network
  - pfSense/Suricata (OPT1 interface/172.16.2.1)
  - Windows Server 2022 (172.16.2.2)
  - Windows 10 Education (172.16.2.3)
  - Fedora Server 39 (172.16.2.4)
  - Fedora KDE Plasma Desktop 39 (172.16.2.5)
- **172.16.3.0/24:** Malware Analysis Network
  - pfSense/Suricata (OPT2 interface/172.16.3.1)
  - Flare-VM (172.16.3.2)
  - SIFT Workstation/REMnux (172.16.3.3)

![List of VMs.](/images/VM_Homelab/ListOfVMs.png)
_An overview of the VMs in my homelab._

## VirtualBox

![VM settings.](/images/VM_Homelab/VM_screenshot.png)
_VM settings for my pfSense VM. All VMs have thier host computer sharing options disabled._

![Host-only adapter.](/images/VM_Homelab/HostOnlyAdapter.png)
_The host-only adapter connects my host computer to the LAN network._

## pfSense/Suricata

![LAN firewall rules.](/images/VM_Homelab/FirewallRulesLAN.png)
![OPT1 firewall rules.](/images/VM_Homelab/FirewallRulesOPT1.png)
![OPT2 firewall rules.](/images/VM_Homelab/FirewallRulesOPT2.png)
_Firewall rules for the LAN, Sysadmin, and Malware Analysis networks._

![LAN DHCP reservations.](/images/VM_Homelab/LAN_DHCP.png)
![OPT1 DHCP reservations.](/images/VM_Homelab/OPT1_DHCP.png)
![OPT2 DHCP reservations.](/images/VM_Homelab/OPT2_DHCP.png)
_DHCP reservations for the LAN, Sysadmin, and Malware Analysis networks._

![DNS resolver.](/images/VM_Homelab/DNS_screenshot1.png)
![DNS forwarding mode.](/images/VM_Homelab/DNS_screenshot2.png)
![DNS name servers.](/images/VM_Homelab/DNS_screenshot3.png)
_DNS settings for the pfSense VM._

![NTP servers.](/images/VM_Homelab/NTP_settings.png)
_NTP settings for the pfSense VM._

![Suricata interfaces.](/images/VM_Homelab/Suricata_screenshot1.png)
![Suricata alerts.](/images/VM_Homelab/Suricata_screenshot2.png)
_Suricata is enabled on all internal network interfaces. The second screenshot is an example of alerts for the LAN interface. The IDS functionality is enabled for the Sysadmin and Malware Analysis networks as well._

![Remote logging.](/images/VM_Homelab/RemoteLogging.png)
_All firewall filter logs and Suricata logs are sent via syslog to 172.16.1.3:5147, which is the Splunk instance._

## Splunk

![Splunk logs.](/images/VM_Homelab/SplunkIndex.png)
_My Splunk instance receives JSON logs on udp/5147 from the pfSense VM and stores them in the network index._

## REMnux, SIFT Workstation, and Flare VM

![REMnux.](/images/VM_Homelab/REMnuxSIFT.png)
_REMnux and SIFT toolkits installed on the same Ubuntu Desktop._

![FlareVM.](/images/VM_Homelab/FlareVM.png)
_Flare VM installed on Windows 10._

## Future Homelab Plans for the Windows 10, Windows Server 2022, Fedora Server 39, and Fedora KDE Plasma Desktop 39 DMs

The rest of the VMs have their operating systems installed. My plan in the future is to set up a small, makeshift "production" environment that includes both Windows and Linux operating systems. I will update this article when I have set up an Active Directory domain for the Windows VMs.

## Changelog

- **02/01/2024:** Added a changelog.
- **02/02/2024:** Fixed grammar and spelling mistakes.
