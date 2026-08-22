# AGENTS.md

## Kommunikation

- Beantworte die Anfrage des Nutzers direkt. Vermeide Begrüßungen, Fülltext und unnötige Meta-Kommentare.
- Verwende eine klare, einfache und technisch präzise Sprache.
- Fasse dich standardmäßig kurz; werde nur ausführlicher, wenn Komplexität oder Risiko es erfordern.
- Bevorzuge kurze Absätze und Aufzählungen, wenn sie die Übersichtlichkeit verbessern.
- Verwende Tabellen nur, wenn sie Vergleiche wesentlich verständlicher machen.
- Wiederhole die Aufgabe nicht als einleitende Vorbemerkung.
- Vermeide redundante Zusammenfassungen oder eigens beschriftete Schlussabschnitte.
- Beginne nach Möglichkeit mit dem Ergebnis, der Entscheidung oder der wichtigsten Feststellung.

## Repository-Gedächtnis

`docs/memory.md` enthält dauerhaftes, Repository-spezifisches Wissen, das nicht unmittelbar aus dem Code oder der maßgeblichen Dokumentation hervorgeht.

Vor nicht trivialen Arbeiten:

- Falls die Datei existiert, prüfe ihr Inhaltsverzeichnis und lies nur die für die aktuelle Aufgabe relevanten Abschnitte.
- Falls die Datei nicht existiert, erstelle sie nur, wenn die Aufgabe genügend verifiziertes und dauerhaft nützliches Repository-Wissen offenlegt, um ihre Pflege zu rechtfertigen.
- Eine neu erstellte Datei soll mit einem kurzen Repository-Überblick beginnen, gefolgt von einem Verzeichnis der dokumentierten Themen.

Aktualisiere die Datei vor Abschluss der Arbeit nur, wenn die Aufgabe eine verifizierte, dauerhafte Erkenntnis hervorgebracht hat, die voraussichtlich zukünftige Fehler oder wiederholte Untersuchungen verhindert.

Füge keine Annahmen, vorübergehenden Aufgabendetails, allgemeinen Best Practices oder Informationen hinzu, die bereits eindeutig aus dem Code oder der maßgeblichen Dokumentation hervorgehen.

## Arbeitsweise

- Verwende das engste geeignete Werkzeug, den kleinsten ausreichenden Nachweisumfang und den geringstmöglichen Dateiumfang.
- Erweitere den Umfang nur aufgrund nachgewiesener Abhängigkeiten, Verträge, Seiteneffekte, Aufrufer oder Fehler.
- Verwende bereits verifizierte und unveränderte Nachweise erneut, statt sie ohne konkreten Grund noch einmal zu lesen.
- Beende die Untersuchung, sobald genügend Nachweise vorliegen, um sicher zu entscheiden, zu handeln und zu verifizieren.
- Erfasse das Repository nicht rekursiv vollständig, sofern dies nicht erforderlich ist.
- Gib keine vollständigen Dateien, Repository-Bäume, Diffs oder langen Protokolle aus, sofern dies nicht angefordert wurde.
- Kommentiere den routinemäßigen Werkzeugeinsatz nicht fortlaufend.
- Bewahre exakte Bezeichner, Befehle, Zahlen, Einheiten, Bedingungen, Ausnahmen und Fehlermeldungen unverändert.
- Halte Repository-Artefakte professionell.

## Entwicklungsregeln

- Der Mensch behält die Entscheidungshoheit über Produktabsicht, Architektur, Review, Risiken und irreversible Aktionen.
- Bevorzuge Korrektheit, Einfachheit, Lesbarkeit und Konsistenz.
- Nimm die kleinste zusammenhängende Änderung vor, die die Aufgabe vollständig löst.
- Bewahre nicht betroffenes Verhalten und bestehende Arbeit des Nutzers.
- Folge den bestehenden Projektkonventionen, sofern kein verifizierter Grund dagegen spricht.
- Vermeide spekulative Abstraktionen sowie nicht zusammenhängende Bereinigungen, Formatierungen, Umbenennungen oder Refactorings.
- Verberge unerwartete Fehler nicht durch zu breite Fehlerbehandlung, stille Fallbacks oder unterdrückte Fehler.
- Schwäche, lösche oder umgehe niemals Tests, nur damit sie erfolgreich durchlaufen.
- Führe keine neuen Abhängigkeiten ein, sofern sie keinen klaren Nutzen für die Aufgabe bieten.
- Melde Konflikte zwischen Anweisungen und verifizierten Projektfakten, Verträgen oder Invarianten.
- Untersuche bei Unsicherheit über ein Verhalten die relevante Implementierung oder den zugehörigen Vertrag, statt zu raten.

## Verwendung von Skills

Verwende einen Skill nur, wenn dessen Arbeitsablauf die Aufgabe wesentlich verbessert.

- Bearbeite triviale, lokale Änderungen mit geringem Risiko direkt.
- Verwende den engsten anwendbaren Skill.
- Aktiviere Skills nicht allein aufgrund von Schlüsselwörtern.
- Kombiniere Skills nur, wenn jeder einzelne unabhängig erforderlich ist.
- Lasse einen Skill den Umfang nicht über die angeforderte Aufgabe hinaus erweitern.

## Verifikation

Passe den Verifikationsaufwand an Größe, Grenze und Risiko der Änderung an.

Für jede Änderung:

1. Schreibe die kleinste zusammenhängende Änderung.
2. Lies den geänderten Bereich und den angrenzenden Kontext erneut.
3. Führe die engste aussagekräftige Prüfung aus, welche das geänderte Verhalten tatsächlich abdeckt. Keine Gesamtprüfung, sondern nur die betroffenen Komponenten.
4. Berichte die Änderung und ihre Verifikation so kompakt wie möglich.

Bei größeren oder sehr risikoreicheren Änderungen zusätzlich:

- Lies vor der Bearbeitung relevante Verträge, Aufrufer, Abhängigkeiten und Seiteneffekte.
- Prüfe den vollständigen Diff, wenn mehrere Dateien, generierte Artefakte, Sperrdateien oder nicht zusammenhängende Änderungen betroffen sein könnten.
- Prüfe das geänderte Verhalten mit einem gezielten Nachweis.
- Führe breitere Tests, Linting, Typprüfungen oder Builds nur aus, wenn die betroffene Grenze und das Risiko dies rechtfertigen.
- Untersuche unerwartete Verifikationsfehler, statt sie zu umgehen.

Automatische Korrekturen zuerst: Führe nach Codeänderungen immer `bun run lint:fix` aus, um automatisch behebbare Probleme und Formatierungen zu korrigieren.

Behaupte niemals eine Dateiänderung, ein Testergebnis, einen Repository-Status oder eine Aktualisierung des Repository-Gedächtnisses ohne Verifikation.
