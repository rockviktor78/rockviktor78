# Definition of Done - Join Projekt

Bitte erfülle alle Punkte auf dieser Liste, bevor du das Projekt einreichst.

## 📋 Allgemeine Anforderungen

### Projektstruktur
- [ ] Alle User einschließlich Gast-Login nutzen das gleiche Board und die selben Kontakte, Tasks etc.
- [ ] Alle User Stories und Akzeptanzkriterien sind erfüllt
- [ ] Alle Features funktionieren fehlerfrei und wie erwartet
- [ ] Mindestens 5 realistische Tasks erstellt
- [ ] Mindestens 10 Kontakte hinzugefügt
- [ ] Manuelles Testing mit aktuellsten Versionen: Chrome, Firefox, Safari, Edge
- [ ] Bei Projektabgabe Link zum GitHub Repository beigefügt

### GitHub-Richtlinien
- [ ] Repository ist **public**
- [ ] GitHub von Anfang an genutzt und gepflegt
- [ ] Regelmäßige Commits von jedem Teilnehmer (mindestens ein Commit pro Arbeitssitzung)
- [ ] Aussagekräftige Commit-Messages verwendet
- [ ] `.gitignore` vorhanden (unnötige Dateien ausgeschlossen)
- [ ] Nach Abschluss: Jedes Gruppenmitglied hat das Projekt geforked

### User Experience
- [ ] User erhält intuitiv Feedback bei Interaktionen (hover, toast-messages etc.)
- [ ] Alle UI-Elemente entsprechen dem Design-Prototypen (Farben, Abstände, Schatten)
- [ ] Transitions auf anklickbaren Elementen: 75ms - 125ms
- [ ] Funktioniert auf mobilen Geräten mit vertikaler Anordnung der Kanban-Spalten
- [ ] Buttons haben `cursor: pointer;`
- [ ] Inputs und Buttons haben keinen Standard-Border (`border: unset;`)

## 🛠️ Technische Anforderungen

### Architektur & Struktur
- [ ] MPA-Architektur (Multi-Page-Application)
- [ ] Strukturierte und konsistente Dateinamen und -strukturen
- [ ] Startseite heißt `index.html`
- [ ] Keine Konsolenfehler, Fehlermeldungen oder Logs
- [ ] Maximal 400 Zeilen Code pro Datei
- [ ] Erstellter Content ist unmittelbar sichtbar

### Design
- [ ] Buttons haben `cursor: pointer;`
- [ ] Inputs und Buttons ohne Standard-Border (`border: unset;`)
- [ ] Form-Validation bei leeren Inputs implementiert

### Responsiveness
- [ ] Jede Seite funktioniert bei jeder Auflösung bis min. 320px
- [ ] Content-Begrenzung für große Monitore (max-width z.B. bei 1920px / linksbündig)
  - Gilt nicht für Design-Elemente
- [ ] Funktioniert sowohl mobile als auch auf Desktop
- [ ] Landscape-Modus auf mobilen Geräten deaktiviert (außer speziell optimiert)
- [ ] Keine horizontalen Scrollbalken bei kleineren Auflösungen

### Dateistruktur
- [ ] Dateinamen sind beschreibend und aussagekräftig
- [ ] Konsistente Benennung
- [ ] Für jede Seite mindestens eine JS-Datei
- [ ] Eine allgemeine seitenübergreifende JS-Datei vorhanden
- [ ] Strukturierte CSS-Dateienstruktur

### Formulare
- [ ] Form Validation implementiert
- [ ] Erstellter Content direkt sichtbar
- [ ] Button deaktiviert während Ladezeit
- [ ] **Assigned-to Feld**: Drop-Down Menü schließt automatisch bei Klick außerhalb
- [ ] **Assigned-to Feld**: Kontakte (nicht User) sind auswählbar
- [ ] **Subtask-Feld**: Enter-Taste erstellt Subtask (nicht Haupt-Task)

## 💻 JavaScript / Clean Code

