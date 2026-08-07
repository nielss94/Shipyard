# Shipyard

**Build your Unity game and publish it to Steam, from your own machine.**

No build server, no cloud service, no subscription, no account to create. Shipyard runs
Unity and Steam's upload tool for you, and it knows the things that normally go wrong.

<!-- SCREENSHOT: the Build screen mid-build, stepper on the left and live log on the right.
     This is the one that sells the tool. Put it here, full width. -->

Publishing a Unity game to Steam by hand means running Unity in batch mode with the right
arguments, packaging the player without accidentally shipping Unity's debug folders,
writing Steam's build scripts, remembering which machine is allowed to upload which
platform, and hoping the version you uploaded came from the commit you think it did.

Shipyard does all of that, shows you what is happening while it happens, and stops you
before the mistakes that are expensive to undo.

---

## What it does

- **Builds your game** for Windows, macOS and Linux, driving the Unity editor you already
  have installed.
- **Uploads to Steam**, generating the build scripts and depot mappings from your project's
  settings.
- **Tells you what this machine can and cannot do**, and why. A Windows PC cannot build a
  Mac game. Shipyard says so up front instead of failing halfway through.
- **Carries builds between machines**, so your PC builds the Windows version, your Mac
  builds the Mac version, and they go out as one release.
- **Refuses the releases that break things.** Mixed versions, half-finished uploads and a
  Mac build packaged on Windows all get stopped with an explanation.
- **Keeps a record.** Every build is stored with its version, its commit and the machine
  that produced it.

Everything happens on your computer. Nothing is uploaded anywhere except to Steam, when you
press the button.

---

## Download

