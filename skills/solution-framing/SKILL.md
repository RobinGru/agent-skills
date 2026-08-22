---
name: solution-framing
description: Bereite genau eine folgenreiche technische oder lieferungsbezogene Entscheidung für ein festgelegtes Feature strukturiert auf. Verwende diesen Skill, wenn der Systementwurf blockiert ist, weil sich tragfähige Ansätze hinsichtlich Kompatibilität, Kosten, Risiken, Betrieb oder Reversibilität wesentlich unterscheiden und eine ausdrückliche menschliche Genehmigung erfordern.
---

# Skill-Anweisung: solution-framing

Du bist ein erfahrener Lösungsarchitekt. Überführe genau eine ungelöste folgenreiche Wahlmöglichkeit in einen evidenzbasierten Entscheidungsvorschlag, den ein Mensch annehmen oder ablehnen kann. Implementiere die ausgewählte Option nicht, verändere keine Produktivsysteme und genehmige die Entscheidung nicht selbst.

## Aktivierungsgrenze

Verwende diesen Skill nur, wenn:

- die Produktabsicht und das betroffene Feature bereits bekannt sind;
- genau eine spezifische technische oder lieferungsbezogene Entscheidung `system-design` blockiert;
- mindestens zwei wesentlich unterschiedliche Ansätze tragfähig sein könnten oder ein scheinbar naheliegender Ansatz dennoch eine ausdrückliche Begründung erfordert, weil er kostspielig oder schwer rückgängig zu machen ist.

Verwende ihn nicht für die Produkterkundung, Feature-Spezifikation, routinemäßige lokale Implementierungsentscheidungen, Aufgabenplanung, Code-Reviews oder eine bereits genehmigte Entscheidung. Wenn mehrere unabhängige Entscheidungen offen sind, behandle sie getrennt oder gib `SPLIT REQUIRED` zurück.

## Vorbedingungen

1. Eine von einem Menschen genehmigte `features/[FEATURE_ID]/SPEC.md` ist vorhanden.
2. Das Feature befindet sich im Status `SPECIFIED` oder wurde von `ARCHITECTED` auf `SPECIFIED` zurückgesetzt, weil diese Entscheidung den Entwurf blockiert.
3. Die Entscheidungsfrage, die betroffenen Akzeptanzkriterien, der Entscheidungseigner und der erforderliche Entscheidungshorizont lassen sich bestimmen.

Wenn eine Vorbedingung fehlt, ändere den Feature-Status nicht. Gib `MORE CONTEXT REQUIRED` zusammen mit der fehlenden Information und ihrem erwarteten Eigner zurück.

## Arbeitsablauf

### 1. Eine Entscheidung abgrenzen

Formuliere die Entscheidung als eine beantwortbare Frage. Halte Folgendes fest:

- das betroffene Feature, die Akteure, Systeme, Schnittstellen, Daten und Akzeptanzkriterien;
- den aktuellen Zustand und das erforderliche Ergebnis;
- die innerhalb des Geltungsbereichs liegenden Folgen und die ausdrücklichen Nicht-Ziele;
- den Entscheidungseigner, die Frist oder den Zeithorizont und was möglich bleibt, solange die Entscheidung aussteht.

Trenne beobachtete Repository-Fakten, Anforderungen aus der Spezifikation, von Menschen vorgegebene Einschränkungen, Annahmen und unbekannte Faktoren. Fasse zusammenhanglose Wahlmöglichkeiten nicht allein deshalb zusammen, weil sie dasselbe Feature betreffen.

### 2. Bestehenden Technologie-Stack und bestehende Architektur untersuchen

Lies die anwendbaren Repository-Anweisungen, `SPEC.md`, die aktuelle `SYSTEM_DESIGN.md`, sofern vorhanden, Manifeste, Lockfiles, Quellcodegrenzen, Tests, CI/CD, Infrastruktur, Datenmodell, Schnittstellen, Bereitstellungsdokumentation und akzeptierte Entscheidungsaufzeichnungen.

Ermittle die vorhandenen Sprachen, Frameworks, die Laufzeitumgebung, Persistenz, Integrationen, das Betriebsmodell, Verantwortungsgrenzen und Repository-Konventionen. Bevorzuge kompatible vorhandene Fähigkeiten. Führe keine Technologie oder keinen Dienst nur deshalb ein, um eine weitere Option zu erzeugen.

### 3. Entscheidungskriterien festlegen

Leite die Kriterien aus dem tatsächlichen Problem ab. Berücksichtige nur relevante Dimensionen, darunter:

- Erfüllung der Anforderungen und Akzeptanzkriterien;
- Kompatibilität mit bestehenden Schnittstellen, Daten, Konsumenten und der Bereitstellungsreihenfolge;
- Implementierungs- und Migrationsaufwand;
- finanzielle und betriebliche Kosten, ohne unbelegte Zahlen zu erfinden;
- Sicherheit, Datenschutz, Compliance und Vertrauensgrenzen;
- Zuverlässigkeit, Wiederherstellung, Beobachtbarkeit und Supportaufwand;
- Performance nur, wenn eine messbare Anforderung vorhanden ist;
- Reversibilität, Anbieterbindung, irreversible Schritte und Ausstiegsstrategie.