- [ ] Eine Funktion hat nur eine Aufgabe
- [ ] Eine Funktion ist maximal 14 Zeilen lang (HTML ausgenommen)
- [ ] Deutliche Funktionsnamen
- [ ] camelCase für Dateinamen, Variablen und Funktionen
- [ ] Erster Buchstabe von Funktionen/Variablen ist kleingeschrieben
- [ ] 2 Leerzeilen Abstand zwischen Funktionen
- [ ] Max 400 LOCs (Lines of Code) pro Datei
- [ ] Dateien richtig benannt: `index.html`, `script.js`, `style.css`
- [ ] HTML Code in extra Funktion (falls nötig)
- [ ] Extra Ordner für templates und Bilder (`img`)
- [ ] Statischer HTML Code wird nicht über JavaScript generiert
- [ ] Funktionen nach JSDoc Standard dokumentiert

## ⚠️ Häufige Fehler vermeiden

- [ ] Menüpunkte verschieben sich nicht beim Hover
- [ ] Tickets verschwinden nicht beim Drag & Drop
- [ ] User-Feedback vorhanden wenn etwas gespeichert/geändert wird
- [ ] Columns im Board gehen nicht zu weit runter
- [ ] Formvalidation bei Add Contact / Edit Contact vorhanden
- [ ] Kein "rauslaufen" von Subtasks, Kontakten und allgemeinem Content

---

# User Stories

## 1. Benutzeraccount & Administration

### User Story 1: Benutzerregistrierung
**Als** neuer Benutzer **möchte ich** mich registrieren können, **um** Zugang zu Join zu erhalten und Join nutzen zu können.

**Akzeptanzkriterien:**
- [ ] Registrierungsformular mit E-Mail-Adresse, Name und Passwort
- [ ] Datenschutzerklärung muss vor Registrierung akzeptiert werden
- [ ] Fehlermeldung bei falscher Eingabe (z.B. ungültige E-Mail)
- [ ] "Registrieren"-Button deaktiviert, solange nicht alle Pflichtfelder ausgefüllt

### User Story 2: Anmeldung
**Als** Benutzer **möchte ich** mich anmelden können, **um** Zugriff auf das Dashboard und das Kanban-Board zu bekommen.

**Akzeptanzkriterien:**
- [ ] Login-Formular mit E-Mail und Passwort
- [ ] Fehlermeldung bei falscher Eingabe
- [ ] Gast-Login Option (kann alle Funktionalitäten testen)
- [ ] Nicht angemeldete Besucher werden bei geschützten Seiten auf Login-Seite weitergeleitet

### User Story 3: Abmeldung
**Als** Benutzer **möchte ich** mich von Join abmelden können, **damit** niemand ohne meine Zustimmung auf meinen Account zugreifen kann.

**Akzeptanzkriterien:**
- [ ] "Logout"-Option in der Benutzeroberfläche
- [ ] Nach Logout: Weiterleitung zum Login-Bildschirm
- [ ] Persönliche Daten ohne erneutes Einloggen nicht zugänglich

### User Story 4: Dashboard
**Als** Benutzer **möchte ich** die wichtigsten Informationen zu Anzahl der Tasks und nächste Deadline auf dem Dashboard sehen.

**Akzeptanzkriterien:**
- [ ] Dashboard zeigt Anzahl der Tasks bis zur nächsten Deadline
- [ ] Dashboard zeigt Anzahl der Tasks in: ToDo, In Progress, Awaiting Feedback, Done
- [ ] Begrüßungsnachricht abhängig von Tageszeit (z.B. "Good morning, [Benutzername]")

---

## 2. Kanbanboard & Taskmanagement

### User Story 1: Kanban-Board Anzeige
**Als** Benutzer **möchte ich** die Tasks auf einem Kanban-Board angezeigt bekommen.

