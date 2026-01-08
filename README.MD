# JEA Configuration Repository

This repository contains Just Enough Administration (JEA) session configurations and role capabilities for securing PowerShell remote access.

## Overview

JEA provides a security layer by restricting what authenticated users can do in PowerShell remoting sessions. Each session configuration (`.pssc` file) is paired with role capability files (`.psrc` files) that define exactly which cmdlets, functions, and external commands are available.

## Session Configurations

### [WindowsUpdate.pssc](WindowsUpdate.pssc)
Allows members of the `JEA_WindowsUpdate` security group to manage Windows updates using the PSWindowsUpdate module. Users can run Windows Update cmdlets and restart the computer.

**Available Commands:**
- All `*-WindowsUpdate` and `*-WU*` cmdlets
- `Restart-Computer`
- External: `HOSTNAME.EXE`

### [SmbShare.pssc](SmbShare.pssc)
Allows members of the `JEA_SmbShare` security group to manage network shares (create, modify, and remove SMB shares).

**Available Commands:**
- All `*-SmbShare*` cmdlets

### [Service.pssc](Service.pssc)
Allows members of the `JEA_Service` security group to query, start, stop, and restart services.

**Available Commands:**
- `Get-Service`
- `Start-Service`
- `Restart-Service`
- `Stop-Service`

## Role Capabilities

Each role capability file in the [RoleCapabilities/](RoleCapabilities/) directory defines the specific commands, modules, and external executables available for a particular role. These files are referenced by the corresponding session configuration files.

## Usage

These configurations are deployed to PowerShell endpoints to enable secure, delegated administration. Connect to a JEA session using:

```powershell
Enter-PSSession -ComputerName <Server> -ConfigurationName <ConfigName>
```

Where `<ConfigName>` is the name of the registered session configuration (e.g., `WindowsUpdate`, `SmbShare`, or `Service`).