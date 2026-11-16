# MyApp

A minimal Java "Empty Activity" Android app.

- Package name: `com.mat.myapp`
- Language: Java
- Min SDK: 24, Target/Compile SDK: 34

## Build locally (Android Studio)

1. Open this folder in Android Studio.
2. Let it sync (it will fetch the Gradle wrapper jar automatically).
3. **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
4. APK lands in `app/build/outputs/apk/debug/app-debug.apk`.

## Build a release APK with GitHub Actions (no PC needed)

1. Create a new GitHub repo and push this project to it (branch `main`).
2. The included workflow `.github/workflows/release-apk.yml` runs automatically on push,
   and also builds `app:assembleRelease`.
3. Go to the repo's **Actions** tab → open the latest run → download the
   **app-release-apk** artifact (zip containing the APK).

### Signing the release build (recommended)

Without a keystore, Android Gradle Plugin still produces a release APK, but it
is **unsigned** and can't be installed on a normal device as-is. To get a
properly signed release APK:

1. Generate a keystore (needs a JDK with `keytool`, e.g. on your PC):
   ```
   keytool -genkeypair -v -keystore release.keystore -alias mykey \
     -keyalg RSA -keysize 2048 -validity 10000
   ```
2. In your GitHub repo, go to **Settings → Secrets and variables → Actions**
   and add these repository secrets:
   - `KEYSTORE_BASE64` — output of `base64 -w0 release.keystore` (Linux/macOS)
     or `certutil -encode release.keystore keystore.b64` (Windows)
   - `KEYSTORE_PASSWORD` — the keystore password
   - `KEY_ALIAS` — the alias you used (e.g. `mykey`)
   - `KEY_PASSWORD` — the key password
3. Push again (or re-run the workflow). The APK in the artifact will now be
   signed and installable.

**Keep `release.keystore` and its passwords private** — losing it means you
can never publish an update under the same signature again.
"# cloud" 
"# cloud" 