**Akzeptanzkriterien:**
- [ ] Board mit vier Spalten: ToDo, In Progress, Awaiting Feedback, Done
- [ ] Info-Text wenn Spalte keine Tasks enthält
- [ ] Jeder Task zeigt: Kategorie, Titel, Beschreibung-Preview, zugewiesene Benutzer (Initialen), Priorität
- [ ] Vollständige Beschreibung und alle Infos beim Klick auf Task
- [ ] "+"-Icon in jeder Spalte zum Hinzufügen neuer Tasks

### User Story 2: Subtask-Fortschritt
**Als** Benutzer **möchte ich** den Fortschritt von Tasks mit Subtasks visualisiert sehen.

**Akzeptanzkriterien:**
- [ ] Fortschrittsanzeige oder Balkendiagramm für Tasks mit Subtasks
- [ ] Anzeige: Anzahl erledigte Subtasks / Gesamtzahl Subtasks
- [ ] 100% Fortschritt bei vollständig abgeschlossenen Tasks (abhebende Farbe)
- [ ] Hover/Klick zeigt Details (z.B. "5 von 7 Subtasks erledigt")

### User Story 3: Suchfunktion
**Als** Benutzer **möchte ich** Tasks anhand ihres Titels schnell finden können.

**Akzeptanzkriterien:**
- [ ] Suchfeld auf dem Kanban-Board
- [ ] Echtzeit-Filterung bei Eingabe
- [ ] Nur Tasks mit passendem Titel/Beschreibung angezeigt
- [ ] Alle Tasks sichtbar bei leerer Suchanfrage
- [ ] Hinweis bei keine Suchergebnisse (z.B. "Keine Ergebnisse gefunden")

### User Story 4: Task hinzufügen
**Als** Benutzer **möchte ich** Tasks intuitiv hinzufügen können mit allen notwendigen Details.

**Akzeptanzkriterien:**
- [ ] "Add Task"-Option im Hauptmenü
- [ ] "+"-Icon in jeder Spalte (Status wird automatisch gesetzt)
- [ ] "Add Task"-Symbol neben Suchleiste
- [ ] Formular mit folgenden Feldern:
  - [ ] **Titel*** (Pflichtfeld)
  - [ ] **Beschreibung** (optional)
  - [ ] **Fälligkeitsdatum (Due Date)*** (Pflichtfeld)
  - [ ] **Priorität** (urgent, medium, low - Default: "Medium")
  - [ ] **Zugewiesen an (Assigned to)** (Dropdown für Kontakte)
  - [ ] **Kategorie*** (Pflichtfeld - "Technical Tasks" oder "User Story")
- [ ] Speichern nur möglich mit: Titel, Fälligkeitsdatum, Kategorie

### User Story 5: Subtasks verwalten
**Als** Benutzer **möchte ich** Subtasks hinzufügen, bearbeiten und organisieren können.

**Akzeptanzkriterien:**
- [ ] Subtask-Abschnitt mit Eingabefeld im Task-Formular
- [ ] Enter-Taste oder Häkchen-Symbol fügt Subtask hinzu
- [ ] "X"-Symbol setzt Eingabefeld zurück
- [ ] Eingabefeld wird nach Hinzufügen automatisch geleert
- [ ] Hover zeigt Stift-Icon (Bearbeiten) und Mülleimer-Icon (Löschen)
- [ ] Stift-Icon ermöglicht Bearbeiten des Subtask-Titels
- [ ] Mülleimer-Icon löscht Subtask

### User Story 6: Task bearbeiten/löschen
**Als** Benutzer **möchte ich** bestehende Tasks bearbeiten oder löschen können.

**Akzeptanzkriterien:**
- [ ] Klick auf Task öffnet Ticketdetails-Ansicht
- [ ] Stift-Icon aktiviert Bearbeitungsmodus
- [ ] Bearbeitbar: Titel, Beschreibung, Fälligkeitsdatum, Priorität, Zugeordnete, Subtasks
- [ ] **NICHT** bearbeitbar: Kategorie
- [ ] Änderungen können gespeichert oder verworfen werden
- [ ] Papierkorb-Icon löscht Task dauerhaft
- [ ] Gelöschter Task nicht mehr auf Board sichtbar

