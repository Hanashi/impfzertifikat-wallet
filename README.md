# Impfzertifikat Wallet

Leider möchten [CovPass](https://github.com/eu-digital-green-certificates/dgca-wallet-app-ios/issues/69) und die [Corona Warn App](https://github.com/corona-warn-app/cwa-wishlist/issues/503) das Impfzertifikat nicht in Apple Wallet integrieren. Deswegen zeige ich dir hier, wie du das selbst machen kannst. Bei dieser Methode müsst ihr euer Zertifikat nicht aus der Hand geben und es landet auf keinem externen Server.

## Voraussetzungen
- macOS
- XCode
- Apple Entwicklerkonto
- bisschen Ahnung von Computern

## 1. signpass bauen

1. Lade dir das [Wallet Beispiel](https://developer.apple.com/services-account/download?path=/iOS/Wallet_Support_Materials/WalletCompanionFiles.zip) von Apple herunter.
2. Entpacke die ZIP-Datei (wenn dies nicht ohnehin automatisch geschieht).
3. Gehe in den Ordner `signpass`.
4. Öffne das Projekt (durch Doppelklicken auf `signpass.xcodeproj`) in XCode.
5. Klicke in XCode auf `Product` > `Build For` > `Running`.
6. Gehe in den Build Ordner (meiner liegt unter `/Users/hanashi/Library/Developer/Xcode/DerivedData`) und suche die kompilierte Datei `signpass`.
7. Kopiere die Datei in Ordner wo du sie schnell wiederfindest (z.B. `~/Documents`).

## 2. Zertifikat erstellen

### 2.1 Identifier anlegen

1. Gehe zu [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/certificates/list).
2. Wähle [Identifieres](https://developer.apple.com/account/resources/identifiers/list) aus.
3. Klick auf das [+](https://developer.apple.com/account/resources/identifiers/add/bundleId).
4. Wähle `Pass Type IDs` und klicke auf `Continue`.
5. Gib eine Beschreibung bei `Description` an und einen `Identifier` (z.B. `pass.dev.hanashi.impfzertifikat`) und klicke auf `Continue`.
6. Klicke nun auf `Register`.

### 2.2 Zertifikat erstellen

1. Öffne das Programm `Schlüsselbundverwaltung` auf deinem Mac.
2. Klicke im Menü auf `Schlüsselbundverwaltung` > `Zertifikatassistent` > `Ein Zertifikat einer Zertifizierungsinstanz mit "Max Mustermann" anfordern...` (statt `Max Mustermann` steht da wahrscheinlich dein Name).
3. Gib eine E-Mail und einen allgemeinen Namen an. Die Korrektheit ist nicht wichtig.
4. Wähle die Option `Auf der Festplatte sichern` und klicke auf `Fortfahren`.
5. Wähle einen Pfad aus wo das Zertifikat gespeichert werden soll.
6. Gehe zu [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/certificates/list).
7. Klicke auf das [+](https://developer.apple.com/account/resources/certificates/add).
8. Wähle unter `Services` die Option `Pass Type ID Certificate` und klicke auf `Continue`.
9. Vergib dem Zertifikat einen Namen und wähle bei `Pass Type ID` deine (in 2.1) erstellte `Pass Type ID` aus und klicke auf `Continue`.
10. Lade hier deine in 2.2.5. abgelegt Datei hoch und klicke auf `Continue`.
11. Klicke auf Download und öffne die `*.cer`-Datei (dadurch wird sie in den Schlüsselbund importiert).

## 3. Wallet-Informationen konfigurieren

1. Klone dieses Repository.
2. Öffne die Datei `pass.json` im Ordner `Impfzertifikat.pass` in einen Editor.
3. Lese den Inhalt des QR-Codes deines digitalen Impfzertifikats aus.
4. Enkodiere den ausgelesenen Text als JSON (z.B. mit folgendem [Tool](https://de.functions-online.com/json_encode.html), aber Achtung das Tool könnte die Informationen speichern).
5. Kopiere den enkodieren Text und ersetze den Text `"BARCODE"` in Zeile 7 in der `pass.json`.
6. Ersetze in Zeile 3 den Text `PASS_TYPE_IDENTIFIER` durch deinen `Identifier`, welchen du in 2.1.5 gewählt hast.
7. Ersetze in Zeile 5 den Text `TEAM_ID` durch deine Team ID, welche du in deinem Entwickler-Konto unter [Membership](https://developer.apple.com/account/#/membership) findest.
8. Ersetze in Zeile 18 den Text `Max Mustermann` durch deinen Namen (optional kannst du Zeile 16 bis 19 löschen).
9. Speichere die `pass.json`.

## 4. .pkpass-Datei generieren

1. Öffne das Terminal und navigiere via `cd` zu dem Pfad wo der Ordner `Impfzertifikat.pass` liegt.
2. Führe den Befehl `~/Documents/signpass -p Impfzertifikat.pass` aus (den Pfad zu `signpass` musst du ggf. anpassen).
3. Nun sollte die .pkpass-Datei erstellt sein. Diese kannst du via E-Mail, Airdrop oder auch auf alle anderen Wege auf dein iPhone bringen.