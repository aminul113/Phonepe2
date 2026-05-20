# PhonePe Business → Telegram Forwarder

Android-only app that captures every PhonePe Business payment via **SMS**, **Notifications**, and **Accessibility (screen overlay)**, deduplicates across all three sources, and forwards the payment to your Telegram bot **instantly** in real time. Runs in background as a foreground service.

> ⚠️ Sideload only. Cannot be published on Google Play because of `READ_SMS`.

---

## EASIEST way to get a real APK — GitHub Actions (no Android Studio needed)

1. Create a free GitHub account at https://github.com/signup if you don't have one.
2. Click **+ → New repository**, name it anything (e.g. `phonepe-telegram`), set **Public** or **Private**, click **Create repository**.
3. On the new repo page, click **uploading an existing file** (link in the middle).
4. Drag-and-drop **every file and folder from this unzipped project** (including the hidden `.github` folder — on Mac press `Cmd+Shift+.`, on Windows enable "Hidden items" in Explorer).
5. Scroll down → click **Commit changes**.
6. Click the **Actions** tab at the top → click the latest **"Build APK"** run → wait ~3-5 minutes for the green ✓.
7. Scroll to **Artifacts** at the bottom → click **phonepe-telegram-apk** to download a ZIP containing `app-debug.apk`.
8. Transfer the `.apk` to your phone, tap it, allow "Install unknown apps", install.

That's it. You now have a real, installable APK.

---

## Alternative: Android Studio (manual, one-time setup)

1. Download Android Studio: https://developer.android.com/studio
2. Install with default options (this takes 15-30 min and downloads ~3 GB of SDK).
3. Launch Android Studio → **Open** → select this unzipped `phonepe-telegram` folder.
4. Wait for "Gradle sync" to finish at the bottom (5-10 min first time).
5. Top menu: **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
6. When done, click **locate** in the toast → grab `app/build/outputs/apk/debug/app-debug.apk`.
7. Send the APK to your phone and install.

---

## After installing on your phone

1. Open the app.
2. Paste your **Telegram Bot Token** and **Chat ID** → tap **Save** → tap **Send test** to verify.
3. Grant all three permissions:
   - **SMS read** (in-app prompt)
   - **Notification access** (opens system list → toggle on "PhonePe → Telegram")
   - **Accessibility** (opens system list → tap our app → toggle on)
4. Tap **Disable battery optimization** so Android doesn't kill the background service.
5. Done. Next PhonePe payment you receive → Telegram pings instantly.

## How the dedupe works

Same payment may fire on SMS + Notification + Overlay within a few seconds. The app keeps a 10-minute in-memory cache keyed by the transaction ID (from SMS) or by `amount + sender + minute-bucket` when the txn ID isn't available. **First source wins — Telegram is sent once.**

## File layout

```
app/src/main/
  AndroidManifest.xml
  java/com/lovable/phonepetg/
    MainActivity.kt
    core/      Parser, Dedupe, TelegramClient, Pipeline, PaymentEvent
    capture/   SmsReceiver, PhonePeNotifListener, PhonePeAccessibility, BootReceiver
    service/   ForegroundCaptureService
    data/      Settings (EncryptedSharedPreferences)
  res/xml/accessibility_config.xml
.github/workflows/build-apk.yml   ← cloud APK builder
```