### User Story 7: Drag & Drop
**Als** Benutzer **möchte ich** Tasks per Drag & Drop zwischen Spalten verschieben können (Desktop & Mobil).

**Akzeptanzkriterien:**
- [ ] Tasks sind "greifbar" und verschiebbar
- [ ] Visuelle Rückmeldung während Drag (z.B. leichte Drehung)
- [ ] Loslassen in Spalte platziert Task und aktualisiert Status
- [ ] Verschieben erfolgt flüssig ohne Verzögerung
- [ ] Task bleibt an Position bis erneut verschoben
- [ ] Spalten zeigen gestrichelte Box (dashed box) als Drop-Zone
- [ ] **Mobil**: Vertikale Spaltenanordnung
- [ ] **Mobil**: Long Tap zum Greifen ODER Pfeil-Icon öffnet Popup-Menü zur Auswahl

---

## 3. Verwaltung der Kontakte

### User Story 1: Kontaktliste
**Als** Benutzer **möchte ich** eine übersichtliche alphabetisch sortierte Kontaktliste sehen.

**Akzeptanzkriterien:**
- [ ] Seite/Bereich für Kontakte vorhanden
- [ ] Alphabetische Sortierung nach Namen
- [ ] E-Mail-Adresse unterhalb des Namens angezeigt
- [ ] Unterteilung in Buchstaben-Abschnitte
- [ ] Klick auf Kontakt öffnet Detailansicht (Name, E-Mail, Telefon)

### User Story 2: Kontaktinformationen nachschlagen
**Als** Benutzer **möchte ich** Kontaktinformationen wie E-Mail und Telefon nachschlagen können.

**Akzeptanzkriterien:**
- [ ] Klick auf Kontakt öffnet Detailansicht
- [ ] Detailansicht zeigt: Name, E-Mail-Adresse, Telefonnummer

### User Story 3: Kontakt hinzufügen
**Als** Benutzer **möchte ich** neue Kontakte hinzufügen können.

**Akzeptanzkriterien:**
- [ ] "Hinzufügen"-Option oder Symbol vorhanden
- [ ] Formular für: Name, E-Mail, Telefonnummer
- [ ] Nach Speichern: Kontakt in Liste sichtbar

### User Story 4: Kontakt bearbeiten/löschen
**Als** Benutzer **möchte ich** Kontakte bearbeiten oder löschen können.

**Akzeptanzkriterien:**
- [ ] Optionen zum Bearbeiten und Löschen in Detailansicht
- [ ] Bearbeiten öffnet Formular mit vorhandenen Daten
- [ ] Löschen entfernt Kontakt endgültig
- [ ] Gelöschter Kontakt wird aus allen zugewiesenen Aufgaben entfernt

### User Story 5: Eigenen Account bearbeiten
**Als** Benutzer **möchte ich** meinen eigenen Account in der Kontaktliste bearbeiten können.

**Akzeptanzkriterien:**
- [ ] Eigener Account in "Contacts"-Liste sichtbar
- [ ] Eigener Kontakt kann genauso bearbeitet werden wie andere

---

## 4. Sonstiges

### User Story 1: Legal Notice / Impressum
**Als** Benutzer **möchte ich** die Rechtshinweise und Impressum einsehen können.

**Akzeptanzkriterien:**
- [ ] "Legal Notice" Bereich vorhanden
- [ ] Link führt zu Seite mit Anbieter-Informationen und rechtlichen Hinweisen
- [ ] Verwendung von realitätsnahen Namen (kein Lorem Ipsum)

### User Story 2: Datenschutzerklärung
**Als** Benutzer **möchte ich** die Datenschutzerklärung einsehen können.

**Akzeptanzkriterien:**
- [ ] "Privacy Policy" Bereich vorhanden
- [ ] Link führt zu Seite mit Informationen über Datensammlung, -verwendung und -schutz
