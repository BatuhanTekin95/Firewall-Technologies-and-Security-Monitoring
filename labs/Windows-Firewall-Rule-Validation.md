# Windows Firewall Rule Validation Lab

**Status:** Ready to run

## Objective

Validate a narrowly scoped inbound rule, collect the related Windows Firewall evidence and return the host to its original state.

## Lab setup

- One Windows 10/11 or Windows Server target
- One client on the same isolated lab network
- Local administrator access on the Windows target
- An elevated PowerShell session
- TCP port `8088` unused before the test

Do not run this procedure on a production endpoint. Record the target and client IP addresses before starting.

## 1. Capture the baseline

Run on the Windows target:

```powershell
$labGroup = "Portfolio-Firewall-Lab"
$baseline = Get-NetFirewallProfile |
    Select-Object Name, LogAllowed, LogBlocked, LogFileName, LogMaxSizeKilobytes

Get-NetConnectionProfile
Get-NetFirewallProfile |
    Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction,
        LogAllowed, LogBlocked, LogFileName

Get-NetTCPConnection -LocalPort 8088 -ErrorAction SilentlyContinue
```

Expected result: no existing listener on TCP `8088`. If the port is in use, choose another high port and update every command consistently.

## 2. Enable temporary logging

```powershell
Set-NetFirewallProfile -Profile Domain,Private,Public `
    -LogAllowed True -LogBlocked True -LogMaxSizeKilobytes 20480
```

The default log path is normally:

```text
%windir%\System32\LogFiles\Firewall\pfirewall.log
```

## 3. Start a temporary listener

Keep this PowerShell window open on the target:

```powershell
$listener = [System.Net.Sockets.TcpListener]::new(
    [System.Net.IPAddress]::Any,
    8088
)
$listener.Start()
$listener.LocalEndpoint
```

## 4. Create and validate an allow rule

In a second elevated PowerShell window on the target:

```powershell
New-NetFirewallRule `
    -DisplayName "Lab - Allow TCP 8088" `
    -Group $labGroup `
    -Direction Inbound `
    -Action Allow `
    -Protocol TCP `
    -LocalPort 8088 `
    -Profile Any
```

From the client:

```powershell
Test-NetConnection -ComputerName TARGET_IP -Port 8088 -InformationLevel Detailed
```

Record:

- Test time and timezone
- Client and target IP addresses
- `TcpTestSucceeded` result
- Rule name and active Windows network profile
- Matching firewall log line, if recorded

## 5. Replace it with a block rule

Run on the target:

```powershell
Get-NetFirewallRule -Group $labGroup | Remove-NetFirewallRule

New-NetFirewallRule `
    -DisplayName "Lab - Block TCP 8088" `
    -Group $labGroup `
    -Direction Inbound `
    -Action Block `
    -Protocol TCP `
    -LocalPort 8088 `
    -Profile Any
```

Repeat the client test. The expected result is `TcpTestSucceeded : False`.

## 6. Review evidence

On the target:

```powershell
Get-NetFirewallRule -Group $labGroup |
    Select-Object DisplayName, Enabled, Direction, Action, Profile

Get-Content "$env:windir\System32\LogFiles\Firewall\pfirewall.log" -Tail 100
```

Look for the test timestamp, protocol, source IP, destination IP, destination port and action. If no matching event appears, confirm the active profile, log path, service state and whether another control handled the traffic.

## 7. Cleanup and rollback

Stop the listener in its PowerShell window:

```powershell
$listener.Stop()
```

Remove all rules created by this lab and restore the saved logging settings:

```powershell
Get-NetFirewallRule -Group $labGroup -ErrorAction SilentlyContinue |
    Remove-NetFirewallRule

foreach ($profile in $baseline) {
    Set-NetFirewallProfile `
        -Profile $profile.Name `
        -LogAllowed $profile.LogAllowed `
        -LogBlocked $profile.LogBlocked `
        -LogFileName $profile.LogFileName `
        -LogMaxSizeKilobytes $profile.LogMaxSizeKilobytes
}

Get-NetFirewallRule -Group $labGroup -ErrorAction SilentlyContinue
Get-NetTCPConnection -LocalPort 8088 -ErrorAction SilentlyContinue
```

Expected result: neither a lab rule nor a listener remains.

## Evidence template

| Item | Observation |
| --- | --- |
| Lab date and timezone | Not recorded yet |
| Target / client | Not recorded yet |
| Active firewall profile | Not recorded yet |
| Allow test result | Not recorded yet |
| Block test result | Not recorded yet |
| Matching log evidence | Not recorded yet |
| Cleanup verified | Not recorded yet |

I will replace these placeholders with my own results when I run the lab. Until then, this document describes the test procedure rather than claiming a completed investigation.

## References

- [Microsoft: New-NetFirewallRule](https://learn.microsoft.com/en-us/powershell/module/netsecurity/new-netfirewallrule)
- [Microsoft: Set-NetFirewallProfile](https://learn.microsoft.com/en-us/powershell/module/netsecurity/set-netfirewallprofile)
- [Microsoft: Configure Windows Firewall logging](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure-logging)
