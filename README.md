# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I constructed a small-scale honeynet within Azure and integrated log data from various sources into a Log Analytics workspace. This setup feeds into Microsoft Sentinel, enabling the creation of attack maps, initiation of alerts, and generation of incidents. Over an initial 24-hour period, I recorded security metrics in a less secure environment. Following this, I implemented a series of security enhancements to fortify the environment. Post-hardening, I monitored and recorded metrics for another 24 hours. The comparative results of these two phases are presented below.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially set up with direct exposure to the internet. The Virtual Machines had their Network Security Groups and in-built firewalls configured to allow unrestricted access. Additionally, all other resources were deployed with public endpoints, making them fully visible online without any use of Private Endpoints.


In the "AFTER" phase, the security stance was significantly tightened. Network Security Groups were adjusted to block all incoming traffic, except for connections from my administrative workstation. Furthermore, all resources were safeguarded using their inherent firewalls, complemented by the implementation of Private Endpoints for enhanced security.

## Attack Maps Before Hardening / Security Controls
<img width="1231" alt="nsg-malicious-allowed-in before" src="https://github.com/marcoasmith/Cloud-SOC/assets/155500497/2c3c052c-0f70-4712-9e0a-8d488eafb862">
<br>
<img width="1399" alt="linux-ssh-auth-fail before" src="https://github.com/marcoasmith/Cloud-SOC/assets/155500497/d1a5e430-f084-48cd-8f7d-016ccda9c1a8">
<br>
<img width="1095" alt="windows-rdp-auth-fail before" src="https://github.com/marcoasmith/Cloud-SOC/assets/155500497/43f6a5b0-d55f-4ca1-8bd7-6f6cdc491751">
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 11/30/2023 21:10:46
Stop Time 12/1/2023 21:10:46

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 76196
| Syslog                   | 14859
| SecurityAlert            | 3
| SecurityIncident         | 273
| AzureNetworkAnalytics_CL | 32488

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 12/6/2023 21:35:08
Stop Time	12/7/2023 21:35:08

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3285
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

This project involved developing a small honeynet within Microsoft Azure, with log data from various sources being fed into a Log Analytics workspace. Utilizing Microsoft Sentinel, this setup facilitated the triggering of alerts and the generation of incidents based on the analyzed logs. Key metrics were recorded in the initial less-secure environment prior to the application of security controls. After implementing these security measures, the metrics were recorded again. It's significant to note that there was a marked decrease in the number of security events and incidents following the application of the security controls, showcasing their efficacy.

It should be highlighted that in a scenario where the network resources were extensively used by everyday users, there's a possibility that a higher number of security events and alerts could have been produced during the 24-hour period after the security controls were put into place.
