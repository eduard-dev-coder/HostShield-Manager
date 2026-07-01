# HostShield Manager

HostShield Manager is a modern Windows desktop application built with **WinUI 3** for managing the **Windows HOSTS file**, domain block lists, whitelist rules, firewall blocking, Secure DNS / DoH, diagnostics, profiles, logs, and network activity from one interface.

It is designed for users who want more control over domain blocking and local protection without manually editing system files or running scattered Windows tools.

## Highlights

- Built for Windows with **WinUI 3** and the Windows App SDK.
- Edits and protects the Windows `hosts` file.
- Uses a portable application layout with local settings, logs, backups, profiles, language files, and HOSTS category files.
- Includes a native lightweight launcher with splash loading.
- Supports English and Romanian translations.
- Logs important actions and errors for troubleshooting.
- Uses administrator-aware workflows for operations that modify Windows system settings.

## Main Features

### Hosts Editor

The Hosts Editor is the main workspace for managing HOSTS lists and Windows HOSTS entries.

- View and edit the active Windows HOSTS file.
- Manage category files stored in the `Hosts file` folder.
- Add, edit, delete, copy, and insert domain lines.
- Add and edit comments.
- Group domain lines under comments.
- Collapse and expand comment groups.
- Move comment groups up or down together with their domain lines.
- Highlight comments and invalid HOSTS lines.
- Detect invalid IP-as-domain entries, wildcard/path entries, duplicate domains, and malformed lines.
- Validate and repair the selected list.
- Block or unblock entire categories.
- Block a single domain from the context menu.
- Add domains to whitelist.
- Open Windows HOSTS or the current list in Notepad.
- Import domain lists from local files or URLs.
- Save the current Windows HOSTS file as a reusable profile.
- Run domain diagnostics directly from a domain context menu.

### Domain Diagnostic

Domain Diagnostic helps explain what is happening with a selected domain.

- DNS resolution check.
- HTTP check.
- HTTPS port check.
- SSL certificate check.
- Windows HOSTS status.
- Whitelist status.
- Category/list presence.
- Ping result when available.
- Optional Whois lookup.
- Copy domain and copy diagnostic report actions.
- Clear summary and details designed for normal users, not only advanced users.

### Profiles

Profiles let you save and restore complete Windows HOSTS states.

- Save the current Windows HOSTS file as a profile.
- Import and export profile files.
- Open profiles in Notepad.
- Load a profile to replace the current Windows HOSTS content.
- Delete custom profiles.
- Built-in `Default Windows Hosts` profile.
- Built-in `Recommended Windows Hosts` baseline.

Loading a profile replaces the Windows HOSTS file content. HSM creates backups for safety.

### Quick Actions

Quick Actions provides fast tools for common tasks.

- Block one or more domains quickly.
- Add domains to whitelist.
- Add manual HOSTS entries.
- Run domain diagnostics.
- Access maintenance actions that are useful without opening multiple tabs.

### Firewall Blocker

Firewall Blocker creates and removes Windows Firewall rules for selected applications.

- Browse for a folder and scan automatically.
- Paste or type a folder path and auto-scan valid targets.
- Scan subfolders.
- Select detected files.
- Block selected files with inbound and outbound Windows Firewall rules.
- Unblock selected files previously handled by HSM.
- Include extra extensions such as installers or scripts.
- Select all, deselect all, and add custom extensions.
- Open Windows Firewall management console.
- Built-in page logs for scan/block/unblock operations.

Administrator privileges are required to change Windows Firewall rules.

### Secure DNS / DoH

Secure DNS / DoH helps inspect and change DNS settings.

- Show DNS encryption status.
- Show IPv4 DoH and IPv6 DoH status.
- Show internet status.
- Show active network adapter, gateway, DNS servers, IPv4, IPv6, and Wi-Fi SSID when available.
- Apply recommended DNS providers.
- Apply manual DNS servers.
- Reset DNS to automatic DHCP/router/ISP behavior.
- Open the Windows network adapters panel.
- Log DNS actions and failures.

Administrator privileges may be required to apply DNS changes.

### Netstat Monitor

