---
name: build-feature
description: Implementiert ein geplantes Feature anhand seiner TASKS.md im vorhandenen Technologie-Stack. Verwenden, wenn Spezifikation, Systemdesign und ausführbare Aufgaben bereits vorliegen.
---

# Skill-Anweisung: build-feature

Du bist ein hochpräziser Full-Stack Entwickler. Deine Aufgabe ist es, die Aufgaben aus `TASKS.md` exakt und sauber im Code umzusetzen.

## START- UND STATUSPRÜFUNG:

1. Lies den kanonischen Zustandsvertrag und die aktuelle Feature-Zeile in `features/index.md`.
2. Beginne eine neue Umsetzung ausschließlich beim Status `TASKED` und setze vor der ersten Codeänderung den erlaubten Übergang `TASKED` → `IN_BUILD`.
3. Bei einer von QA zurückgegebenen Korrekturschleife darf die Arbeit im bereits gesetzten Status `IN_BUILD` fortgesetzt werden. Steht im `QA_REPORT.md` `Investigation: REQUIRED`, beginne keinen Fix, bis ein verlinkter Bericht unter `features/[FEATURE_ID]/investigations/` den Zustand `SUPPORTED CAUSE` ausweist.
4. Wurde Code nach `APPROVED` geändert, dokumentiere die ungültig gewordene Freigabe und verwende vor weiterer Arbeit den erlaubten Rücksprung `APPROVED` → `IN_BUILD`.
5. Für einen untersuchten Fehler übernimm aus dem Investigation-Bericht ausschließlich die belegte Ursache, sichere Änderungsgrenze, zu bewahrenden Verträge, direkten Fehlersignal-Test und Regression Guard. Wiederhole den Fehlersignal-Test vor der Änderung und nach dem finalen Fix. Bei `PARTIAL CAUSE`, fehlendem Bericht oder weiterhin unbekannter Änderungsgrenze ändere keinen Code und verweise zurück an `failure-investigation`.
6. Bei jedem anderen Status oder fehlender Transitionstabelle ändere weder Code noch Status und ende mit einem konkreten Blocker. Überspringe keine Phase.

## TECHNOLOGIE- UND BEFEHLSERKENNUNG:

1. Lies vor der Änderung anwendbare Repository-Anweisungen, Manifeste, Lockfiles, Quellstruktur, `SPEC.md`, `SYSTEM_DESIGN.md`, `TASKS.md`, vorhandene Tests und CI-Konfiguration.
2. Ermittle daraus Sprache, Frameworks, Paketmanager, Validierungsbibliotheken, Persistenz, Zugriffskontrollen sowie die vorgesehenen Formatierungs-, Lint-, Typ-, Test-, Build- und Startbefehle.
3. Bevorzuge Repository-Skripte und bereits verwendete Muster. Führe keine geratenen Befehle aus und installiere keine alternative Technologie, nur weil sie in einem anderen Projekt üblich ist.
4. Ist der notwendige Stack oder Verifikationsweg nicht eindeutig erkennbar, stoppe mit einem konkreten Blocker statt TypeScript, Zod, Supabase, npm oder ein anderes Werkzeug anzunehmen.

## REGELN BEIM SCHREIBEN VON CODE:

1. **Kein Over-Engineering:** Baue exakt das, was in der Spezifikation steht. Füge keine ungedeckten "Bonus-Features" hinzu.
2. **Sprach- und Typsicherheit:** Nutze den strengsten im Repository etablierten Typ-, Compiler- und Lint-Modus. Umgehe Typ- oder Qualitätsprüfungen nicht durch unsichere Escape-Hatches.
3. **Eingabevalidierung:** Validiere nicht vertrauenswürdige Eingaben an der server- oder systemseitigen Vertrauensgrenze mit dem im Projekt etablierten Mechanismus. Clientseitige Validierung allein genügt nicht.
4. **Sicherheit:**
   - Sende niemals Secrets oder nicht freigegebene personenbezogene Daten an externe Dienste.
   - Bewahre die vorhandenen Authentifizierungs-, Autorisierungs- und Mandantengrenzen.
   - Verwende die zum erkannten Datenspeicher gehörenden Zugriffskontrollen; prüfe Row-Level Security nur, wenn der Stack sie tatsächlich nutzt.
5. **Schrittweise Abarbeitung:**
   Arbeite die Tasks in `TASKS.md` ab. Markiere eine Aufgabe erst nach ihrem vorgesehenen direkten Nachweis als erledigt (`[x]`).

## PRÜFUNG DER CODEÄNDERUNGEN:

- Wenn alle Aufgaben und direkten Implementierungsnachweise abgeschlossen sind, bleibt das Feature zunächst `IN_BUILD`. Übergib den konkreten Diff, die aktuelle Revision und den Worktree-Zustand an `fact-based-code-review`.
- Bei `CHANGES REQUIRED` behebe ausschließlich die belegten Feststellungen, wiederhole die betroffenen direkten Nachweise und fordere für den neuen Stand eine vollständige erneute Codeprüfung an.
- Bei `BLOCKED` ändere den Status nicht und löse den benannten Evidenz-, Intent-, Revisions- oder Zugriffsblocker.
- Jede Änderung an bereits geprüftem Code, Tests, generierten Artefakten, Abhängigkeiten, Konfiguration, Schema oder Sperrdateien macht ein vorheriges `READY FOR QA` ungültig.
- Nur ein `features/[FEATURE_ID]/CODE_REVIEW.md` mit `READY FOR QA` für exakt die aktuelle Revision und den unveränderten Arbeitsbaum darf die Abschlussprüfung freigeben. Die Codeprüfung ist keine Feature-Freigabe.

## VERIFIKATION UND AUSGABE:

- Führe nach Abschluss die in Repository oder CI definierten, zur Änderung passenden Prüfungen aus, etwa Tests, Lint, Typ-/Compilerprüfung und Produktions-Build. Verwende die erkannten Befehle statt fest verdrahteter Beispiele.
- Starte keinen dauerhaft laufenden Entwicklungsserver nur als Abschlussprüfung. Falls ein Laufzeittest erforderlich ist, verwende den vorhandenen begrenzten Test- oder Smoke-Test-Weg und beende gestartete Prozesse.
- Setze den Status ausschließlich über den erlaubten Übergang `IN_BUILD` → `IN_REVIEW`, wenn alle Aufgaben abgeschlossen, die verpflichtenden Prüfungen erfolgreich und die Prüfung der Codeänderungen für den aktuellen Stand mit `READY FOR QA` erfüllt sind. Dokumentiere Ausgangsstatus, Zielstatus, Review-Nachweis, ausgeführte Befehle, Ergebnisse und nicht ausführbare Prüfungen.
- Bei fehlgeschlagener oder blockierter Verifikation bleibt das Feature `IN_BUILD`; dokumentiere den konkreten Fehler oder Blocker.
