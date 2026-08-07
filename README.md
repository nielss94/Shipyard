<div align="center">

# 🚢 Shipyard

### Build your Unity game and ship it to Steam. From your own machine, in a few clicks.

[![Download](https://img.shields.io/github/v/release/nielss94/Shipyard?style=for-the-badge&label=%E2%AC%87%20Download&color=2ea043)](https://github.com/nielss94/Shipyard/releases/latest)
[![Discord](https://img.shields.io/badge/Discord-Join%20the%20server-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/9upSEmQZJv)
[![Issues](https://img.shields.io/badge/Ideas%20%26%20bugs-Always%20welcome-f5a623?style=for-the-badge&logo=github&logoColor=white)](https://github.com/nielss94/Shipyard/issues)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Buy%20a%20coffee-FF5E5B?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/codedeckcoffee)

**Free · No account · Nothing leaves your machine · Windows & macOS**

</div>

<!-- SCREENSHOT: the Build screen mid-build, stepper on the left and live log on the right.
     This is the one that sells the tool. Full width, right here. -->

---

Publishing a Unity game to Steam by hand is a chore. Batch-mode arguments. Depot scripts. Debug folders you didn't mean to ship. A Mac build that only a Mac can make.

**Shipyard does all of it for you.** Point it at your project, press Build, press Publish.

No build server. No cloud. No subscription. No sign-up.

---

## 💬 This is built for you — tell me what you need

Shipyard exists to make shipping less painful. If something annoys you, or you want it to do something it doesn't do yet, I want to hear about it.

| | |
| :-- | :-- |
| 💬 **[Join the Discord](https://discord.gg/9upSEmQZJv)** | Questions, ideas, or just to say what you're building. No question is too small. |
| 💡 **[Open an issue](https://github.com/nielss94/Shipyard/issues)** | Bugs, feature requests, rough edges, wishlist items. All welcome, all read. |

Feature requests genuinely steer this tool. Ask for things.

**I'm planning to open source Shipyard**, so you can send pull requests directly. If that would make you contribute, say so — it moves the plan up.

---

## ⬇️ Download

**[Get the latest release →](https://github.com/nielss94/Shipyard/releases/latest)**

| | |
| :-- | :-- |
| 🪟 **Windows** | `Shipyard-<version>-win-x64-Setup.exe` — installs for your user, no admin prompt. Prefer not to install? Take the `.zip`. |
| 🍎 **macOS** | `Shipyard-<version>-mac-arm64.dmg` for Apple Silicon, `-mac-x64.dmg` for Intel. |

<details>
<summary><b>⚠️ Windows or macOS blocking it? Click here — it takes 10 seconds</b></summary>

<br>

Shipyard isn't code-signed, and the Mac build isn't notarized. Certificates cost money every year and this tool is free, so the warning is the trade. Your system is telling you the truth: it can't confirm who made this.

**Windows** — SmartScreen says the publisher is unknown. Click **More info** → **Run anyway**.

**macOS** — right-click the app and choose **Open** instead of double-clicking. If macOS says the app is *damaged*, it isn't. That's what it says about anything downloaded without a certificate:

```
xattr -dr com.apple.quarantine "/Applications/Shipyard.app"
```

**Want to verify the download first?** Every release ships a `SHA256SUMS.txt`. The number should match exactly:

```
Get-FileHash .\Shipyard-1.0.0-win-x64-Setup.exe -Algorithm SHA256
```

```
shasum -a 256 -c SHA256SUMS.txt
```

</details>

---

## 🚀 Get started

**1. Open Shipyard and add your project.** Point it at your Unity project folder.

**2. Let it write your settings.** It reads your Unity version, product name, Steam app id and Build Profiles by itself, then asks you for the few things it can't know — and tells you where in Steamworks to find each answer. Nothing is written until you say yes.

**3. Work through the Get started checklist.** Unity → project → platforms → Steam. Steam can wait; you don't need it to make a build.

**4. Press Build.**

> ### ⚠️ Check this one setting
>
> Each platform has an **output path** — where Unity writes the game, and what to call it.
>
> It must match the executable Steam launches, **exactly**, spaces included. Find it in Steamworks under **Installation → General Installation → Launch Options → Executable**.
>
> Nothing can check this for you. Unity will happily write `MyGame.exe` when Steam expects `My Game.exe`, report success, and produce a build that uploads perfectly and then does nothing when a player presses Play. Check it once per platform and you're done forever.
>
> Two things that follow: name the file Unity actually writes (the `.exe`, the `.app` — not the folder), and give each platform its own build folder, because everything in that folder gets shipped.

<details>
<summary><b>What you need installed</b></summary>

<br>

None of this has to be ready before you open the app. Shipyard checks all of it, says what's missing in plain language, and puts a button on everything it can fix for you.

| You need | Notes |
| :-- | :-- |
| **Unity**, via Unity Hub | Any version Hub can install. Shipyard matches the exact version your project asks for. Used most against Unity 6. |
| **Platform support modules** | Installed through Unity Hub. Shipyard tells you which are missing. |
| **git** | So builds can record which commit they came from. |
| **steamcmd** | Shipyard downloads and installs it for you. Nothing to do. |
| **A Steam builder account** | With upload rights to your app. Not your personal account, if you can avoid it. |

</details>

<!-- SCREENSHOT: Doctor, with a mix of green rows and one blocked row showing its
     explanation and fix button. Shows the tool being helpful rather than just failing. -->

---

## 🛠️ What it does

- 🎮 **Builds your game** for Windows, macOS and Linux, driving the Unity editor you already have.
- 🚢 **Uploads to Steam**, generating the build scripts and depot mappings from your project settings.
- 🧭 **Tells you what this machine can and can't do**, up front — instead of failing halfway through.
- 🔁 **Carries builds between machines**, so a PC and a Mac can ship one release together.
- 🛡️ **Stops the releases that break things.** Mixed versions, half-finished uploads, a Mac build packaged on Windows.
- 📋 **Keeps a record.** Every build stored with its version, its commit and the machine that made it.

Only want to build, with no Steam setup at all? That works. Steam is a separate step and stays optional until you want it.

---

## 🔨 Making a build

**1. Open Build.** Every platform in your project is listed. The ones this computer can actually produce are tickable; the rest show why not.

**2. Set a version.**

**3. Press start, and watch it work.** Steps appear as Unity reports them, next to the live log. A step you never see is a step that never ran.

Each platform is its own job, so you can cancel one without touching the others. Cancelling stops Unity properly and leaves nothing behind that looks like a finished build.

<details>
<summary><b>A few things worth knowing</b></summary>

<br>

- **One build runs at a time.** A second one waits its turn and says so. Two Unity builds on one machine just make each other slower.
- **You can leave.** The bottom bar keeps reporting while you use the rest of the app.
- **Build a specific branch or commit** by naming it. Shipyard checks it out somewhere separate, so whatever you have open and half-finished stays exactly as it is.
- **Builds** lists everything you've made — platform, version, commit, size, machine. It warns you when your newest builds came from different versions of your code, early enough to be useful.

</details>

<!-- SCREENSHOT: the Build screen before starting, with one platform tickable and another
     greyed out showing "build macOS on a macOS machine". -->

---

## 📦 Publishing

**1. Open Publish.** You get your app id, every depot, and the build that would go into each one.

**2. Pick your depots and branch**, and write the description players and testers will see.

**3. Run a Dry run first.** It does everything except upload, and shows you exactly what it would send.

**4. Choose what happens when it finishes.** Neither is preselected:

- **Upload only** — the build lands on Steam and nothing changes for anyone. You press *Set Build Live* yourself when you're ready. This is the default.
- **Set it live** — everyone on that branch gets it the moment the upload finishes.

**5. Publish.**

Shipyard checks as you go. Anything that would stop the upload shows up before it starts, not after five hundred megabytes have gone up the wire.

<details>
<summary><b>Signing in to Steam (you only do this once)</b></summary>

<br>

The first time you publish, Shipyard signs you in and asks Steam's questions as they arrive. Your password goes straight to Steam's own tool — it is not stored, not logged, and not kept in the app.

Steam has three ways of doing the second step, and Shipyard tells you which one you're in:

- a code sent to the account's email,
- a code from the Steam mobile app,
- or no code at all, just an approval to tap on your phone.

That last one catches people out, because nothing appears on screen and it looks frozen. Shipyard speaks up and tells you to check your phone.

</details>

<details>
<summary><b>Release templates — turn a release into one click</b></summary>

<br>

If you ship the same way every time, save it as a template: which platforms, which branch, how the description reads, how the version number moves. Then a release is one button, and Shipyard builds everything and uploads it in the right order.

</details>

<!-- SCREENSHOT: the Publish screen showing depots, the branch, and the two promotion
     options with their consequences written out. -->

---

## 🌍 One release, every platform

Windows can't build a Mac game, and a Mac game packaged on Windows will install and then refuse to start. So each machine builds what it can, and the pieces come together:

**1.** On the PC, build the Windows version.

**2.** Find it with **Reveal**, and copy that one file to your Mac however you like.

**3.** On the Mac, use **Import a build**. Shipyard checks the file arrived intact, which catches a truncated copy before Steam does.

**4.** Publish both from the Mac, in one upload.

<details>
<summary><b>Can I publish one platform now and the other later?</b></summary>

<br>

Shipyard will let you, and will tell you what it means — because the answer surprises people.

Promoting a half-finished release makes Steam fill in the missing platforms from whatever was there before. Nothing is left empty, but your players end up with a Windows version and a Mac version built from different versions of your code.

Publishing everything in one go is the only way to be sure that doesn't happen.

</details>

---

## 🧯 When something goes wrong

**1. Start with Doctor.** Every problem it finds comes with what it means and how to fix it. Most things end here.

**2. Open the Console** at the right of the bottom bar. A failed job keeps its reasons — error, warnings, steps and output, for uploads as well as builds.

**3. Collect diagnostics**, at the bottom of **Help**. One file describing your setup, settings and recent logs. Passwords and Steam tokens are stripped out, and the file lists what it left out. Nothing is sent anywhere — you get a file, and you decide who sees it.

**4. [Ask in Discord](https://discord.gg/9upSEmQZJv) or [open an issue](https://github.com/nielss94/Shipyard/issues).** A diagnostics bundle attached to a report is worth more than anything else you can send.

<details>
<summary><b>Why did Shipyard stop me?</b></summary>

<br>

Four things get refused, and each one exists because someone would otherwise have a bad week:

- **Publishing to the `default` branch.** That's your live store page. Steam doesn't allow it from a tool, and neither does Shipyard. Use a beta branch and promote it yourself.
- **Uploading a Mac build from a Windows PC.** It would install and never launch. Windows can still build and publish its own part of the release.
- **Publishing a release whose platforms came from different versions of your code.**
- **Trusting Unity when it says it succeeded.** Unity sometimes reports success having built nothing at all. Shipyard checks the game is actually on disk.

Building with uncommitted changes is allowed, with a warning. The build itself is fine — it just means the recorded commit doesn't describe what you shipped.

</details>

---

## 🔒 Your machine, your data

- **Nothing leaves your computer** except the build you upload to Steam. No telemetry, no analytics, no crash reporting, no account, no cloud.
- **Your Steam password is never typed into Shipyard.** It goes straight to Steam's own tool.
- **No auto-update.** Shipyard never phones home. Check [CHANGELOG.md](CHANGELOG.md) when you wonder whether to grab a newer one.

Shipyard adds exactly one file to your project — `publish.config.json`, and only after you approve it. It travels with your game, so your teammates get the same setup.

---

## ☕ Support the project

Shipyard is built and maintained by one person, alongside making a game. It's given away because it turned out to be useful.

If it saved you an afternoon and you feel like buying the coffee that went into it, there's a **[Ko-fi](https://ko-fi.com/codedeckcoffee)**. Entirely optional, and it changes nothing: the tool is free, has no paid version, and will stay that way.

The best thing you can send instead is a bug report or a feature request. [Issues](https://github.com/nielss94/Shipyard/issues) · [Discord](https://discord.gg/9upSEmQZJv)

---

<details>
<summary><b>📄 License, terms and legal</b></summary>

<br>

Shipyard is free to use, for personal and commercial projects alike, with no limits on what you make with it. The full terms are in [TERMS.md](TERMS.md).

It's provided as is, without warranty of any kind. Publishing to Steam is your responsibility, and you should check what you're about to release before you release it.

Shipyard is built on open source software. Those licenses are in [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md), and inside the app under **Help → About Shipyard**.

Copyright © 2026 Niels van Dam. All rights reserved.

Shipyard is not affiliated with, endorsed by, or connected to Unity Technologies or Valve Corporation. Unity is a trademark of Unity Technologies. Steam is a trademark of Valve Corporation.

</details>
