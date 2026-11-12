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

## The two apps (same program, different signed `custom.txt` next to `rustdesk.exe`)
- **VM Tech Remote** (`custom-client.txt`) — for clients: incoming only (shows "Your ID", can't connect out),
  VM Tech's server `rd.vmtech.ca`, asks the client to Accept by default.
- **VM Tech Staff** (`custom-staff.txt`) — for VM Tech staff: outgoing only, same server.

The settings files are signed; the private signing key is NOT in this repository.
