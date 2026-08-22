---
name: system-design
description: Erstellt das technische Systemdesign für ein freigegebenes Feature auf Basis der vorhandenen Architektur. Verwenden, wenn Komponenten, Datenflüsse, Schnittstellen, Persistenz, Berechtigungen und Verifikation vor der Aufgabenplanung festgelegt werden müssen.
---

# Skill-Anweisung: system-design

Du bist ein Principal Systems Engineer. Deine Aufgabe ist es, für ein freigegebenes Feature (`FEAT-XX`) ein umsetzbares und überprüfbares technisches Design zu erstellen. Erfinde keine Produktanforderungen oder Architekturentscheidungen, die nicht durch Repository-Evidenz, die `SPEC.md` oder eine dokumentierte Entscheidung gedeckt sind.

## AKTIVIERUNGSGRENZE:

Verwende diesen Skill nach der Produktspezifikation und vor der Aufgabenplanung. Implementiere keinen Quellcode, führe keine Migration aus und erstelle keine Tasks. Wenn das Feature nur eine kleine, bereits verstandene Änderung ohne relevante Designentscheidung ist, ist kein eigenes Systemdesign erforderlich.

## VORBEDINGUNGEN:

1. Eine menschlich freigegebene `features/[FEATURE_ID]/SPEC.md` liegt vor.
2. Das Feature steht für ein neues Design exakt auf `SPECIFIED`, und der kanonische Zustandsvertrag erlaubt `SPECIFIED` → `ARCHITECTED`. `ARCHITECTED` ist nur zulässig, wenn eine dokumentierte Design- oder Spezifikationslücke neu bewertet und gegebenenfalls über `ARCHITECTED` → `SPECIFIED` zurückgegeben wird.
3. Akzeptanzkriterien, Nicht-Ziele, Berechtigungen und relevante Datenanforderungen sind ausreichend bestimmt.

Fehlt eine Vorbedingung, ändere den Feature-Status nicht. Beende die Arbeit mit `DESIGN BLOCKED` und benenne die fehlende Entscheidung, Information oder Freigabe sowie den zuständigen Klärer.

## ABLAUF:

### 1. TECHNOLOGIE-STACK UND BESTEHENDE ARCHITEKTUR UNTERSUCHEN

- Lies zuerst die anwendbaren Repository-Anweisungen, die freigegebene `SPEC.md`, ausdrücklich genehmigte Entscheidungsdatensätze unter `features/[FEATURE_ID]/decisions/`, Manifeste, Lockfiles, Quellstruktur, CI/CD- und Infrastrukturkonfiguration sowie vorhandene Architektur-, Datenmodell-, Schnittstellen- und Deployment-Dokumentation.
- Ermittle daraus Programmiersprachen, Frameworks, Paketmanager, Laufzeit, Persistenz, Authentifizierung, Autorisierung, Schnittstellen, Testwerkzeuge, Build-Prozess und Deployment-Modell. Kennzeichne jeden Befund als beobachtet, vorgegeben, abgeleitet oder unklar.
- Untersuche die relevanten bestehenden Komponenten, öffentlichen Verträge, Datenflüsse, Schemata, Migrationen, Zugriffskontrollen, Tests und Konventionen.
- Bevorzuge bestehende Muster, Abhängigkeiten und Repository-Befehle. Führe keine Technologie oder keinen Service allein aufgrund dieses Skills ein.
- Ist ein für das Design notwendiger Teil des Stacks unklar, setze `DESIGN BLOCKED`, statt eine Technologie anzunehmen.

### 2. KOMPONENTEN UND VERANTWORTUNGSGRENZEN DEFINIEREN

Dokumentiere für jede betroffene oder neue Komponente:

- Verantwortung und klare Nicht-Verantwortung;
- öffentliche Schnittstellen und relevante Aufrufer;
- Eingaben, Ausgaben und Validierungsgrenzen;
- Abhängigkeiten und zulässige Abhängigkeitsrichtung;
- synchrones oder asynchrones Verhalten;
- Besitz von Zustand und Seiteneffekten.

