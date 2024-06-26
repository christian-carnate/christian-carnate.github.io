---
title: Building a Virtual Machine (VM) Home Lab
date: 2024-02-01 20:18:30 -0700
categories: [Projects, Virtual Machine (VM) Home Lab]
tags: [projects]
---

Over Christmas break, I decided to build my own virtual machine (VM) home lab in VirtualBox. I used the concepts, configurations, and network design idea from [**Building Virtual Machine Labs: A Hands-on Guide (Second Edition)**](https://leanpub.com/avatar2) by Tony Robinson (of course, with my own modifications as well). Here is a quick rundown of the VMs in my home lab.
- **VirtualBox:** [VirtualBox](https://www.virtualbox.org/) is a free, open-source type 2 hypervisor for Windows, Mac, and Linux operating systems running x86/x64 CPU architectures that support virtualization.
- **pfSense/Suricata:** [pfSense](https://www.pfsense.org/) is a free, open-source firewall based on the FreeBSD operating system. [Suricata](https://suricata.io/) is a free, open-source intrusion detection and prevention system (IDPS) for Linux operating systems. In my home lab, the Suricata instance is an installed package on the pfSense firewall.
- **Kali Linux:** [Kali Linux](https://www.kali.org/) is a free, open-source, Debian-based Linux distribution made for penetration testers and ethical hackers. Essentially, it's the script kiddie's best friend.
- **Ubuntu/Splunk:** [Ubuntu](https://ubuntu.com/) is a free, mostly open-source, and Debian-based Linux distribution that is commonly used for its flexibility. [Splunk](https://www.splunk.com/) is an enterprise SIEM solution. I installed Splunk and configured the free license group as well as integrated pfSense and Suricata logs into the *network* index.
- **Windows Server 2022/Windows 10 Education:** [Windows](https://www.microsoft.com/en-us/windows) is a series of operating systems developed and owned by Microsoft for personal computers and enterprises. I obtained software licenses from [Azure Dev Tools for Teaching](https://azureforeducation.microsoft.com/devtools), which is free because I am a Grand Canyon University student.
- **Fedora Server 39/Fedora KDE Plasma Desktop 39:** [Fedora Linux](https://fedoraproject.org/) is a free, open-source Linux distribution developed by the Fedora Project, which is primarily sponsored by Red Hat. You can think of Fedora Linux as the playground for software development updates for CentOS and Red Hat Enterprise Linux.
- **Flare-VM:** [Flare-VM](https://github.com/mandiant/flare-vm) is an overlay for malware analysis and reverse engineering on Microsoft Windows 10+.
- **SIFT Workstation/REMnux:** [SIFT Workstation](https://www.sans.org/tools/sift-workstation/) is an incident response and digital forensics toolkit for Linux. [REMnux](https://remnux.org/) is a malware analysis and reverse engineering toolkit for Linux. I installed both toolkits on Ubuntu Desktop.

Additionally, my VM home lab is subnetted into three networks.
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

![List of VMs.](/assets/img/article_img/VM_Home_Lab/ListOfVMs.png)
_An overview of the VMs in my home lab._

## VirtualBox

![VM settings.](/assets/img/article_img/VM_Home_Lab/VM_screenshot.png)
_VM settings for my pfSense VM. All VMs have their host computer sharing options disabled._

![Host-only adapter.](/assets/img/article_img/VM_Home_Lab/HostOnlyAdapter.png)
_The host-only adapter connects my host computer to the LAN network._

## pfSense/Suricata

![LAN firewall rules.](/assets/img/article_img/VM_Home_Lab/FirewallRulesLAN.png)
![OPT1 firewall rules.](/assets/img/article_img/VM_Home_Lab/FirewallRulesOPT1.png)
![OPT2 firewall rules.](/assets/img/article_img/VM_Home_Lab/FirewallRulesOPT2.png)
_Firewall rules for the LAN, Sysadmin, and Malware Analysis networks._

![LAN DHCP reservations.](/assets/img/article_img/VM_Home_Lab/LAN_DHCP.png)
![OPT1 DHCP reservations.](/assets/img/article_img/VM_Home_Lab/OPT1_DHCP.png)
![OPT2 DHCP reservations.](/assets/img/article_img/VM_Home_Lab/OPT2_DHCP.png)
_DHCP reservations for the LAN, Sysadmin, and Malware Analysis networks._

![DNS resolver.](/assets/img/article_img/VM_Home_Lab/DNS_screenshot1.png)
![DNS forwarding mode.](/assets/img/article_img/VM_Home_Lab/DNS_screenshot2.png)
![DNS name servers.](/assets/img/article_img/VM_Home_Lab/DNS_screenshot3.png)
_DNS settings for the pfSense VM._

![NTP servers.](/assets/img/article_img/VM_Home_Lab/NTP_settings.png)
_NTP settings for the pfSense VM._

![Suricata interfaces.](/assets/img/article_img/VM_Home_Lab/Suricata_screenshot1.png)
![Suricata alerts.](/assets/img/article_img/VM_Home_Lab/Suricata_screenshot2.png)
_Suricata is enabled on all internal network interfaces. The second screenshot is an example of alerts for the LAN interface. The IDS functionality is enabled for the Sysadmin and Malware Analysis networks as well._

![Remote logging.](/assets/img/article_img/VM_Home_Lab/RemoteLogging.png)
_All firewall filter logs and Suricata logs are sent via syslog to 172.16.1.3:5147, which is the Splunk instance._

## Splunk

![Splunk logs.](/assets/img/article_img/VM_Home_Lab/SplunkIndex.png)
_My Splunk instance receives JSON logs on udp/5147 from the pfSense VM and stores them in the network index._

## REMnux, SIFT Workstation, and Flare VM

![REMnux.](/assets/img/article_img/VM_Home_Lab/REMnuxSIFT.png)
_REMnux and SIFT toolkits installed on the same Ubuntu Desktop._

![FlareVM.](/assets/img/article_img/VM_Home_Lab/FlareVM.png)
_Flare VM installed on Windows 10._

## Future home lab Plans for the Windows 10, Windows Server 2022, Fedora Server 39, and Fedora KDE Plasma Desktop 39 VMs

The rest of the VMs have their operating systems installed. My plan in the future is to set up a small, makeshift "production" environment that includes both Windows and Linux operating systems. I will update this article when I have set up an Active Directory domain for the Windows VMs.

## Changelog

- **02/01/2024:** Added a changelog.
- **02/02/2024:** Fixed grammar and spelling mistakes.
