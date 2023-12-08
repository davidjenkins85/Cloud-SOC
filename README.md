# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/davidjenkins85/Cloud-SOC/assets/150871072/31a38018-14df-40d3-8c82-178b77a865a6)

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, then measured metrics for another 24 hours. My results are below. The metrics displayed are:

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

In the "BEFORE" metrics scenario, all resources were initially deployed and made accessible on the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other deployed resources had public endpoints exposed to the internet, rendering Private Endpoints unnecessary

In the "AFTER" metrics context, Network Security Groups were strengthened by blocking ALL traffic except for my admin workstation, and additional protection measures were implemented for other resources through their built-in firewalls and the use of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
NSG Allowed Inbound Malicious Flows![image](https://github.com/davidjenkins85/Cloud-SOC/assets/150871072/96229d79-108d-496b-9843-fb1951d4d415)
<br>
Linux Syslog Auth Failures![image](https://github.com/davidjenkins85/Cloud-SOC/assets/150871072/e9141db0-0850-419c-b6eb-9a309a11d6cf)
<br>
Windows RDP/SMB Auth Failures![image](https://github.com/davidjenkins85/Cloud-SOC/assets/150871072/044505e4-4125-414b-be24-7a5d2c17c32d)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-19 20:38
Stop Time 2023-11-20 20:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 46258
| Syslog                   | 79
| SecurityAlert            | 13
| SecurityIncident         | 108
| AzureNetworkAnalytics_CL | 1939

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-28 17:30
Stop Time	2023-03-19 17:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10382
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a compact honeynet was established in Microsoft Azure, and log sources were incorporated into a Log Analytics workspace. Microsoft Sentinel was utilized to initiate alerts and generate incidents based on the ingested logs. Furthermore, metrics were assessed in the insecure environment both before the implementation of security controls and afterward. Notably, the number of security events and incidents exhibited a significant decrease post the application of security measures, underscoring their efficacy.

It's important to highlight that if the network's resources were extensively utilized by regular users, there might have been a likelihood of generating more security events and alerts within the 24-hour period following the implementation of security controls.
