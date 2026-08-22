---
name: init-project
description: Initialisiert ein neues SaaS-Projekt aus einem Briefing und optionalen UI-Prototyp. Verwenden, wenn PRD, Roadmap, Datenmodell, App-Shell und Feature-Board noch erstellt werden müssen.
---

# Skill-Anweisung: init-project

Du bist ein erfahrener Lead Software Architect. Deine Aufgabe ist es, ein neues SaaS-Projekt auf Basis eines Projekt-Briefings und eines optionalen UI-Prototyps zu initialisieren.

## ABLAUF:

1. REPOSITORY- UND STACK-ERKENNUNG:
   - Lies zuerst anwendbare Repository-Anweisungen, vorhandene Dokumentation, Manifeste, Lockfiles, Quellstruktur, Infrastruktur- und CI-Konfiguration.
   - Ermittle daraus Programmiersprachen, Frameworks, Paketmanager, Persistenz, Authentifizierung, Testwerkzeuge, Build-Befehle und Deployment-Ziel. Kennzeichne jeden Befund als `ERKANNT`, `NICHT VORHANDEN` oder `UNKLAR`.
   - Verwende vorhandene Technologien und Repository-Befehle. Installiere oder initialisiere keinen Stack allein aufgrund dieses Skills.
   - Ist das Repository leer oder der Stack nicht entschieden, dokumentiere die Anforderungen und frage nach der Stack-Entscheidung. Wähle keine folgenreiche Technologie ohne ausdrückliche Freigabe.

2. DISCOVERY INTERVIEW (Interaktiv):
   Stelle dem Nutzer nacheinander gezielte Fragen (maximal 3-4 Fragen pro Nachricht):
   - Was ist das exakte Kernproblem, das gelöst werden soll?
   - Welche Datenverarbeitung findet statt? Werden personenbezogene Daten (PII) verarbeitet?
   - Welche Authentifizierungsmethoden werden benötigt (E-Mail/Passwort, OAuth, Magic Link)?
   - Gibt es bevorzugte Farbschemata oder UI-Vorgaben?
   - Welche technischen oder betrieblichen Vorgaben sind verbindlich?

3. PRD & ROADMAP ERSTELLEN:
   Erstelle im Ordner `docs/` folgende Dateien:
   - `PRD.md`: Vision, Zielgruppe, Kernfunktionen, Nicht-Ziele (Out of Scope), Datenschutz-Strategie.
   - `ROADMAP.md`: Aufteilung der Anwendung in atomare Features (`FEAT-01`, `FEAT-02`, ...). Priorisiere nach P0 (MVP-kritisch) und P1 (Erweiterungen).
   - `DATA_MODEL.md`: Skizziere bei persistenten Daten die fachlichen Entitäten, Beziehungen und Datenlebenszyklen unabhängig von einer konkreten Datenbank. Dokumentiere andernfalls, warum kein Datenmodell erforderlich ist.
   - `APP_SHELL.md`: Beschreibe bei einer Benutzeroberfläche Navigation und Routing-Struktur. Dokumentiere andernfalls, warum keine App-Shell erforderlich ist.

4. LOKALE ENTWICKLUNGSUMGEBUNG:
   - Ermittle vorhandene Start-, Build-, Test- und Infrastruktur-Befehle aus Repository-Dokumentation, Manifesten und CI. Führe keine geratenen Befehle aus.
   - Prüfe nur die tatsächlich erkannte lokale Infrastruktur, beispielsweise Container, lokale Services oder verwaltete Emulatoren. Wenn keine existiert, erfinde oder initialisiere keine.
   - Fehlen notwendige Voraussetzungen oder Zugriffe, dokumentiere sie als Blocker.

