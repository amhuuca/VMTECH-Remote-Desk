# VM Tech Remote — VM Tech's build of RustDesk

This is RustDesk (https://github.com/rustdesk/rustdesk, AGPL-3.0) with VM Tech branding, built for
VM Tech (vmtech.ca) clients and staff. All credit for the remote desktop itself goes to the RustDesk team.

## What we changed
- `src/common.rs` — the app trusts settings files (`custom.txt`) signed with **VM Tech's** key
  (`vmtech/vmtech-settings-key.pub`) instead of RustDesk's. Nothing else in how RustDesk works is changed.
- Icons (`res/*.png`, `res/*.ico`, `flutter/windows/runner/resources/app_icon.ico`) and the in-app
  logo (`vmtech/icon.png`, `vmtech/logo.png`, copied into `flutter/assets/` at build time).
- `flutter/windows/runner/Runner.rc` — product name "VM Tech Remote".
- `.github/workflows/vmtech-windows.yml` — builds Windows x64 only and outputs two zips.

## The two apps (same program, different signed `custom.txt` next to the exe)
- **VMTechRemote** (`custom-client.txt`, `VMTechRemote.exe`) — for clients: incoming only (shows "Your ID",
  can't connect out), VM Tech's server `rd.vmtech.ca`, asks the client to Accept by default.
  It reports connections (start/accepted/end, who connected) and online status to the VM Tech portal
  (`api-server` = `https://app.vmtech.ca/rd`, RustDesk's own built-in audit + heartbeat). Never screen contents,
  files or keystrokes.
- **VMTechStaff** (`custom-staff.txt`, `VMTechStaff.exe`) — for VM Tech staff: outgoing only, same server.
  Opens `vmtechstaff://connection/new/<ID>` links from the portal once installed.

App names have **no spaces** on purpose: RustDesk uses the app name unquoted in Windows install commands
(`sc start <name>`, registry keys, the `<name>://` link type), so a space breaks installing.

The settings files are signed; the private signing key is NOT in this repository.
