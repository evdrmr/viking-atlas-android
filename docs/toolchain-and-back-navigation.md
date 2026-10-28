# Android toolchain and back navigation

Updated October 27, 2026.

## Build configuration

- Build JDK: Java 25; the workstation uses Android Studio's bundled JDK through `JAVA_HOME`.
- Gradle wrapper: 9.8.2, with the official distribution SHA-256 pinned in `gradle/wrapper/gradle-wrapper.properties`.
- Kotlin Gradle plugin: 2.4.21.
- Android Gradle Plugin: 9.2.1; compile/target SDK: 36.
- Java and Kotlin output remains Java 8-compatible. The build JDK is separate from Android bytecode targeting.
- Kotlin compiler options use the typed `compilerOptions` DSL instead of deprecated `android.kotlinOptions`.

Kotlin 2.4.21's [published compatibility table](https://kotlinlang.org/docs/gradle-configure-project.html) includes AGP 9.2.1 but lists Gradle 9.7.0 as its highest fully supported version. Newer Gradle versions are permitted with possible warnings or limitations. Gradle 9.8.2 is the owner's requested version; local verification does not expand the upstream support matrix.

## Back navigation

The activity registers a lifecycle-aware AndroidX `OnBackPressedCallback`, enabled only while `WebView.canGoBack()` is true. The callback navigates WebView history; at the root, it is disabled so Android handles leaving the activity. State is refreshed in `doUpdateVisitedHistory`, `onPageFinished`, and after each `restoreState` call, including activity recreation. There is no legacy `onBackPressed()` override.

See [Android's predictive-back migration guidance](https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture).

## Verification

On October 27, 2026, clean debug/release builds, release bundle generation, signature verification, and both lint variants passed locally with this toolchain. Lint reported zero errors; remaining warnings are recorded in its reports. Existing unit-test executions: 0; failures: 0.

Run with Java 25:

```bash
./gradlew clean test lintDebug lintRelease assembleDebug assembleRelease bundleRelease --no-build-cache
```

Check both lint XML reports for Error/Fatal findings even where `abortOnError` is disabled. Verify the release AAB with `jarsigner -verify`. Tasks with no unit-test sources are not test coverage.

Device acceptance remains separate: test gesture and button Back through multiple WebView pages, returning to the root, same-document history, reload/offline recovery, and rotation/process recreation with restored history. These local build checks do not establish device or Play Console acceptance.
