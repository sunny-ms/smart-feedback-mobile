# Smart Feedback — Android app (prototype)

A [Capacitor 8](https://capacitorjs.com) shell that opens the live site
(`https://smartfeedback.tutorpad.workers.dev`) and adds Google's ML Kit
document scanner through
[`@capgo/capacitor-document-scanner`](https://github.com/Cap-go/capacitor-document-scanner).

Login, pages and uploads are the website's. The app adds only native
features, which the website reaches through `window.Capacitor.Plugins`
(for example `DocumentScanner.scanDocument()`). In a normal browser
`window.Capacitor` is absent and the site behaves as before.

## Build

There's no local build. The development laptop's network blocks
`registry.npmjs.org`, so GitHub Actions builds the APK
(`.github/workflows/android.yml`) on every push to `main`, or from
**Actions → Android APK → Run workflow**:

1. `npm install`, then `npx cap add android` and `npx cap sync android`
   (the `android/` folder is generated each time and not committed).
2. `versionCode` = the run number, `versionName` = `0.1.<run>`.
3. Signs with `keystore/debug.keystore`, a fixed **debug** key (password
   `android`, no secret). The key is the same every build, so a new APK
   installs over the old one.
4. Publishes `smart-feedback-0.1.<run>.apk` as a GitHub pre-release.

## Install on a phone

1. Open the latest release under **Releases** on the phone, signed in to
   GitHub (the repo is private).
2. Download the APK and tap it. Allow "Install unknown apps" for the
   browser or My Files when asked.
3. If Play Protect warns about an unrecognised app, choose **Install anyway**.

Notes:
- The ML Kit scanner needs a real phone with Google Play services; it
  doesn't run on the emulator.
- The debug key is for testing only. A Play Store release needs a proper
  upload key kept in GitHub Secrets.
