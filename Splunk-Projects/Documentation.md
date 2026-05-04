Project: Distributed SIEM Integration (Splunk Enterprise)
🛡️ Home Lab: Windows-to-macOS Telemetry Pipeline

=== 1. Project Overview ===
This project demonstrates the deployment and configuration of a distributed Splunk Enterprise environment. The goal was to establish a persistent logging pipeline between a remote Windows 11 endpoint and a centralized macOS Indexer to monitor system behavior and security events in real-time.


=== 2. Architecture & Components ===
> Indexer / Search Head: macOS (Splunk Enterprise)
> Endpoint / Forwarder: Windows 11 (Splunk Universal Forwarder)
> Protocol: TCP Port 9997 (Unencrypted)
> Data Volume: ~80k+ events (baseline)



=== 3. Implementation Steps ===

--- A. Indexer Configuration (macOS) ---
1.  Installed Splunk Enterprise to act as the central management hub.
2.  Configured a Listening Port on `9997` to receive incoming streams from the network.
3.  Created a custom index: `endpoint_windows` to ensure data isolation and retention management.

--- B. Forwarder Deployment (Windows) ---
1.  Installed the Splunk Universal Forwarder (UF) on the Windows 11 source.
2.  Defined the target output (Mac IP) via `outputs.conf`.
3.  Manual Overrides: Due to version-specific CLI deprecations, manually configured the `inputs.conf` stanzas to capture:
    - `WinEventLog://Security`
    - `WinEventLog://System`
    - `WinEventLog://Application`


=== 4. Technical Challenges & Solutions ===

Challenge -> Resolution 

- CLI Deprecation -> The `add monitor` command was deprecated for Event Logs. Shifted to manual stanza management in `etc/system/local/inputs.conf`. 

- Firewall Blocks -> Port 9997 was initially blocked on the Mac. Configured local firewall rules to whitelist the Windows endpoint IP. 

- Event Noise -> High volume of EventCodes `4907` and `5379`. Identified as normal system noise and implemented Splunk filters for search optimization. 


=== 5. Future Roadmap ===
- [ ] Sysmon Integration: Deploying Microsoft Sysinternals Sysmon for deeper process-level visibility.
- [ ] Alerting: Building a correlation search to detect "Brute Force" patterns (EventCode 4625).


=== 6. Tools Used ===
- Splunk Enterprise & Universal Forwarder
- Windows Event Viewer
- macOS Terminal (zsh)
- Microsoft Security+ Theory & Implementation