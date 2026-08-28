# AssetSnap IT Lens

A QR code and barcode scanner for Android, built for fast bulk scanning of physical inventory. Scans and exports directly to CSV, XLSX, PDF, XML, JSON, or a custom webhook/API endpoint — no manual data entry required.

## Description

I built this after getting frustrated with existing code scanner apps that made bulk inventory scanning slower than it needed to be. It's designed around one core workflow: scan a stack of physical assets quickly, review/edit the data, then export straight into whatever format your tracking system expects.

Primary:
- Fast bulk scanning of both QR codes and barcodes in one flow.
- Export to `.csv`, `.xlsx`, `.pdf`, `.xml`, `.json`, or POST directly to a webhook — pick whatever your downstream system needs.
- Full scan history is viewable before you commit to an export.

Secondary:
- Duplicate scans are automatically blocked.
- Every field is editable after scanning — fix a misread serial number or fill in a blank before exporting.
- Scan history persists across app restarts (until you clear it).
- Light mode / dark mode.

## Getting Started

### Dependencies

#### System / CLI tools you need installed:
* Visual Studio Code
* Android Studio — required to build/run the Android app (provides Android SDK, build tools, phone emulator/s)
* Node.js + npm (v18+ recommended) — required to run npm install and the Capacitor CLI
* JDK 17 (bundled with recent Android Studio, or install separately) — required for Gradle builds
* Git — for cloning/pulling this repo
* A physical Android device (with USB debugging enabled) or an Android Studio emulator — for testing the camera scanner, since emulator cameras can be unreliable

#### Recommended VS Code extensions
* (Not necessarily required, but recommended) Prettier — code formatter (`esbenp.prettier-vscode`)
* Live Server — for quick local browser preview of `index.html` before packaging into the app
* ES6/JS snippet or linting extension of your choice (optional)

#### npm install commands
run:
```
npm install
```
If ever you need to install these packages manually (just as I have), here:
```
npm install @capacitor/android@^8.5.0
npm install @capacitor/core@^8.5.0
npm install @capacitor/preferences@^8.0.1
npm install @capgo/capacitor-file-sharer@^8.1.3
npm install @zxing/library@^0.23.0
npm install jspdf@^2.5.1
npm install jspdf-autotable@^3.5.28
npm install jsqr@^1.4.0
npm install --save-dev @capacitor/cli@^8.5.0
```

#### Syncing VS + Android Gradle commands
- `npx cap sync` — the one you'll run most. Copies your web assets (`index.html`, `app.js`, `styles.css`, `libs/`) into the native Android project AND updates/installs any native Capacitor plugins. Run this after every `npm install` or every time you change web code and want it reflected in the app.
- `npx cap add android` — one-time setup command. Only run this if the `android/` folder doesn't already exist in your project.
- `npx cap copy android` — a lighter version of `sync`. Copies web assets over but skips the native plugin update step.
- `npx cap open android` — opens the native project in Android Studio so you can build, run on an emulator/device, or generate a signed APK.

### Installing

* Clone the repo:
  ```
  git clone https://github.com/<your-username>/android-it-inventory-scanner.git
  cd android-it-inventory-scanner
  ```
* Install dependencies:
  ```
  npm install
  ```
* Check `capacitor.config.json` — make sure `webDir` points to wherever your `index.html`/`app.js`/`libs/` actually live.
* If the `android/` platform folder isn't present yet:
  ```
  npx cap add android
  ```
* Sync everything into the native project:
  ```
  npx cap sync
  ```
* If you want to send exports to a webhook/API endpoint, add your own endpoint URL in the app's export screen once it's running — nothing is hardcoded, it's stored locally on-device.

### Executing program

* **To preview the web UI only (no scanning, layout/UI checks only):**
  1. Open the project folder in VS Code.
  2. Right-click `index.html` → "Open with Live Server."
  3. Camera-dependent scanning won't work the same in a desktop browser preview — this is just for UI/layout iteration.

* **To run the actual Android app:**
  1. Make sure dependencies are installed and synced (`npm install` → `npx cap sync`).
  2. Open the native project in Android Studio:
     ```
     npx cap open android
     ```
  3. Plug in a physical Android device via USB (with USB debugging enabled) or start an emulator from Android Studio's Device Manager.
  4. Hit the green ▶ Run button in Android Studio and select your target device.
  5. Grant camera permission when prompted on first launch — this is required for QR/barcode scanning.
  6. Scan a code, review/edit the entry in the history list, then export using the format buttons (CSV, XLSX, PDF, JSON, XML, or Webhook POST).

* **Rebuilding after making changes:**
  ```
  npx cap copy android
  ```
  then re-run from Android Studio. Use `npx cap sync` instead of `copy` if you added/updated a Capacitor plugin.

```
# quick reference — full rebuild-and-run loop
npm install
npx cap sync
npx cap open android
```

## Help

* If `npx cap sync` fails complaining it can't find your web assets, double check the `webDir` value in `capacitor.config.json` matches your actual folder structure.
* If the camera won't initialize on a physical device, confirm camera permissions were granted in Android's app settings (Settings → Apps → AssetSnap IT Lens → Permissions).
* If Gradle build errors reference JDK version mismatches, confirm Android Studio is pointed at JDK 17 (File → Project Structure → SDK Location).

## Authors

[Torhymn](https://github.com/conductorhythmn)

## Version History

* 1.0.0
  * Initial release

## License

This project is licensed under the MIT License - see the LICENSE.md file for details

## Acknowledgments

* Built using [Capacitor](https://capacitorjs.com/)
* QR decoding via [jsQR](https://github.com/cozmo/jsQR)
* Barcode decoding via [ZXing](https://github.com/zxing-js/library)
* PDF export via [jsPDF](https://github.com/parallax/jsPDF) and [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable)
* Spreadsheet export via [SheetJS / xlsx](https://sheetjs.com/)
