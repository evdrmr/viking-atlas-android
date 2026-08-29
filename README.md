# ᚱ Viking Atlas — Android Application

Official Android Companion Application and Google Play Bundle for **Viking Atlas: Interactive Cinematic Sagas and Deities Chronicles**.

---

## 📱 Application Metadata

* **Package / Application ID:** `net.hobbyshot.vikingatlas`
* **Target Web Platform:** [https://www.hobbyshot.net/viking-atlas](https://www.hobbyshot.net/viking-atlas)
* **Compile SDK:** 35 (Android 15)
* **Minimum SDK:** 24 (Android 7.0)
* **Target SDK:** 35 (Android 15)
* **Key Components:**
  - Full-screen hardware-accelerated interactive web wrapper
  - Swipe-to-refresh pull gesture
  - JavaScript bridge interface (`WebAppInterface`)
  - Offline fallback screen (*Mists of Niflheim*)
  - ProGuard / R8 code & resource shrinking
  - Release signed Android App Bundle (`.aab`) generation

---

## 🛠️ Build Commands

```bash
# Generate Debug Bundle
./gradlew bundleDebug

# Generate Signed Release Bundle for Google Play Console (.aab)
./gradlew bundleRelease
```

The release bundle is generated at:
`app/build/outputs/bundle/release/viking-atlas-release.aab`
