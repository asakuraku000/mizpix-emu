# MizPix Emu

An Android app that wraps the [Mizpix Online](https://www.mizpixonline.com/) web game in a WebView, with native touch controls, a cleaner HUD and a built-in update checker.

> **Unofficial.** This project is a fan-made wrapper. It is not made by, affiliated with, or endorsed by Mizpix Online. The game itself is loaded from `https://www.mizpixonline.com/`.

## Features

- **Native joystick.** Your thumb is handled by the app itself, so the joystick keeps working while you attack or use skills with another finger. It stays where you last touched it.
- **Native skill, potion, attack and target buttons.** The game's web buttons are hidden and replaced with round on-screen buttons that copy the game's own icons and cooldowns. They hide automatically when a menu is open.
- **Cleaner HUD.** Compact header (Life / Mana / EXP / Gold and buffs), a scrolling news ticker for announcements, the map on the left with a show/hide button, and the party panel and target frame in better spots.
- **Landscape only.** Locked to landscape with both directions supported.
- **Check updates button.** A gear button at the right end of the header buttons opens a dialog that compares the game's JS and CSS files with the server. If anything changed, **Update now** downloads the new files and replaces your saved copies, then **Restart game** reloads the game.
- **Automatic update watch.** While the game is open, the app checks every minute and shows a "May bagong update ng game" banner with a **Refresh** button when something changed.
- **Saved copy on your phone.** Game files (JS, CSS, images, audio) are stored in the app's private storage and revalidated against the server, so the game starts quickly and still opens when your connection is poor.
- **Error log.** Crashes and errors are saved to a log you can view, copy or share from the in-app menu.

## Install

1. Open the [**Releases**](../../releases) page and download the latest `.apk`.
2. Open the file on your phone. If Android asks, allow installs from your browser or file manager.
3. If Play Protect warns about an unknown app, that is expected for apps installed outside the Play Store. The app only requests internet access (see below).
4. If you already have an older version that was signed with a different key, uninstall it first.

Requires Android 5.0 (API 21) or newer.

## Using the app

| Action | How |
| --- | --- |
| Move | Touch and drag in the left half of the screen |
| Attack / skills / potions | The round buttons on the right |
| Check for game updates | Tap the gear button in the header, then **Check updates** |
| Menu (reload, redownload, error log, exit) | Press the Android **Back** button |

The Back menu has:
- **Reload game** checks the core files, then loads the game.
- **Redownload resources** clears the saved copies and downloads everything again.
- **View error log** shows saved errors with Share and Clear.
- **Exit** closes the app.

## Permissions and privacy

The app requests only:

- `INTERNET`
- `ACCESS_NETWORK_STATE`

It does not request access to your contacts, messages, files, accounts or location. The only site it connects to is `www.mizpixonline.com`. The error log stays on your phone unless you choose to share it.

## Build from source

Requirements: Android Studio (recent version), JDK 11 or newer, Android SDK 33.

```bash
git clone https://github.com/asakuraku000/mizpix-emu.git
cd mizpix-emu
./gradlew assembleDebug
```

The debug APK is created in `app/build/outputs/apk/debug/`.

### Signed release build

1. Create a key once:
   ```bash
   keytool -genkey -v -keystore mizpix.jks -keyalg RSA -keysize 2048 -validity 10000 -alias mizpix
   ```
2. Put `mizpix.jks` next to `settings.gradle.kts`.
3. Copy `keystore.properties.example` to `keystore.properties` and fill in your passwords.
4. Run `./gradlew assembleRelease`. The signed APK is created in `app/build/outputs/apk/release/`.

**Never commit or share `mizpix.jks` or `keystore.properties`.** Back them up privately, because every future update must be signed with the same key.

## Configuration

Most options live in `app/src/main/java/com/mixpix/emu/Config.java`, including the game URL, joystick behavior, HUD tweaks, update-check timing and the settings button (`SETTINGS_BUTTON`). `README_MIZPIX.md` documents each option in more detail (in Filipino).

## Project layout

```
app/src/main/
  java/com/mixpix/emu/   Java source (MainActivity, GameClient, UpdateWatcher, ResourceSync, ...)
  assets/                mz_*.js scripts injected into the game page
  res/                   layouts, drawables, strings
```

The `mz_*.js` scripts and the Java code are bundled inside the APK, so changing them requires building a new APK. The game's own JS and CSS update through **Check updates** without a rebuild.

## Disclaimer

Use at your own risk. Check the game's rules before using a modified client. This project does not change the game's server connection or game logic; it only changes how the game is displayed and controlled on your device.
