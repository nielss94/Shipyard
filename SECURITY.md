# Security

Shipyard handles Steam credentials, so a security report is worth more than a bug report.

## Reporting

Do not open a public issue for a security problem. Use GitHub's
[private vulnerability reporting](https://docs.github.com/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
on this repository instead.

Include what you did, what happened, and the version from **Help → About Shipyard**. If a
credential of yours was exposed, rotate it first and report second.

This is a solo project, so there is no guaranteed response time. Reports are read.

## What Shipyard does with credentials

Stated here so a report can point at a broken promise rather than at a suspicion:

- **Your Steam password is never handled by Shipyard.** Sign-in runs Steam's own `steamcmd`
  and passes what you type straight to it. The password is not stored, not logged, and not
  kept in the app.
- **The Steam login token belongs to steamcmd**, which writes it where it keeps its own
  configuration. Shipyard reads only the account names from that file, never the token.
- **Passwords, tokens and Steam Guard codes are redacted** from captured output before it
  reaches the screen, a log file, or disk.
- **Diagnostics bundles are redacted**, and the folder holding the Steam login token is
  never read when one is built. Each bundle lists what it left out.
- **Nothing is sent anywhere.** No telemetry, no analytics, no crash reporting, no update
  check. The only network traffic Shipyard causes is Steam's own upload tool talking to
  Steam, and downloading steamcmd from Valve when you ask it to install one.

A credential appearing in a log, in a diagnostics bundle, on screen, or in any file
Shipyard writes is a security bug. Please report it.

## What is not a vulnerability

- **The builds are unsigned, and the macOS build is not notarized.** This is known and
  documented. Verify the SHA-256 checksum published with each release.
- **Shipyard runs Unity, git and steamcmd as you.** It is a tool for driving them, so it
  has whatever access you have.
