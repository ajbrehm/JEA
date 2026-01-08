# JEA Configuration Repository

This repository contains Just Enough Administration (JEA) session configurations and role capabilities for securing PowerShell remote access.

## Overview

JEA provides a security layer by restricting what authenticated users can do in PowerShell remoting sessions. Each session configuration (`.pssc` file) is paired with role capability files (`.psrc` files) that define exactly which cmdlets, functions, and external commands are available.

## Session Configurations

### [Dism.pssc](Dism.pssc)
Allows members of the `JEA_Dism` security group to install and remove Windows components and perform housekeeping tasks.

**Available Commands:**
- All `*-WindowsCapability*` cmdlets from the Dism module
- External: `dism.exe`

### [LocalAccounts.pssc](LocalAccounts.pssc)
Allows members of the `JEA_LocalAccounts` security group to create and modify local users and local group memberships.

**Available Commands:**
- All `*-LocalGroup*` cmdlets
- All `*-LocalUser` cmdlets

### [Process.pssc](Process.pssc)
Allows members of the `JEA_Process` security group to query and manage processes.

**Available Commands:**
- `Get-Process`
- `Stop-Process`

### [ScheduledTasks.pssc](ScheduledTasks.pssc)
Allows members of the `JEA_ScheduledTasks` security group to create, modify scheduled tasks, and manage task permissions using user account credentials.

**Available Commands:**
- `Get-ScheduledTask`
- `Get-Help`
- Custom Functions: `Register-RestrictedScheduledTask`, `Get-ScheduledTaskPermissions`, `Claim-ScheduledTask`

### [Service.pssc](Service.pssc)
Allows members of the `JEA_Service` security group to query, start, stop, and restart services.

**Available Commands:**
- `Get-Service`
- `Start-Service`
- `Restart-Service`
- `Stop-Service`

### [SmbShare.pssc](SmbShare.pssc)
Allows members of the `JEA_SmbShare` security group to manage network shares (create, modify, and remove SMB shares).

**Available Commands:**
- All `*-SmbShare*` cmdlets

### [IIS.pssc](IIS.pssc)
Allows members of the `JEA_IIS` security group to manage Internet Information Services (IIS) using the IISAdministration and WebAdministration modules.

**Available Commands:**
- Modules: `IISAdministration`, `WebAdministration` (imported)
- All `*-IIS*` and `*-Web*` cmdlets
- External: `C:\Windows\System32\iisreset.exe`

Recommendation: Install the Web Management Service for remote IIS management.

### [WindowsUpdate.pssc](WindowsUpdate.pssc)
Allows members of the `JEA_WindowsUpdate` security group to manage Windows updates using the PSWindowsUpdate module.

**Available Commands:**
- All `*-WindowsUpdate` and `*-WU*` cmdlets
- `Restart-Computer`
- External: `HOSTNAME.EXE`

## Role Capabilities

Each role capability file in the [RoleCapabilities/](RoleCapabilities/) directory defines the specific commands, modules, and external executables available for a particular role. These files are referenced by the corresponding session configuration files.

## Usage

These configurations are deployed to PowerShell endpoints to enable secure, delegated administration. Connect to a JEA session using:

```powershell
Enter-PSSession -ComputerName <Server> -ConfigurationName <ConfigName>
```

Where `<ConfigName>` is the name of the registered session configuration (e.g., `Dism`, `LocalAccounts`, `Process`, `ScheduledTasks`, `Service`, `SmbShare`, or `WindowsUpdate`).