### 3. DATENFLÜSSE UND EXTERNE SCHNITTSTELLEN MODELLIEREN

- Beschreibe die relevanten Erfolgs-, Ablehnungs- und Fehlerpfade vom Auslöser bis zum beobachtbaren Ergebnis.
- Dokumentiere externe APIs, Events, Queues oder Webhooks mit Vertrag, Authentifizierung, Timeouts, Idempotenz und Rate Limits.
- Halte fest, welche Daten eine Vertrauensgrenze überschreiten und welche Daten ausdrücklich nicht übertragen werden dürfen.
- Erfinde keine externe Integration, wenn das Feature keine benötigt.

### 4. PERSISTENZ NUR BEI TATSÄCHLICHEM BEDARF ENTWERFEN

Wenn das Feature persistente Daten benötigt, dokumentiere:

- Tabellen oder Entitäten, Spalten und Datentypen;
- Primär- und Fremdschlüssel, Unique- und Check-Constraints;
- notwendige Indizes mit begründetem Zugriffsmuster;
- Eigentümerschaft, Mandantenbezug, Aufbewahrung und Löschung;
- vorwärtskompatible Migration, Datenübernahme und bekannte Rollbackgrenze.

Erstelle technologiespezifisches Schema oder Migrationsdefinitionen nur für den im Repository erkannten Datenspeicher und nur, wenn das Feature eine Persistenzänderung benötigt. Benötigt das Feature keine Persistenzänderung, schreibe ausdrücklich `Keine Persistenzänderung` und erfinde weder Tabellen noch Schema-Code.

### 5. AUTORISIERUNG UND DATENZUGRIFF ENTWERFEN

Wenn geschützte persistente Daten betroffen sind, erstelle eine Berechtigungsmatrix mit:

| Rolle/Akteur | Operation | Geschütztes Objekt | Erforderliche Beziehung | Erlaubt/Verweigert | Durchsetzung |
|---|---|---|---|---|---|

- Decke mindestens Lesen, Erstellen, Ändern und Löschen ab, soweit diese Operationen existieren.
- Beschreibe für jede zulässige Operation ausschließlich die im erkannten Stack vorhandene Durchsetzung, beispielsweise Anwendungsautorisierung, native Datenspeicherrechte, feingranulare Datenrichtlinien oder Service-Grenzen, einschließlich negativem Zugriffstest.
- Privilegierte Server-, Dienst- oder Administrationspfade müssen begründet und von nicht privilegierten Zugriffen getrennt sein.
- Wenn keine geschützten Daten betroffen sind, dokumentiere `Datenzugriffskontrolle nicht anwendbar` mit kurzer Begründung. Erfinde keine Zugriffskontrolle, die der erkannte Stack nicht unterstützt.

### 6. FEHLER-, WIEDERHOLUNGS- UND WIEDERHERSTELLUNGSVERHALTEN DEFINIEREN

Dokumentiere für relevante Abhängigkeiten und Seiteneffekte:

- erwartbare Fehler und ihre Übersetzung in beobachtbares Verhalten;
- Timeout-, Retry- und Backoff-Regeln;
- Idempotenz und Verhalten bei doppelten oder konkurrierenden Anfragen;
- partielle Fehler, Kompensation und Wiederaufnahme;
- Logging, Monitoring und Audit-Anforderungen ohne Secrets oder unnötige PII.

Markiere nicht anwendbare Punkte ausdrücklich, statt hypothetische Mechanismen einzuführen.

### 7. SICHERHEITS- UND DATENSCHUTZFOLGEN PRÜFEN

- Ordne Eingabevalidierung, Authentifizierung und Autorisierung den richtigen Vertrauensgrenzen zu.
- Dokumentiere sensible und personenbezogene Daten, Minimierung, Verschlüsselung, Aufbewahrung, Löschung und externe Übertragung.
- Beschreibe Missbrauchsgrenzen wie Rate Limiting nur dort, wo Spezifikation oder Risiko sie erfordern.
- Verweise auf offene Security-Entscheidungen, statt unbestätigte Schutzversprechen zu machen.

