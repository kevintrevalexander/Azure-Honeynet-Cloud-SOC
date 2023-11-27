# Building a SOC + Honeynet in Azure (Live Traffic)
![Azure Honeynet Cloud SOC KTA-Infographic-01](https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/7617cdf4-7eda-4b76-828c-c72664da8501)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening KTA-Infographic-02](https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/e2b4b0e0-7225-40e9-b510-cab21d3c6fa8)

## Architecture After Hardening / Security Controls
![Architecture After Hardening KTA-Infographic-03](https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/41b70d45-156f-4938-a640-bdd00f4a994e)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<img width="1175" alt="linux-ssh-auth-fail (before)" src="https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/2d28c8c1-c83a-4977-8835-67a51b6633f2">
<img width="981" alt="mssql-auth-fail (before)" src="https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/73731fc5-7529-44dd-8dec-27f4b60f0ffa">
<img width="1198" alt="nsg-malicious-allowed-in (before)" src="https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/c2504b0a-fbad-4005-b612-357e4ed312d2">
<img width="960" alt="windows-rdp-auth-fail (before)" src="https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/27bc5fb1-877d-4b74-afde-1dd86ea2c934">

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-09 20:57:43
Stop Time 2023-11-10 20:57:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15787
| Syslog                   | 17590
| SecurityAlert            | 280
| SecurityIncident         | 279
| AzureNetworkAnalytics_CL | 2419

![Metrics Before Hardening KTA-Infographic-04](https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/8aee3e72-c447-4b33-b8c3-8bd6dcc3d03a)

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-10 21:59:35
Stop Time 2023-11-11 21:59:35

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3016
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

![Metrics After Hardening KTA-Infographic-05 jpg](https://github.com/kevintrevalexander/Azure-Honeynet-Cloud-SOC/assets/150730972/301bcc94-da78-4a35-a966-3fb2e811b9d5)

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
