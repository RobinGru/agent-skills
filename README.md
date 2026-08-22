# Agent Skills

Dieses Repository enthält zehn eigene, deutschsprachige Zed Agent Skills für einen dokumentierten Feature-Arbeitsablauf von der Projektinitialisierung bis zur Bereitstellung.

Die Skills sind technologieneutral: Sie untersuchen zuerst Repository-Anweisungen, Manifeste, Sperrdateien, CI/CD und vorhandene Konventionen. Sie setzen weder TypeScript noch Supabase, npm oder einen bestimmten Hosting-Anbieter voraus.

## Eigene Skills

| Skill | Zweck |
|---|---|
| [`init-project`](skills/init-project/SKILL.md) | Klärt Produkt und technischen Kontext und erstellt die initialen Planungsartefakte. |
| [`write-spec`](skills/write-spec/SKILL.md) | Erstellt eine testbare Feature-Spezifikation mit Akzeptanzkriterien im `Given / When / Then`-Format. |
| [`solution-framing`](skills/solution-framing/SKILL.md) | Vergleicht genau eine folgenreiche technische Entscheidung und übergibt nur eine ausdrücklich menschlich genehmigte Auswahl an das Systemdesign. |
| [`system-design`](skills/system-design/SKILL.md) | Entwirft ein stackgerechtes Systemdesign mit Komponenten, Datenflüssen, Zugriffskontrollen und Verifikationsplan. |
| [`task-planner`](skills/task-planner/SKILL.md) | Zerlegt Spezifikation und Systemdesign in nachvollziehbare Implementierungsaufgaben. |
| [`build-feature`](skills/build-feature/SKILL.md) | Implementiert die geplanten Aufgaben mit den im Repository erkannten Werkzeugen und Prüfungen. |
| [`fact-based-code-review`](skills/fact-based-code-review/SKILL.md) | Prüft eine konkrete Änderung auf belegte Defekte und Integrationsrisiken, ohne Code oder Feature-Status zu verändern. |
| [`failure-investigation`](skills/failure-investigation/SKILL.md) | Reproduziert und reduziert Fehler mit unbekannter Ursache, prüft konkurrierende Hypothesen und übergibt nur eine belegte sichere Änderungsgrenze an die Umsetzung. |
| [`qa-agent`](skills/qa-agent/SKILL.md) | Prüft Akzeptanzkriterien und Sicherheitsanforderungen revisionsgebunden, ohne Quellcode zu reparieren. |
| [`deploy-feature`](skills/deploy-feature/SKILL.md) | Plant und führt eine ausdrücklich autorisierte Bereitstellung mit Probelauf-, Migrations-, Wiederherstellungs- und Kurzprüfung durch. |

Die Beschreibungen, Überschriften und Anweisungstexte aller eigenen Skills sind deutsch. Skill-Namen, Dateipfade und fest definierte Statuswerte bleiben als stabile technische Identifikatoren unverändert.

Jeder Skill liegt im Zed-kompatiblen Format vor:

```text
skills/<skill-name>/SKILL.md
```

## Installation in Zed

Kopiere die gewünschten vollständigen Skill-Verzeichnisse an einen von Zed erkannten Ort:

- global: `~/.agents/skills/<skill-name>/SKILL.md`
- projektbezogen: `<projekt>/.agents/skills/<skill-name>/SKILL.md`

Beispiel für eine globale Installation:

```sh
cp -R skills/system-design ~/.agents/skills/system-design
cp -R skills/qa-agent ~/.agents/skills/qa-agent
```

Vorhandene Zielverzeichnisse nicht ungeprüft überschreiben. Starte anschließend eine neue Zed-Konversation und kontrolliere, ob die installierten Skills im Skill-Katalog erscheinen.

`deploy-feature` hat `disable-model-invocation: true` und wird aus Sicherheitsgründen nur manuell aktiviert.

Die ebenfalls deutschsprachige [`AGENTS.md`](AGENTS.md) enthält die übergeordneten Arbeits-, Kommunikations- und Verifikationsregeln dieses Repositorys. Sie verlangt deutsche Antworten und legt derzeit Englisch für Code, Kommentare und andere Repository-Artefakte fest. Zielprojekte können durch eigene, enger geltende Anweisungen abweichende Artefaktsprachen definieren.

## Feature-Artefakte

Die Skills arbeiten mit folgenden Projektartefakten:

```text
features/
├── index.md
└── <FEATURE_ID>/
    ├── SPEC.md
    ├── decisions/
    │   └── <decision-id>.md
    ├── investigations/
    │   └── <investigation-id>.md
    ├── SYSTEM_DESIGN.md
    ├── TASKS.md
    ├── CODE_REVIEW.md
    └── QA_REPORT.md
```

`features/index.md` ist die kanonische Zustandsübersicht und enthält Statusdefinitionen sowie die erlaubte Übergangstabelle. Der Hauptpfad ist:

```text
ROADMAP → SPECIFIED → ARCHITECTED → TASKED
        → IN_BUILD → IN_REVIEW → APPROVED → DEPLOYED
```

`DEPLOYED` bezeichnet eine autorisiert bereitgestellte und durch eine Kurzprüfung bestätigte Revision und ist terminal. Rücksprünge sind nur über die in `features/index.md` dokumentierten Übergänge zulässig, etwa `IN_REVIEW` → `IN_BUILD` nach fehlgeschlagener QA oder `APPROVED` → `IN_BUILD`, sobald freigegebener Code verändert wurde.

Ein Statuswechsel muss Ausgangsstatus, Zielstatus, verantwortlichen Skill, Grund und Nachweis dokumentieren. Fehlt ein erlaubter Übergang, bleibt der Status unverändert. Phasen dürfen nicht übersprungen werden.

## Typischer Ablauf

1. `init-project` erstellt Produkt- und Projektgrundlagen.
2. `write-spec` definiert das beobachtbare Verhalten eines Features.
3. `solution-framing` klärt bei Bedarf jeweils eine folgenreiche technische Entscheidung und verlangt eine ausdrückliche menschliche Auswahl.
4. `system-design` entwirft die technische Umsetzung im erkannten Technologie-Stack und bindet genehmigte Entscheidungen ein.
5. `task-planner` erstellt die ausführbaren Aufgaben.
6. `build-feature` implementiert und verifiziert die Aufgaben; das Feature bleibt zunächst `IN_BUILD`.
7. `fact-based-code-review` prüft die konkrete Änderung. Nur `READY FOR QA` für den unveränderten Stand erlaubt `build-feature`, `IN_BUILD` → `IN_REVIEW` zu setzen.
8. `qa-agent` prüft Akzeptanzkriterien, Security und Revision. Bei einem Fehler mit unbekannter Ursache übergibt es zunächst an `failure-investigation`.
9. `failure-investigation` reproduziert und reduziert den Fehler, ohne Code oder Feature-Status zu verändern; nur `SUPPORTED CAUSE` geht als begrenzter Fix zurück an `build-feature`.
10. Nach der Korrektur und einer erneuten Codeprüfung prüft `qa-agent` die neue Revision erneut.
11. `deploy-feature` plant zunächst einen Probelauf (`DRY RUN`) und führt nur ausdrücklich autorisierte Schritte aus.

Die Skills sind bewusst getrennt. Eine spätere Phase darf fehlende Freigaben oder Nachweise einer früheren Phase nicht selbst erfinden. Der reguläre Prüfpfad lautet:

```text
build-feature → fact-based-code-review → qa-agent
```

`fact-based-code-review` verändert keinen Feature-Status und vergibt keine Feature-Freigabe. Der Übergang `IN_REVIEW` → `APPROVED` gehört ausschließlich `qa-agent`.

Die Fehlerkorrekturschleife lautet bei unbekannter Ursache:

```text
qa-agent → failure-investigation → build-feature → qa-agent
```

`failure-investigation` verändert dabei keinen Feature-Status.


## Validierung

Dieses Repository enthält derzeit keinen eigenen Installer und keine eigene automatisierte Eval-Suite. Vor Änderungen sollten mindestens geprüft werden:

- jedes Skill-Verzeichnis enthält genau eine `SKILL.md`;
- YAML-Frontmatter enthält einen zum Verzeichnis passenden `name` und eine konkrete `description`;
- lokale Markdown-Links zeigen auf vorhandene Dateien;
- die Skills werden nach der Installation in einer neuen Zed-Konversation erkannt;
- sicherheitskritische Skills werden mit kontrollierten Probeläufen evaluiert.

## Lizenz

Für dieses Repository und die eigenen Skills ist derzeit noch keine Projektlizenz festgelegt.