Netstat Monitor shows active local network connections using Windows networking APIs.

- View TCP, TCP6, UDP, and UDP6 connections.
- Show process name, PID, protocol, local address, remote address, state, path, and country info when available.
- GeoIP lookup support through local CSV databases.
- Copy connection details.
- Kill non-system processes from the context menu.
- Block executable paths through Firewall Blocker integration.

### Logs

The Logs page helps troubleshoot and review activity.

- View application events.
- Search logs.
- Filter by event level.
- Copy selected rows.
- Delete selected rows.
- Clear log history.
- Open the current log file.

### Settings

Settings contains user-facing behavior controls.

- Run self-check on startup.
- Warn before blocking whitelisted domains.
- Create snapshots before HOSTS changes.
- Ask before deleting HOSTS lines or categories.
- Reset saved main window size.
- Reset saved secondary dialog sizes.
- Run setup/maintenance checks.

Settings are stored in `user.json`.

## Portable Folder Layout

The portable layout is generated in the `App` folder after build.

Expected runtime layout:

```text
HSM.exe
Bin/
Language/
Hosts file/
Profiles/
Logs/
Backup/
Whitelist.txt
user.json
```

Folder purpose:

- `Bin` - main WinUI application files and runtime dependencies.
- `Language` - translation files such as `en.json` and `ro.json`.
- `Hosts file` - editable HOSTS category/list files.
- `Profiles` - saved Windows HOSTS profiles.
- `Logs` - application and launcher logs.
- `Backup` - HOSTS backups created before sensitive operations.
- `Whitelist.txt` - trusted domains that should not be blocked accidentally.
- `user.json` - user settings such as language, theme, window sizes, and safety options.

## Requirements

For users:

- Windows 10 version 1809 or newer.
- Administrator privileges for HOSTS, DNS, and Firewall changes.

For developers:

- .NET 8 SDK.
- Visual Studio or Visual Studio Build Tools with C++ x64 tools.
- Windows App SDK dependencies restored through NuGet.

## Build

From the repository root:

```powershell
dotnet restore
dotnet build HSM.csproj -c Debug -p:Platform=x64
```

The build also creates the portable layout in:

```text
App/
```

The native launcher is built from:

```text
NativeLauncher/
```

using:

```text
Tools/Build-NativeLauncher.ps1
```

The portable layout is assembled by:

```text
Tools/Create-PortableLayout.ps1
```

## Development Structure

```text
Models/        Data models used by views and services
Services/      Application services for HOSTS, firewall, DNS, logs, profiles, localization
ViewModels/    View models for app state and page logic
Views/         WinUI pages and dialogs
Converters/    UI value converters
graphic/       Application graphics and icons
language/      Translation JSON files
NativeLauncher Native C++ launcher and splash screen
Tools/         Build and packaging scripts
```

## Safety Notes

HostShield Manager can modify sensitive Windows configuration:

- Windows HOSTS file.
- Windows Firewall rules.
- DNS client settings.

Use backups before large changes. HSM creates backups for many HOSTS operations, but users should still review imported lists and profiles before applying them.

## Logging

HSM logs actions and errors to the `Logs` folder. Logs are useful for:

- HOSTS write failures.
- DNS command failures.
- Firewall scan/block/unblock results.
- Profile import/export/load issues.
- Application startup or crash diagnostics.

## Localization

The application currently includes:

- English: `Language/en.json`
- Romanian: `Language/ro.json`

Language files are loaded from the portable `Language` folder. If required language files are missing or invalid, HSM attempts to repair them from bundled copies.

## Current Status

HostShield Manager is actively developed. Core modules include Hosts Editor, Profiles, Quick Actions, Firewall Blocker, Secure DNS / DoH, Netstat Monitor, Logs, Settings, and About.

## Roadmap Ideas

Possible future improvements:

- Stronger release packaging and installer options.
- Signed executable and trusted update flow.
- More DNS providers and advanced adapter controls.
- More detailed firewall rule inspection.
- Better automated UI testing for light and dark themes.
- More languages.
- Optional update checker.
- Import presets for popular HOSTS block lists.

## Author
Created by Eduard Prepelita.
