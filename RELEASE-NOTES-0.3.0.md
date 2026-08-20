<!-- Paste this into the GitHub release body for the v0.3.0 tag. -->

- **A Mac can now build the Windows version of your game, if it is a Mono build.** Shipyard
  used to grey out Windows on a Mac and send you to a PC. That is right for IL2CPP, which
  cannot cross-compile, and wrong for Mono, which can — Unity offers Windows Mono support
  for a Mac editor. So a project that ships Mono can now build and release everything from
  one machine.
- **To turn it on**, set the Windows platform's scripting backend to `Mono2x` in Project
  settings. Doctor then offers to install *Windows Build Support (Mono)* through Unity Hub
  instead of pointing you at another machine, and Windows becomes tickable on the Build
  screen. The Windows depot uploads from the Mac like any other.
- **Mono is a real trade.** The game runs slower than an IL2CPP build and its code is
  straightforward to read. Worth it to release from one machine; not a better default.
- **Unity decides the backend, not Shipyard.** The setting in Shipyard says what this
  machine may attempt; Unity's own Player settings are what get used. If they disagree the
  build starts and then fails, so check **Project Settings → Player → Scripting Backend**
  once.
- **Nothing else changed about which machine builds what.** A Windows IL2CPP player still
  needs Windows, and a Mac game still has to be built and uploaded from a Mac.
- **For a project created before this version**, remove the `requiresHostOS` line from the
  Windows entry in `publish.config.json`. Shipyard no longer writes that field — it works
  out which machine can build what from the platform and the backend — but where you have
  one it is treated as a deliberate pin and still wins.
- **Refusals now name which rule applies.** "IL2CPP cannot cross-compile" and "a Mac game
  loses the permission it needs to launch if it is packaged anywhere else" were one message
  before. They are different problems with different fixes, and each now says so —
  including when it is your own project settings doing the pinning rather than Unity.
- Full list of changes in [CHANGELOG.md](https://github.com/nielss94/Shipyard/blob/main/CHANGELOG.md).