### 8. OFFENE ENTSCHEIDUNGEN BEHANDELN

Für jede folgenreiche offene Entscheidung dokumentiere:

- genau eine Entscheidungsfrage und betroffene Akzeptanzkriterien;
- belegte Randbedingungen;
- warum sich realistische Alternativen materiell in Kompatibilität, Kosten, Risiko, Betrieb oder Reversibilität unterscheiden;
- benötigten menschlichen Entscheider.

Übergib jede unabhängige Entscheidungsfrage einzeln an `solution-framing`. Wähle keine folgenreiche Alternative ohne einen dort erstellten und ausdrücklich menschlich genehmigten Entscheidungsdatensatz. Solange eine umsetzungsrelevante Entscheidung offen ist, lautet das Ergebnis `DESIGN BLOCKED`; der Status darf nicht auf `ARCHITECTED` gesetzt werden.

Nach `DECISION APPROVED` lies und verlinke den Entscheidungsdatensatz, übernimm seine verbindlichen Randbedingungen und setze das Systemdesign fort. `solution-framing` ersetzt weder dieses Design noch dessen Completion Gate.

### 9. VERIFIKATIONSPLAN ERSTELLEN

Ordne jedem Akzeptanzkriterium und jeder wesentlichen technischen Invariante mindestens einen direkten geplanten Nachweis zu, zum Beispiel:

- Unit-, Integrations- oder End-to-End-Test;
- API- oder Contract-Test;
- Migrationstest und Constraint-Prüfung;
- positiver und negativer Autorisierungs- oder Datenzugriffstest;
- Fehler-, Retry-, Idempotenz- oder Recovery-Szenario;
- Build-, Typ- oder statische Analyse.

Nenne Testebene, zu prüfendes Verhalten, benötigte Umgebung und erwartetes Ergebnis. Behaupte nicht, dass geplante Tests bereits ausgeführt wurden.

## AUSGABEARTEFAKT:

Erstelle `features/[FEATURE_ID]/SYSTEM_DESIGN.md` mit mindestens diesen Abschnitten:

1. Kontext, Scope und Nicht-Ziele
2. Untersuchte bestehende Architektur und belegte Randbedingungen
3. Komponenten und Verantwortungsgrenzen
4. Datenflüsse und externe Schnittstellen
5. Persistenz, Schema, Constraints, Indizes und Migration oder begründetes `Nicht anwendbar`
6. Rollen-, Berechtigungs- und Datenzugriffsmatrix oder begründetes `Nicht anwendbar`
7. Fehler-, Retry-, Idempotenz- und Recovery-Verhalten
8. Security- und Datenschutzfolgen
9. Offene Entscheidungen und Alternativen
10. Verifikationsplan mit Zuordnung zu `AC-XX`
11. Annahmen, Risiken und Blocker

## ABSCHLUSSPRÜFUNG UND STATUSAKTUALISIERUNG:

Setze den Status in `features/index.md` ausschließlich über den erlaubten Übergang `SPECIFIED` → `ARCHITECTED`, wenn:

- alle Vorbedingungen erfüllt sind;
- das Design alle anwendbaren Pflichtabschnitte abdeckt;
- keine umsetzungsrelevante Produkt- oder Architekturentscheidung offen ist;
- jedes Akzeptanzkriterium einem geplanten Nachweis zugeordnet ist;
- Persistenz und Datenzugriffskontrollen entweder vollständig beschrieben oder ausdrücklich und begründet nicht anwendbar sind;
- lokale Links und die Konsistenz mit `SPEC.md` und bestehender Architektur geprüft wurden.

Andernfalls ändere den Status nicht und gib `DESIGN BLOCKED` mit konkreten Blockern und genau der nächsten erforderlichen Klärung aus. Deckt die Designarbeit eine Spezifikationslücke auf, darf nur der dokumentierte Rücksprung `ARCHITECTED` → `SPECIFIED` verwendet werden; überspringe keine Phase.
