# Changelog

Shipyard does not check for updates or install them, so this is the file to read when you
want to know whether a newer version is worth downloading.

Versions follow [semantic versioning](https://semver.org): the first number changes when
something works differently than it used to, the second when features are added, the third
for fixes.

---

## 0.3.0

### Fixed

- **A Mac can build a Windows game after all, if it is a Mono build.** Shipyard used to
  grey out every Windows platform on a Mac and send you to a PC. That is the right answer
  for IL2CPP, which cannot cross-compile — but not for Mono, and Unity does offer Windows
  Mono support for a Mac editor. Set the Windows platform's scripting backend to `Mono2x`
  and it becomes buildable here: Doctor offers to install the module rather than pointing
  at another machine, and the Windows depot uploads from the Mac like any other. Mono is a
  real trade — the game runs slower than IL2CPP and its code is easy to read — so this is
  a way to release from one machine, not a better default.
- **Refusals now say which of two reasons applies.** "This platform cannot be built here"
  covered two unrelated causes: IL2CPP not cross-compiling, and a Mac game losing the
  permission its binary needs to launch when packaged anywhere else. They are separate
  rules with separate fixes, and each one now says so — including when it is your own
  project settings doing the pinning rather than Unity.

### Changed

- **A project's settings no longer record which operating system each platform needs.**
  Shipyard works that out from the platform and the scripting backend, so nothing is stored
  that can go stale. Existing projects are unaffected and keep building exactly as they do
  now. If you want to take the Windows-from-a-Mac route above on a project created before
  this version, remove `requiresHostOS` from the Windows entry in `publish.config.json` —
  it is left in place as a deliberate pin, so it still wins where you set it.

---

## 0.2.0

### Downloadable content

- **Build DLC content alongside the game.** A project can declare content builds in
  `publish.config.json`, and Shipyard runs your own Addressables build method for each
  platform, then packages the result as its own artifact. Content is built once per
  platform, because AssetBundles only load on the platform they were built for.
- **Upload DLC to its own depots.** DLC depots sit beside the game's in the Publish screen,
  and the install folder each one lands in is shown next to it — that is the path your game
  hard-codes for its runtime lookup, so it is stated rather than left to be worked out.
- **A build that leaks DLC into the base game is refused.** If content meant for a DLC depot
  ends up inside the player, the build fails, names the assets, and produces nothing that
  could be uploaded. A build that cannot prove it stayed clean is refused too — "the check
  did not run" and "the check found nothing" are not the same answer.
- **Content written inside a build folder is rejected before Unity starts.** A player is
  packaged as its whole build directory, so DLC written under one would ship inside the base
  game without ever entering the player build. The config refuses that arrangement outright.
- **Both of Steam's DLC layouts are supported.** Depots under the base game give you one
  build set live in one act. Depots under the DLC's own app — Steam's "Manage DLC depots
  separately" — upload as their own build, and Shipyard says so every time, because two
  Steam builds cannot be set live together and promoting one without the other ships a game
  missing content it expects.

### Fixed

- **A script that does not compile is no longer reported as a licence problem.** A C# file
  that builds in the editor but not in a player build produced *"Unity could not activate
  its licence"* and told you to sign in to Unity Hub, which changed nothing because nothing
  was wrong with the licence. The failure now reads as a compile error, with the code, the
  file, and a button to open it.

---

## 0.1.0

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