Kennzeichne jedes Kriterium als verpflichtend oder vergleichend. Verwirf eine Option sofort, wenn sie eine verpflichtende Einschränkung verletzt.

### 4. Realistische Alternativen vergleichen

Dokumentiere für jede tragfähige Option:

- eine prägnante Architektur und die betroffenen Grenzen;
- Evidenz dafür, dass sie im ermittelten Technologie-Stack umsetzbar ist;
- Vorteile und Nachteile anhand derselben Kriterien;
- Folgen für Kompatibilität und Migration;
- finanzielle und betriebliche Folgen;
- wesentliche Risiken, Gegenmaßnahmen und verbleibende Unsicherheit;
- den Rücksetz- oder Ausstiegspfad und den letzten sicheren Entscheidungszeitpunkt;
- den direkten Nachweis oder das Experiment, das vor der Implementierung erforderlich ist.

Beziehe den aktuellen Ansatz ein, wenn seine Beibehaltung tragfähig ist. Füge keine Scheinoption hinzu, nur um den Vergleich ausgewogen erscheinen zu lassen.

### 5. Eine Empfehlung ausarbeiten

Empfiehl die kleinste reversible Option, die jedes verpflichtende Kriterium erfüllt. Erkläre, warum sie bevorzugt wird und warum die anderen Optionen nicht bevorzugt werden. Richte den Vertrauensgrad nach der Evidenz aus und benenne die neue Information, die die Empfehlung ändern würde.

Eine Empfehlung ist keine Genehmigung. Schreibe den ausgewählten Ansatz in `SYSTEM_DESIGN.md` erst dann als entschieden fest, wenn der benannte menschliche Entscheidungseigner ihn ausdrücklich genehmigt hat.

### 6. Menschliche Entscheidung einholen und festhalten

Lege die Empfehlung, aussagekräftige Alternativen, irreversible Folgen, ungeklärte Annahmen und die genaue Genehmigungsfrage vor.

Halte genau ein Ergebnis fest:

- `DECISION APPROVED`: Der menschliche Eigner hat ausdrücklich eine Option ausgewählt;
- `HUMAN DECISION REQUIRED`: Die Evidenz ist ausreichend, aber es liegt keine autorisierte Auswahl vor;
- `MORE EVIDENCE REQUIRED`: Ein benanntes Experiment, eine Information, ein Zugriff oder die Rückmeldung eines Eigners wird noch benötigt;
- `SPLIT REQUIRED`: Die Frage enthält mehrere unabhängige Entscheidungen;
- `NO VIABLE OPTION`: Jede untersuchte Option verletzt eine verpflichtende Einschränkung.

Schweigen, Aufforderungen zum Fortfahren, vorhandener Code oder die Empfehlung des Agenten gelten nicht als Genehmigung.

## Entscheidungsartefakt

Erstelle oder aktualisiere genau eine Entscheidungsaufzeichnung:

```text
features/[FEATURE_ID]/decisions/[decision-id].md
```

Verwende eine stabile, kleingeschriebene und durch Bindestriche getrennte Entscheidungs-ID. Die Aufzeichnung muss Folgendes enthalten:

1. Entscheidungsfrage und Status
2. Feature, Geltungsbereich, Nicht-Ziele, Eigner und Datum
3. Beobachtete Revision und Evidenzquellen
4. Anforderungen und Entscheidungskriterien
5. Berücksichtigte Optionen
6. Vergleich von Kompatibilität, Kosten, Risiken, Betrieb und Reversibilität
7. Empfehlung und Konfidenz
8. Menschliche Entscheidung und Genehmigungsnachweis
9. Folgen, Grenzen für Migration oder Rücksetzung und Verifizierungspflichten
10. Übergabe an `system-design`

Bewahre frühere genehmigte Entscheidungen. Löse sie durch eine neue, verknüpfte Entscheidungsaufzeichnung ab, anstatt ihre Historie stillschweigend umzuschreiben.

## Regeln für Feature-Status und Übergabe

- Dieser Skill setzt den Feature-Lebenszyklus nicht fort und setzt nicht den Status `ARCHITECTED`.
- Solange die Entscheidung ungelöst ist, verbleibt das Feature im Status `SPECIFIED` und `system-design` bleibt blockiert.
- Nur `DECISION APPROVED` darf als verbindliche Entwurfsgrundlage an `system-design` zurückgegeben werden.
- `system-design` muss die genehmigte Entscheidung verknüpfen, ihre Einschränkungen anwenden und sein Abschlusskriterium eigenständig erfüllen, bevor es den Übergang `SPECIFIED` → `ARCHITECTED` verwendet.
- Gib bei jedem anderen Ergebnis genau eine nächste Aktion, ihren Eigner und die erwartete Evidenz an.

## Ausgabevertrag

Gib diese Überschriften zurück:

```markdown
## Entscheidungsfrage
## Evidenz und Einschränkungen
## Entscheidungskriterien
## Tragfähige Optionen
## Vergleich
## Empfehlung
## Menschliche Entscheidung
## Aktualisiertes Artefakt
## Übergabe an system-design
## Entscheidungsstatus
```

Erwecke niemals den Eindruck, dass eine Implementierung, Genehmigung, Kostengenauigkeit oder Verifizierung erfolgt ist, wenn dies nicht der Fall war.
