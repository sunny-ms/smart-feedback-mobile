# Smart Feedback — Android app (beta)

**Download:** https://github.com/sunny-ms/smart-feedback-mobile/releases/latest/download/smart-feedback.apk

A [Capacitor 8](https://capacitorjs.com) shell that opens the live site
(`https://smartfeedback.tutorpad.workers.dev`) and adds Google's ML Kit
document scanner through
[`@capgo/capacitor-document-scanner`](https://github.com/Cap-go/capacitor-document-scanner).

Login, pages and uploads are the website's. The app adds only native
features, which the website reaches through `window.Capacitor.Plugins`
(for example `DocumentScanner.scanDocument()`, used by
`frontend/src/lib/nativeScanner.js` in the web app repo). In a normal
browser `window.Capacitor` is absent and the site behaves as before.

## Install on an Android phone

1. Open the download link above on the phone.
2. Open `smart-feedback.apk`. Allow "Install unknown apps" for the browser
   or My Files when asked.
3. If Play Protect warns about an unrecognised app, choose **More details →
   Install anyway** (expected for apps not from the Play Store).
4. Log in with your Smart Feedback account.

New versions install over the old one and keep you logged in.

## Build

GitHub Actions builds the APK (`.github/workflows/android.yml`) on every
push to `main`, or from **Actions → Android APK → Run workflow**:

1. `npm install`, then `npx cap add android` and `npx cap sync android`
   (the `android/` folder is generated each time and not committed).
2. `versionCode` = the run number, `versionName` = `0.2.<run>`.
3. Signs a release build with the release key from GitHub Secrets
   (`ANDROID_KEYSTORE_B64`, `ANDROID_KEYSTORE_PASSWORD`,
   `ANDROID_KEY_ALIAS`) and checks the signature.
4. Publishes `smart-feedback.apk` as the latest GitHub release.

There's no local build: the development laptop's network blocks
`registry.npmjs.org`.

### The release key

Every APK must be signed with the same key, or phones refuse to update
the installed app. The key lives only in GitHub Secrets and with the
maintainer, outside this repo. Losing it means every tester has to
uninstall and reinstall. For the Play Store, it would become the upload
key under Play App Signing.

## Notes

- The ML Kit scanner needs a real phone with Google Play services; it
  doesn't run on the emulator.
- iOS isn't built yet: it needs an Apple Developer account and TestFlight.
