# Changelog

Shipyard does not check for updates or install them, so this is the file to read when you
want to know whether a newer version is worth downloading.

Versions follow [semantic versioning](https://semver.org): the first number changes when
something works differently than it used to, the second when features are added, the third
for fixes.

---

## 1.0.0

The first public release.

### Building

- Build any Unity project for Windows, macOS and Linux, driving the Unity editor installed
  through Unity Hub. Progress is reported as Unity reports it, so nothing is invented and a
  step you never see is a step that never ran.
- Live Unity log beside the steps, following the tail until you scroll up.
- Cancel a build without leaving anything behind that looks finished. Cancelling stops the
  whole process tree, not just the Unity process you can see.
- Build a specific branch or commit without touching your working copy. It is checked out
  separately, and each platform keeps its own Unity `Library` between builds, which is the
  difference between a cold build and a warm one.
- One build at a time, so two of them cannot make each other slower. A second queues and
  says so.
- Everything you build is recorded with its version, its commit, its size and the machine
  that produced it.

### Publishing to Steam

- Upload to Steam depots, with the build scripts and depot mappings generated from your
  project's settings.
- Checks that run while you fill the form, so anything that would stop the upload appears
  before you start it rather than after.
- A dry run that does everything except upload.
- Uploading and going live are separate choices, both spelled out on screen. Uploading
  without setting the build live is the default, always.
- Sign in to Steam from inside the app, including all three kinds of Steam Guard: an
  emailed code, a code from the mobile app, and an approval you tap on your phone with no
  code at all. Your password goes to Steam's own tool and is never stored or logged.
- Release templates, so a release you make the same way every time is one click.

### Working across two machines

- Windows cannot build a Mac game, so each machine builds what it can. Import a build
  handed over from the other one and publish them together.
- Imported builds are checked against their manifest, so a copy that arrived truncated is
  caught here rather than on Steam.
- A warning when the newest build of each platform came from different versions of your
  code, shown on the Builds screen rather than only at publish time.

### Knowing what is wrong

- Doctor checks the whole machine and explains every problem with what it means and how to
  fix it, with a button on everything it can do for you. steamcmd installs itself.
- A setup checklist that walks the same checks in dependency order.
- Failures are explained in plain words rather than as raw output, with the error, the
  warnings, the steps and the captured output kept for every job.
- Help answers why the tool behaves the way it does, written as consequences rather than
  mechanics.
- Collect diagnostics writes one file describing your setup for someone else to read.
  Passwords and Steam tokens are stripped from everything in it, the folder holding your
  Steam login is never opened, and the file lists what it deliberately left out. Nothing is
  sent anywhere.

### Refusals

Shipyard stops these rather than warning about them, because each one produces a release
that is expensive to undo:

- Publishing to the `default` branch.
- Uploading a macOS depot from Windows, which would install and never launch.
- Publishing a release whose platforms came from different versions of your code.
- Treating a Unity exit code of 0 as success when no game was actually written to disk.

### Notes

- Windows and macOS. No installer is published for Linux, though Linux game builds are
  configurable like any other platform.
- The builds are not code-signed and the macOS build is not notarized, so your operating
  system will warn you. The README explains what to click, and every release lists SHA-256
  checksums you can verify first.
- No telemetry, no analytics, no crash reporting, no account, no auto-update.