Get the latest version from the
[releases page](https://github.com/nielss94/Shipyard/releases/latest).

| | |
| --- | --- |
| **Windows** | `Shipyard-<version>-win-x64-Setup.exe` installs for your user only, so there is no admin prompt. Prefer not to install? Take the `.zip` and run it from anywhere. |
| **macOS** | `Shipyard-<version>-mac-arm64.dmg` for Apple Silicon, `-mac-x64.dmg` for Intel. |

Shipyard itself runs on Windows and macOS.

### Your computer will warn you, and here is why

Shipyard is not code-signed, and the Mac version is not notarized. Signing certificates
cost money per year and this is a tool given away for free, so the warning is the trade.
Your system is telling you the truth: it cannot confirm who made this.

What you can do instead is check that the file you downloaded is the file that was
published. Each release lists a `SHA256SUMS.txt`, and the number your computer prints
should match the one in that file exactly:

```
Get-FileHash .\Shipyard-1.0.0-win-x64-Setup.exe -Algorithm SHA256
```

```
shasum -a 256 -c SHA256SUMS.txt
```

Then get past the warning:

- **Windows.** SmartScreen says the publisher is unknown. Click **More info**, then
  **Run anyway**.
- **macOS.** Right-click the app and choose **Open** rather than double-clicking it. If
  macOS says the app is damaged, it is not. That is what it says about apps downloaded
  from the internet without a certificate. Clear the download flag and open it again:

  ```
  xattr -dr com.apple.quarantine "/Applications/Shipyard.app"
  ```

---

## Before you start

| You need | Notes |
| --- | --- |
| **Unity**, installed through Unity Hub | Any version Hub can install. Shipyard matches the exact version your project asks for. It has been used most against Unity 6. |
| **The platform support for what you want to build** | Installed through Unity Hub as a module. Shipyard checks and tells you which are missing. |
| **git** | Your project should be in a git repository, so builds can record which commit they came from. |
| **steamcmd** | Shipyard downloads and installs it for you. Nothing to do in advance. |
| **A Steam builder account** | An account with upload rights to your app on Steamworks. Not your personal account, if you can avoid it. |

None of this has to be ready before you open the app. Shipyard checks all of it, tells you
what is missing in plain language, and puts a button on everything it can fix for you.

---

## Setting up your project

<!-- SCREENSHOT: Doctor, with a mix of green rows and one blocked row showing its
     explanation and fix button. Shows the tool being helpful rather than just failing. -->

**1. Add your project.** Point Shipyard at your Unity project folder. It checks that it
really is one and tells you what was missing if not.

**2. Let it write your settings file.** Shipyard keeps each project's publishing settings
in a small file called `publish.config.json` at the root of your project, so it travels
with your game and your teammates get the same setup. If the file is not there yet,
Shipyard offers to write it. It reads your Unity version, product name, Steam app id and
Build Profiles itself, then asks you for the few things it cannot know. For each one it
tells you where in Steamworks to find the answer. Nothing is written until you say yes.

**3. Work through Get started.** A checklist in dependency order: Unity, your project,
platforms, then Steam. Steam is marked as something that can wait, because you do not need
it to make a build.

### The one setting worth double-checking

Each platform in your settings has an **output path**: where Unity should write the game,
and what to call it.

It has to match the executable Steam is configured to launch, **exactly**, spaces
included. You can find that in Steamworks under **Installation → General Installation →
Launch Options**, in the **Executable** field.

This is the only setting nothing can check for you. Unity will happily write
`MyGame.exe` when Steam is expecting `My Game.exe`, report success, and produce a build
that uploads perfectly and then does nothing when a player presses Play. Check it once per
platform and you never have to think about it again.

Two smaller things that follow from it:

- Name the file Unity actually writes: the `.app` bundle on Mac, the `.exe` on Windows,
  not the folder they sit in.
- Give each platform its own build folder, because everything in that folder is shipped.
  Rename your game's executable and the old one is still sitting there, ready to go up
  with the next build.

---

## Making a build

<!-- SCREENSHOT: the Build screen before starting, with one platform tickable and another
     greyed out showing "build macOS on a macOS machine". -->

Open **Build**. Every platform in your project is listed, and the ones this computer can
actually produce are tickable. The rest are greyed out with the reason, which is usually
that a Mac game needs a Mac.

Set a version, press start, and watch it work. The steps appear as Unity reports them, so
nothing is invented and a step you never see is a step that never ran. Beside them is the
live log, following along until you scroll up to read something.

Each platform is its own job, so you can cancel one without touching the others. Cancelling
stops Unity properly and leaves nothing behind that looks like a finished build.

A few things worth knowing:

- **One build runs at a time.** A second one waits its turn and says so. Two Unity builds
  on one machine just make each other slower.
- **You can leave.** The bar along the bottom keeps reporting while you use the rest of the
  app.
- **Build a specific version of your code** by naming a git branch or commit. Shipyard
  checks it out somewhere separate, so whatever you have open and half-finished stays
  exactly as it is.

**Builds** lists everything you have made: platform, version, commit, size, and which
machine produced it. It warns you when the newest build of each platform came from
different versions of your code, which is the thing that will stop you at publish time,
said early enough to be useful.

---

## Two computers, one release

Windows cannot build a Mac game, and a Mac game packaged on Windows will install and then
refuse to start. So each machine builds what it can, and the pieces come together:

1. On the PC, build the Windows version.
2. Find it with **Reveal**, and copy that one file to your Mac however you like.
3. On the Mac, use **Import a build**. Shipyard checks the file arrived intact, which
   catches a copy that got cut short before you find out from Steam.
4. Publish both from the Mac, in one upload.

You can also publish one platform now and the other later. Shipyard will do it, and will
tell you what it means, because the answer surprises people: promoting a half-finished
release makes Steam fill in the missing platforms from whatever was there before. Nothing
is left empty, but your players end up with a Windows version and a Mac version built from
different versions of your code. Publishing everything in one go is the only way to be
certain that does not happen.

---

## Publishing

<!-- SCREENSHOT: the Publish screen showing depots, the branch, and the two promotion
     options with their consequences written out. -->

Open **Publish**. You get your app id, every depot, and the build that would go into each
one. Pick your depots, pick a branch, write the description players and testers will see.

Shipyard checks as you go. Anything that would stop the upload appears before you start it,
not after five hundred megabytes have gone up the wire.

**Try Dry run first.** It does everything except upload, and shows you exactly what it
would send.

Then choose what happens when the upload finishes. Both outcomes are written on screen and
neither is preselected:

- **Upload only.** The build lands on Steam and nothing changes for anyone. You press *Set
  Build Live* yourself on the Steam partner site when you are ready. This is the default.
- **Set it live.** Everyone on that branch gets this build the moment the upload finishes.

### Signing in to Steam

The first time you publish, Shipyard signs you in to Steam and asks you Steam's questions
as they arrive. Your password goes straight to Steam's own tool. It is not stored, not
written to any log, and not kept in the app.

Steam has three ways of doing the second step and Shipyard tells you which one you are in:

- a code sent to the email on the account,
- a code from the Steam mobile app,
- or no code at all, just an approval to tap in the Steam app on your phone.

That last one catches people out, because nothing appears on screen and it looks like the
app has frozen. Shipyard speaks up and tells you to check your phone.

You only do this once.

### Release templates

If you ship the same way every time, save it as a template: which platforms, which branch,
how the description reads, how the version number moves. Then a release is one click, and
Shipyard builds everything and uploads it in the right order.

---

## What Shipyard will not let you do

Each of these is a refusal, and each exists because someone would otherwise have a bad
week:

- **Publishing to the `default` branch.** That is your live store page. Steam does not
  allow it from a tool, and neither does Shipyard. Use a beta branch and promote it
  yourself.
- **Uploading a Mac build from a Windows PC.** It would install and never launch. Windows
  can still build its own version and publish its own part of the release.
- **Publishing a release whose platforms came from different versions of your code.**
- **Trusting Unity when it says it succeeded.** Unity sometimes reports success having
  built nothing at all. Shipyard checks the game is actually on disk.

Building with uncommitted changes is allowed, with a warning, because the build itself is
fine. It just means the recorded commit does not describe what you shipped.

---

## Your data stays yours

- **No telemetry, no analytics, no crash reporting.** Nothing about you or your game leaves
  your machine except the build you upload to Steam.
- **No account, no cloud, no sign-up.**
- **Your Steam password is never typed into Shipyard.** It goes to Steam's own tool.
- **Passwords, tokens and Steam Guard codes are stripped** from logs and captured output
  before anything is written to disk.
- **No auto-update.** Shipyard does not phone home to check for versions. Come back to the
  releases page when you want a newer one.

---

## When something goes wrong

**Start with Doctor.** Every problem it finds comes with what it means and how to fix it,
and most things end there.

**A job that failed keeps its reasons.** Open the **Console** at the right of the bottom
bar for the error, the warnings, the steps and the output, for uploads as well as builds.

**Need someone else to look?** At the bottom of **Help**, *Collect diagnostics* writes a
single file describing your setup, your settings and your recent logs. Passwords and Steam
tokens are stripped out of everything in it, and the file lists what it deliberately left
out. Nothing is sent anywhere. You get a file, and you decide who sees it.

**Help** is worth reading once, even if nothing is broken. It explains why a machine can
only build some platforms, why the Mac has to upload the Mac version, what "not set live"
really means, and what Steam actually does when you promote a release that is missing a
platform.

---

## What to expect from support

Shipyard is built and maintained by one person, alongside making a game. It is given away
because it turned out to be useful, not because it comes with a support contract. Bug
reports are read and appreciated; there is no promise about how fast. A diagnostics bundle
attached to a report is worth more than anything else you can send.

If Shipyard saved you an afternoon and you feel like buying the coffee that went into it,
there is a [Ko-fi](https://ko-fi.com/codedeckcoffee). Entirely optional, and it changes
nothing: the tool is free, has no paid version, and will stay that way.

---

## Questions people ask

**Does it work with my Unity version?**
Any version you can install through Unity Hub. Shipyard matches the exact version your
project asks for. It has been used most against Unity 6.

**Do I need Steam open?**
No. Shipyard uses Steam's command line upload tool, which it installs for you.

**Can I use it just to build, without any Steam setup?**
Yes. Steam is a separate step and Shipyard treats it as optional until you want it.

**Will it change my project?**
It adds one settings file, and only after you approve it. When you build a specific branch
or commit it works somewhere separate, so your working copy is never touched.

**Does it support Linux?**
Linux game builds are supported by the same settings as the others, though they have had
far less use than Windows and macOS. The Shipyard app itself runs on Windows and macOS.

**Does it upload anywhere other than Steam?**
No.

---

## License and terms

Shipyard is free to use, for personal and commercial projects alike, with no limits on what
you make with it. The full terms are in [TERMS.md](TERMS.md).

It is provided as is, without warranty of any kind. Publishing to Steam is your
responsibility, and you should check what you are about to release before you release it.

Shipyard is built on open source software. Those licenses are in
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md), and also inside the app under
**Help → About Shipyard**.

What changed between versions is in [CHANGELOG.md](CHANGELOG.md). There is no auto-update,
so that is the file to check when you wonder whether to download a newer one.

Copyright © 2026 Niels van Dam. All rights reserved.

Shipyard is not affiliated with, endorsed by, or connected to Unity Technologies or Valve
Corporation. Unity is a trademark of Unity Technologies. Steam is a trademark of Valve
Corporation.