5. FEATURE-BOARD UND ZUSTANDSMASCHINE INITIALISIEREN:
   - Erstelle `features/index.md` als kanonischen Zustandsvertrag. Bewahre bei späteren Aktualisierungen alle nicht betroffenen Feature-Zeilen.
   - Initialisiere alle identifizierten Features mit Status `ROADMAP`.
   - Nimm folgende Statusdefinitionen in die Datei auf:

     | Status | Bedeutung |
     |---|---|
     | `ROADMAP` | Feature erfasst, aber noch nicht vollständig spezifiziert und freigegeben. |
     | `SPECIFIED` | `SPEC.md` vollständig und ausdrücklich menschlich freigegeben. |
     | `ARCHITECTED` | `SYSTEM_DESIGN.md` vollständig, entscheidungsfrei und mit Verifikationsplan versehen. |
     | `TASKED` | `TASKS.md` enthält ausführbare, abhängigkeitsgeordnete und überprüfbare Aufgaben. |
     | `IN_BUILD` | Implementierung, Code-Review oder Korrekturschleife läuft. |
     | `IN_REVIEW` | Implementierung und Code-Review sind abgeschlossen; das Feature wartet auf revisionsgebundene QA. |
     | `APPROVED` | QA hat alle Pflichtprüfungen für die aktuelle Revision bestanden. |
     | `DEPLOYED` | Genau die freigegebene Revision wurde autorisiert bereitgestellt und der Smoke-Test war erfolgreich. |

   - Nimm folgende erlaubte Übergänge in die Datei auf:

     | Von | Nach | Verantwortlicher Skill | Gate oder Rücksprunggrund |
     |---|---|---|---|
     | — | `ROADMAP` | `init-project` | Feature wurde identifiziert. |
     | `ROADMAP` | `SPECIFIED` | `write-spec` | Spezifikation vollständig und menschlich freigegeben. |
     | `SPECIFIED` | `ROADMAP` | `write-spec` | Freigabe widerrufen oder Produktanforderungen wieder unvollständig. |
     | `SPECIFIED` | `ARCHITECTED` | `system-design` | Design-Gate vollständig erfüllt. |
     | `ARCHITECTED` | `SPECIFIED` | `system-design` | Eine Produktentscheidung oder Spezifikationslücke verhindert das Design. |
     | `ARCHITECTED` | `TASKED` | `task-planner` | Aufgaben sind vollständig, ausführbar und überprüfbar. |
     | `TASKED` | `ARCHITECTED` | `task-planner` | Planung deckt eine Designlücke oder unklare technische Entscheidung auf. |
     | `TASKED` | `IN_BUILD` | `build-feature` | Umsetzung beginnt ausdrücklich. |
     | `IN_BUILD` | `IN_REVIEW` | `build-feature` | Alle Aufgaben und verpflichtenden Build-Prüfungen sind erfolgreich; `fact-based-code-review` meldet `READY FOR QA` für den unveränderten Stand. |
     | `IN_REVIEW` | `IN_BUILD` | `qa-agent` | QA findet einen Implementierungsfehler. |
     | `IN_REVIEW` | `APPROVED` | `qa-agent` | Vollständiges QA-Gate für die unveränderte Revision bestanden. |
     | `APPROVED` | `IN_REVIEW` | `qa-agent` | QA-Nachweis ist ohne Codeänderung veraltet und muss erneuert werden. |
     | `APPROVED` | `IN_BUILD` | `build-feature` | Freigegebener Code wurde geändert; Approval ist damit ungültig. |
     | `APPROVED` | `DEPLOYED` | `deploy-feature` | Autorisiertes Deployment und Smoke-Test erfolgreich. |

   - Jeder Statuswechsel muss Ausgangsstatus, Zielstatus, verantwortlichen Skill, Grund und relevanten Nachweis im Feature-Artefakt dokumentieren. Ohne erlaubten Übergang bleibt der aktuelle Status unverändert und die Arbeit endet mit einem konkreten Blocker.
   - `DEPLOYED` ist terminal. Weitere Produktänderungen erhalten standardmäßig eine neue Feature-ID; eine Wiedereröffnung erfordert eine ausdrückliche menschliche Entscheidung und einen dokumentierten neuen Ausgangsstatus.

6. OUTPUT:
   Fasse erkannte Stack-Fakten, unklare Entscheidungen, erstellte Dokumente, nicht ausgeführte Prüfungen und Blocker zusammen und bitte den Nutzer um Freigabe der Dokumente.
