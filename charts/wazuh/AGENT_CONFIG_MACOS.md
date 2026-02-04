# Recommended Centralized Configuration for macOS Agents

To ensure the rules in `rules.FIM-MacOS.xml` are triggered correctly, add the following configuration block to your `agent.conf` (Centralized Configuration) for the relevant macOS agent group.

```xml
<agent_config os="Darwin">
  <!-- Log Analysis: Monitors the system log for kernel, SIP, keychain, and login events -->
  <localfile>
    <location>/var/log/system.log</location>
    <log_format>syslog</log_format>
  </localfile>

  <!-- File Integrity Monitoring (FIM): Monitors critical system files and directories -->
  <syscheck>
    <disabled>no</disabled>
    <frequency>43200</frequency>
    <scan_on_start>yes</scan_on_start>

    <!-- Monitor critical system paths -->
    <directories check_all="yes" realtime="yes">/etc,/usr/bin,/usr/sbin</directories>
    <directories check_all="yes" realtime="yes">/bin,/sbin</directories>
    <directories check_all="yes" realtime="yes">/Library/LaunchAgents,/Library/LaunchDaemons</directories>
    <directories check_all="yes" realtime="yes">/System/Library/LaunchAgents,/System/Library/LaunchDaemons</directories>
    <directories check_all="yes" realtime="yes">/Library/Extensions,/System/Library/Extensions</directories>

    <!-- Explicitly watch sudoers and ssh configs -->
    <directories check_all="yes" realtime="yes">/private/etc/sudoers,/private/etc/sudoers.d</directories>
    <directories check_all="yes" realtime="yes">/private/etc/ssh</directories>

    <skip_nfs>yes</skip_nfs>
  </syscheck>
</agent_config>
```

## How to Apply
1. Go to **Wazuh Dashboard** -> **Management** -> **Groups**.
2. Select the group containing your macOS agents.
3. Click on **Edit Group Configuration**.
4. Paste the XML block above.
5. Click **Save**. The manager will validate and push the configuration to the agents.
