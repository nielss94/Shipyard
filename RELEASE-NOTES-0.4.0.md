# Shipyard 0.4.0

**Windows only.** This release ships a Windows installer and a zip. There is no Mac download for 0.4.0; Mac users stay on 0.3.0 for now.

## Less reading, same safety

Shipyard used to explain itself a lot. Every screen had boxes of text telling you what was about to happen and why, and after a while you stopped reading them. This release cuts that down.

- **One line per warning.** Before an upload, each thing worth knowing is now a single line. If you want the full story, click the line and it opens. If you don't, it stays out of your way.
- **The button tells you what it does.** The upload button now reads "Upload 2 depots" or "Upload 2 depots, set live on develop", so you no longer need a box above it saying the same thing.
- **Only the option you picked explains itself.** When choosing between uploading and going live, the consequence appears under your choice, not under both.
- **Quieter everywhere else.** The Build, Builds, Settings and setup screens follow the same rule: a short line, and the detail one click away. Repeated reminders were removed.

## Small fix

- When a Mac build and its DLC were both missing from a release, the warning said "macos, macos". It now names the DLC properly.

## Installing

Windows will show a SmartScreen warning because the app is not signed. Choose **More info**, then **Run anyway**. Verify the download against `SHA256SUMS.txt` if you want to be sure it is the file we uploaded.
