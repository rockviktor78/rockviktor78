hover fehlt auf dem :  contact-card

Checkliste vor Abgabe


1. Transition

Fazit: Die Regel wird im Projekt systematisch nicht eingehalten. Die meisten Transitions liegen bei 150ms - 600ms statt 75ms - 125ms.

2. Dateien sind richtig benannt: index.html, script.js, style.css
bei uns passt nur die index.html.

Aktuelles Problem: Die Aufteilung ist suboptimal:
script.js ist leer statt genutzt
Zwei separate utilities.js Dateien (scripts/ vs scripts/shared/)
Funktionalität ist über viele Dateien verstreut

Empfehlung laut Dokumentation: Allgemeine Funktionen sollten in script.js sein.

3. Es gibt einen console.log('Live reload enabled.'); board.html 144

