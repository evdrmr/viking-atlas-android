# Sagas of Yggdrasil — Android

Android WebView companion for [Sagas of Yggdrasil](https://www.gamebooks.tech/sagas-of-yggdrasil), the flagship interactive gamebook at Gamebooks.tech.

## Current configuration

| Setting | Value |
| --- | --- |
| Application ID / namespace | `net.hobbyshot.sagasofyggdrasil` |
| Web target | `https://www.gamebooks.tech/sagas-of-yggdrasil` |
| Compile / target SDK | 36 |
| Minimum SDK | 24 |
| Source version at 2026-09-05 baseline | versionCode `3`, versionName `1.0.2` |
| Gradle wrapper | 9.4.1 |
| Android Gradle Plugin | 9.2.1 |
| Kotlin plugin | 2.2.10 |
| Gradle daemon toolchain | Java 21, configured in `gradle/gradle-daemon-jvm.properties` |

Treat `app/build.gradle` and the wrapper/toolchain files as authoritative when releasing.

## How it works

`MainActivity.kt` loads the live web target and enables JavaScript and DOM storage for browser-local progress. It supports swipe-to-refresh, back navigation, image selection, and an `AndroidApp.setSwipeRefreshEnabled` JavaScript bridge. External hosts generally open through Android intents; inspect the actual URL routing rules when changing navigation.

A packaged *Mists of Niflheim* page appears on connection failure and retries the public URL. It is an error/retry screen, not an offline copy of the story. Full offline gameplay has not been established. Native WebView progress is local to the app and does not automatically synchronize with another browser or device.

Narrative, historical data, CSS, and JavaScript changes delivered to the live site normally require no Android rebuild. Rebuild for changes to native code, packaged resources, URL/bridge configuration, SDK settings, signing, or native versioning.

## Build and release

Inspect Git status before changing the project. Use the Gradle wrapper, configured Java toolchain, and a local Android SDK with the required platform installed. Keep machine-specific SDK configuration in ignored `local.properties`.

Release signing reads ignored `keystore.properties` with `storeFile`, `storePassword`, `keyAlias`, and `keyPassword`. These values and the signing keystore must stay out of Git, documentation, and logs. A missing signing file does not prove that `bundleRelease` will fail: the Gradle configuration only attaches the release signing config when properties exist, so verify the resulting signature.

```bash
# Native validation, as appropriate to the change
./gradlew lint testDebugUnitTest

# Debug bundle
./gradlew bundleDebug

# Release bundle
./gradlew bundleRelease
```

Release lint is not enforced by the current build (`checkReleaseBuilds false`, `abortOnError false`), so inspect lint results explicitly. No authored native test cases were present at the September 2026 baseline; a successful empty unit-test task is not device validation.

Expected outputs:

- `app/build/outputs/bundle/debug/sagas-of-yggdrasil-debug.aab`
- `app/build/outputs/bundle/release/sagas-of-yggdrasil-release.aab`

Before a new Play upload, increment `versionCode` beyond the latest uploaded release and set a suitable `versionName`. Verify build success, artifact timestamp, package, version, and signing certificate. Old `viking-atlas-release.aab` files may remain locally; do not confuse them with a fresh release.

Smoke-test launch, navigation/back, progress after reload, external links, offline retry, swipe-to-refresh, and any changed bridge behavior on a device or emulator. Leave the verified AAB ready for the user to upload manually to Google Play Console.

The web project's [operations runbook](../viking-atlas/docs/runbook.md) describes local validation, GitHub push, server pull/build, smoke testing, restart, and production verification. Documentation-only Android corrections do not require a version bump or new AAB.
