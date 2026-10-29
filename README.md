# Stundenplan Übersicht – Prüfungskalender

Fertige, mobile-optimierte Prüfungskalender-Website im Design deiner HTML-Vorlage.

- **1. Termin rot**, **2. Termin blau**, **WH-Termin grün** (Farben frei änderbar)
- Zwei Countdown-Kacheln für den nächsten **1. und 2. Termin**
- Monatskalender, Filter, nach Datum sortierte Terminliste, FH-Prüfungslinks
- Pro Fach bis zu drei unabhängig konfigurierbare Prüfungen
- Admin: Fächer/Termine neu anlegen, einzeln ergänzen, verschieben, bearbeiten, entfernen
- Datum-Picker, Uhrzeit, Prüfer, Raum, Anmelde- und Abmeldefrist, Link, Hinweis
- Browser speichert persönliche Anmeldemarkierungen und Farben im Local Storage
- Automatischer Light/Dark-Modus und manuelle Auswahl
- Gemeinsame Online-Daten für alle via Firebase Firestore, live synchronisiert

## 1. Direkt ansehen

Öffne `index.html` im Browser oder lade sie auf GitHub Pages hoch. **Ohne Firebase-Konfiguration funktioniert die Anzeige bereits mit den 10 Prüfungsterminen aus der gelieferten Vorlage.** Im Vorschaumodus ist die Admin-Speicherung deaktiviert – sonst wären Änderungen nur auf einem einzelnen Gerät sichtbar.

## 2. Firebase kostenlos einrichten (einmalig)

Firebase ist nötig, damit *alle* Mitstudenten dieselben Änderungen sehen. GitHub Pages kann nur statische Dateien ausliefern und keine Termine dauerhaft online verändern.

1. Gehe zu **https://console.firebase.google.com/** und erstelle ein Firebase-Projekt im kostenlosen **Spark**-Tarif. Eine Web-App zum Projekt hinzufügen.
2. In **Authentication → Sign-in method** die Methode **Email/Password** aktivieren. Unter **Users / Nutzer** einen Benutzer mit deiner gewünschten E-Mail und einem **selbst gewählten Passwort** anlegen. **Diese E-Mail und die UID** des Nutzers notieren. Login-Name auf der Website bleibt unabhängig davon `called_michi15`.
3. Unter **Firestore Database** eine Datenbank im **Standard-Modus** (z. B. EU-Region) erstellen. Sicherheitsregeln *nicht* dauerhaft im Testmodus lassen.
4. **Firestore Database → Rules** öffnen, den Inhalt aus `firestore.rules` einsetzen und `ADMIN_UID_HIER` durch die tatsächliche UID aus Schritt 2 ersetzen. Regeln **veröffentlichen**. Alle dürfen nur den Kalender lesen; schreiben kann ausschließlich deine UID.
5. `index.html` in einem Texteditor öffnen und im oberen JavaScript-Block `const CONFIG` suchen. Eintragen:
   - `adminEmail`: E-Mail des zuvor erstellten Firebase-Benutzers
   - `adminUid`: genau dieselbe Firebase-UID wie in den Sicherheitsregeln
   - `firebase.apiKey`, `firebase.authDomain`, `firebase.projectId`, `firebase.appId`: Werte aus **Firebase → Projekteinstellungen → Deine Apps → Web-App → SDK-Einrichtung und Konfiguration**
6. Unter **Authentication → Settings → Authorized domains** die GitHub-Pages-Domain `DEINBENUTZER.github.io` hinterlegen, falls sie noch nicht eingetragen ist (besonders wichtig bei gehostetem Login).

**Das Passwort wird nicht in `index.html` eingetragen und gehört auch nicht ins GitHub-Repository.** Es wird ausschließlich in Firebase Authentication eingerichtet und beim Login eingegeben. Die Firebase-Web-App-Konfiguration ist öffentlich und darf im HTML stehen; der tatsächliche Schreibschutz entsteht durch Authentication + Firestore-Regeln.

## 3. GitHub Pages aktivieren

