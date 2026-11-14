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
- **VMTechDesk** (`custom-client.txt`, `VMTechDesk.exe`) — for clients: incoming only (shows "Your ID",
  can't connect out), VM Tech's server `rd.vmtech.ca`, asks the client to Accept by default.
  It reports connections (start/accepted/end, who connected) and online status to the VM Tech portal
  (`api-server` = `https://app.vmtech.ca/rd`, RustDesk's own built-in audit + heartbeat). Never screen contents,
  files or keystrokes.
- **VMTechStaff** (`custom-staff.txt`, `VMTechStaff.exe`) — for VM Tech staff: outgoing only, same server.
  Opens `vmtechstaff://connection/new/<ID>` links from the portal once installed.

The client is NOT called VMTechRemote: that name (service + `C:\\Program Files\\VMTechRemote`) is already taken by the
VM Tech MeshCentral agent, and RustDesk installs to `Program Files\\<app name>`.

App names have **no spaces** on purpose: RustDesk uses the app name unquoted in Windows install commands
(`sc start <name>`, registry keys, the `<name>://` link type), so a space breaks installing.

The settings files are signed; the private signing key is NOT in this repository.

## Public vs inside client (Nov 2026)
- `custom-client.txt` (public, on vmtech.ca): **approve-mode is locked to "click"** in override-settings, so the client
  must click Accept for every connection — no password can open it, even if someone sets one. This keeps the promise in
  VM Tech's Terms/Privacy ("we only connect when you click Allow, every time").
- `custom-inside.txt` (inside version, staff only — VM Tech's own PCs or a trusted business with a signed agreement):
  default "click", and the VM Tech Remote Support (Inside) app may switch it to "connect anytime" with a hidden password.
- Both are the same program (`VMTechDesk.exe`); only custom.txt differs.

## VM Tech Product Support (free, Nov 2026)
- `custom-product.txt`, app `VMTechSupport` (`VMTechSupport.exe`, installs to Program Files\VMTechSupport — separate from the paid
  VMTechDesk, so both can be on one PC).
- For customers of VM TECH's products (POS AZ, Book AZ, …): works like UltraViewer. The window shows an **ID and a 6-digit
  one-time password**; the customer reads both to us, or clicks **Accept** when we connect (approve-mode both). The password
  changes after every successful connection (and after 10 wrong tries).
- Reports go to `https://app.vmtech.ca/rdp` → the portal logs these sessions at $0 (never billed).
