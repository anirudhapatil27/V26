# Dr Patil Digital OPD — V26 Android build

V26 is an upgrade to the approved V25 clinical application. It adds a clearly visible direct-print action for patient copies while retaining the existing pre-printed-letterhead and PDF/WhatsApp routes.

## V26 printing options

### New consultation
- **Print Patient Copy** — prints the complete prescription with the original digital letterhead through Android's native PrintManager. Select the connected printer in the Android print dialog.
- **Print on Pre-printed Letterhead** — retains the existing physical-letterhead layout for printing on pre-printed stationery.
- **Save complete PDF for WhatsApp** — retains the existing digital PDF creation and Android share/save flow.

### Follow-up
- **Print Follow-up Patient Copy** — prints the full follow-up document with the digital letterhead through Android's native print dialog.
- The existing follow-up PDF route remains available.

The direct-print route uses the same V25 master document builder, A4 page geometry, letterhead image, typography and clinical document content. It does not create a separate or simplified prescription template.

## Preserved V25 functionality

The V26 HTML was derived from the V25 source. Changes to the HTML are limited to adding the two print buttons and their event handlers. The clinical data-entry logic, HPI, diagnosis and medicine libraries, patient records, pre-operative and follow-up workflows, settings, local storage keys and complete backup/restore logic are retained.

## Repository files

- `DrPatil_Digital_OPD_V26_Final.html` — V26 application source.
- `InstrumentSerif-Italic.ttf` — matching font asset referenced by the application.
- `.github/workflows/build-apk.yml` — GitHub Actions workflow for Android packaging, source validation, JavaScript checks, browser PDF/print-bridge smoke tests, APK integrity checks and artifact upload.
- `SOURCE-CHECKSUMS.txt` — SHA-256 checksums for the app source and font.

## Create the GitHub repository

1. Create a new, empty GitHub repository.
2. Extract the ZIP and upload the **contents** of the extracted `DrPatil-Digital-OPD-V26-GitHub` folder to the repository root.
3. Commit the files to the `main` branch.
4. Open **Actions** and select **Build Dr Patil Digital OPD V26 — Direct Patient Printing** if the workflow has not started automatically.
5. Wait for every job step to pass. Download the artifact named `Dr-Patil-Digital-OPD-V26-Direct-Print` from the completed workflow run. The debug APK is inside the artifact ZIP.

## Build and security notes

- Workflow permissions are limited to `contents: read`.
- Third-party GitHub Actions are pinned to full commit SHAs; the version is shown in a comment.
- No personal access token, signing key, patient data or repository secret is required to build the debug APK.
- The workflow validates the exact V26 HTML checksum, the two new print controls, V25 data/backup keys and existing print routes.
- It extracts real HTML script blocks with an HTML parser and runs `node --check` on each one.
- The Chromium smoke test checks the direct-print native bridge hand-off and PDF generation output.
- The Android build step verifies APK archive integrity, package ID and APK signature.

## Moving from an already installed V25 APK

Before replacing V25, open **Settings → Complete Patient & App Backup → Export complete backup** and save the JSON backup somewhere outside the app. A fresh GitHub-hosted debug build may be signed with a different temporary debug key from the one used for the previously installed APK. Android can therefore reject an in-place update because the signatures differ. If Android reports a signature/package conflict, uninstall the old V25 app only after confirming the backup file is safely saved, install V26, then use **Import complete backup** in Settings. Do not uninstall V25 before exporting and checking the backup. The application ID remains `com.drpatil.digitalopd`.

## Important test boundary

A successful GitHub build and headless browser test cannot physically verify a printer, Android print service, WhatsApp, or Xiaomi Pad hardware. Install the APK on the target tablet and test all three consultation options before clinical use. For direct printing, Android's print service and the printer's compatible driver/plugin must be installed and the printer must be connected.

This workflow produces a **debug APK**, not a Play Store release APK. Keep complete patient backups secure and do not commit patient records to GitHub.