1. Ein neues **öffentliches** Repository auf GitHub erstellen, z. B. `stundenplan-uebersicht`.
2. `index.html` ins Stammverzeichnis hochladen. `README.md` und `firestore.rules` kannst du ebenfalls hochladen, sind für die Website aber nicht nötig.
3. Repository → **Settings → Pages** → **Deploy from a branch** → Branch `main`, Ordner `/ (root)` → Save.
4. Die Website ist unter `https://DEINBENUTZER.github.io/stundenplan-uebersicht/` erreichbar.
5. Website öffnen → **Admin** → Benutzername `called_michi15` → dein Firebase-Passwort → Anmelden.
6. Ein Fach bearbeiten oder einen Titel speichern. **Das erste Speichern veröffentlicht auch die 10 Beispieltermine** im gemeinsamen Online-Kalender. Ab dann erscheinen Änderungen automatisch bei allen Besuchern.

## 4. Benutzung

- **Termin ergänzen:** Admin → Fach bearbeiten → gewünschten Termintyp aktivieren → Daten eintragen → speichern.
- **1., 2. und WH-Termin gleichzeitig:** Im selben Fachformular mehrere Termintypen aktivieren, alle Felder ausfüllen und einmal speichern.
- **Termin verschieben:** Datum eines vorhandenen Termins ändern; seine individuelle Markierung „Angemeldet“ bleibt erhalten.
- **Einzelnen Termin löschen:** Häkchen des jeweiligen Termintyps deaktivieren und Fach speichern (mindestens ein Termin muss bleiben). Soll das gesamte Fach verschwinden, auf **Fach löschen** gehen.
- **Farbe ändern:** Zahnrad → drei Farben auswählen → Speichern. Gilt nur im jeweiligen Browser.
- **Angemeldet markieren:** Das ist eine persönliche Merkhilfe und **keine echte FH-Anmeldung**. Die tatsächliche Anmeldung erfolgt über den optionalen FH-Link.
- **An-/Abmeldefrist:** Für jede Prüfung kann ein eigenes Datum hinterlegt werden; die verbleibenden Tage werden automatisch angezeigt.
- **Titel ändern:** Admin → **Kalendertitel ändern**; damit lässt sich die Website für das nächste Semester weiterverwenden.

## 5. Hinweise und Grenzen

- Das Admin-Passwort im HTML zu hinterlegen wäre bei einer öffentlichen GitHub-Website **kein Zugangsschutz**; jeder könnte es im Quelltext sehen. Darum ist Firebase-Login vorgesehen.
- Alle Schreibvorgänge sind durch Firestore-Sicherheitsregeln auf die oben eingetragene UID beschränkt. **Veröffentliche die Regeln unbedingt**, sonst klappt die Speicherung nicht sicher.
- GitHub Pages allein ist kostenlos; Firebase hat im Spark-Tarif ebenfalls kostenlose Kontingente, die für einen Jahrgang mit einem kleinen Kalender typischerweise genügen. Für Firestore sind dies u. a. 50.000 Lese- und 20.000 Schreibvorgänge pro Tag. Beachte die jeweils gültigen Konditionen.
- Die Website verwendet `localStorage` für Anzeigeeinstellungen und individuelle Anmeldemarkierungen, nicht Cookies. Ein anderer Browser übernimmt diese persönlichen Markierungen nicht.
- Initial sind die zehn Termine aus deiner HTML-Vorlage enthalten. Es wurden **keine FH-Links erfunden**: Diese kannst du im Adminformular nachtragen.
- Firestore ist hier bewusst eine einfache gemeinsame Kalender-Datei (`calendars/main`). Für einen einzelnen Admin und einen kleinen Jahrgang reicht das; bei sehr vielen gleichzeitigen Admins wären Transaktionen/Einzeldokumente sinnvoll.

## Projektdateien

- `index.html`: **komplette Website**, benötigt keine Build-Tools oder npm
- `firestore.rules`: Schreibzugriff nur für die Admin-UID
- `README.md`: diese Anleitung