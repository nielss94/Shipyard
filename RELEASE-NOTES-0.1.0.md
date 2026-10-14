<!-- Paste this into the GitHub release body for the v0.1.0 tag. Fill in the checksums from
     SHA256SUMS.txt once both machines have built, and delete any download row you did not
     actually build. -->

# Shipyard 0.1.0

Build your Unity game and publish it to Steam, from your own machine. No build server, no
cloud service, no account.

This is the first public release. What is in it, in full, is in
[CHANGELOG.md](https://github.com/nielss94/Shipyard/blob/main/CHANGELOG.md); the short version is that Shipyard drives Unity and steamcmd
for you, tells you what this machine can and cannot build and why, carries builds between a
PC and a Mac so they release together, and refuses the four releases that are expensive to
undo.

## Download

| Platform | File |
| --- | --- |
| Windows | `Shipyard-0.1.0-win-x64-Setup.exe` (installs for your user, no admin prompt) |
| Windows, no installer | `Shipyard-0.1.0-win-x64.zip` |
| macOS, Apple Silicon | `Shipyard-0.1.0-mac-arm64.dmg` |
| macOS, Intel | `Shipyard-0.1.0-mac-x64.dmg` |

## Your computer will warn you

These builds are not code-signed, and the macOS build is not notarized. Signing
certificates cost money every year and this is free, so the warning is the trade. Your
system is telling you the truth: it cannot confirm who made this.

Check the download against `SHA256SUMS.txt` first:

```
shasum -a 256 -c SHA256SUMS.txt
```

```
Get-FileHash .\Shipyard-0.1.0-win-x64-Setup.exe -Algorithm SHA256
```

Then:

- **Windows.** SmartScreen says the publisher is unknown. **More info**, then **Run anyway**.
- **macOS.** Right-click the app and choose **Open** rather than double-clicking. If macOS
  says the app is damaged, it is not, that is what it says about unsigned downloads:
  ```
  xattr -dr com.apple.quarantine "/Applications/Shipyard.app"
  ```

## Before you start

Unity installed through Unity Hub, git, and a Steam builder account with upload rights to
your app. steamcmd installs itself from inside the app. Nothing has to be ready in advance:
Doctor checks all of it and tells you what is missing.

## Worth knowing

- A machine can only build the platforms it has player support for, and a Mac game can only
  be built on a Mac. This is not a limitation Shipyard invented, and it explains it in place
  rather than failing halfway through.
- Uploading and going live are separate choices. Uploading without setting a build live is
  the default.
- There is no auto-update. Check [CHANGELOG.md](https://github.com/nielss94/Shipyard/blob/main/CHANGELOG.md) when you want to know whether
  a newer version is worth downloading.
- No telemetry, no analytics, no account.

Full guide in the [README](https://github.com/nielss94/Shipyard#readme). Problems go in the issue tracker, and
**Help → Collect diagnostics** produces exactly the information a bug report needs, already
stripped of passwords and tokens.
