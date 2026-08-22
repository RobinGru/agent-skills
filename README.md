# Agent Skills

Dieses Repository enthält elf eigene, deutschsprachige Zed Agent Skills für einen dokumentierten Feature-Arbeitsablauf von der Projektinitialisierung bis zur Bereitstellung.

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
| [`product-interface-engineering`](skills/product-interface-engineering/SKILL.md) | Implementiert genau eine begrenzte sichtbare UI-Aufgabe mit Interaktionszuständen, Barrierefreiheit und responsiver Bedienung und vermeidet unbegründetes generisches KI-Trenddesign. |
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

## Installation in Codex

Codex verwendet dieselbe Skill-Verzeichnisstruktur. Kopiere jedes gewünschte vollständige Skill-Verzeichnis einschließlich seiner `SKILL.md` an einen dieser Orte:

- global für den aktuellen Benutzer: `~/.agents/skills/<skill-name>/SKILL.md`
- nur für ein Repository: `<projekt>/.agents/skills/<skill-name>/SKILL.md`

### Globale Installation unter macOS, Linux oder WSL

```sh
mkdir -p ~/.agents/skills
cp -R skills/write-spec ~/.agents/skills/write-spec
cp -R skills/system-design ~/.agents/skills/system-design
cp -R skills/build-feature ~/.agents/skills/build-feature
```

### Globale Installation unter Windows PowerShell

Unter Windows entspricht das globale Ziel normalerweise:

```text
C:\Users\<benutzername>\.agents\skills\
```

Gib bei `$SkillSource` den absoluten Pfad zum `skills`-Verzeichnis dieses Repositorys an:

```powershell
$SkillSource = "C:\Pfad\zu\agent-skills\skills"
$GlobalSkillDir = Join-Path $env:USERPROFILE ".agents\skills"

New-Item -ItemType Directory -Force $GlobalSkillDir
Copy-Item -Recurse (Join-Path $SkillSource "write-spec") (Join-Path $GlobalSkillDir "write-spec")
Copy-Item -Recurse (Join-Path $SkillSource "system-design") (Join-Path $GlobalSkillDir "system-design")
Copy-Item -Recurse (Join-Path $SkillSource "build-feature") (Join-Path $GlobalSkillDir "build-feature")
```

Danach liegt beispielsweise `write-spec` hier:

```text
C:\Users\<benutzername>\.agents\skills\write-spec\SKILL.md
```

### Projektbezogene Installation

Lege im Zielprojekt `.agents/skills/` an und kopiere die gewünschten Verzeichnisse dorthin:

```text
<projekt>/
└── .agents/
    └── skills/
        ├── write-spec/
        │   └── SKILL.md
        ├── system-design/
        │   └── SKILL.md
        └── build-feature/
            └── SKILL.md
```

Unter Windows PowerShell kannst du die Skills so in ein bestimmtes Projekt kopieren:

```powershell
$SkillSource = "C:\Pfad\zu\agent-skills\skills"
$ProjectDir = "C:\Pfad\zu\deinem-projekt"
$ProjectSkillDir = Join-Path $ProjectDir ".agents\skills"

New-Item -ItemType Directory -Force $ProjectSkillDir
Copy-Item -Recurse (Join-Path $SkillSource "write-spec") (Join-Path $ProjectSkillDir "write-spec")
Copy-Item -Recurse (Join-Path $SkillSource "system-design") (Join-Path $ProjectSkillDir "system-design")
Copy-Item -Recurse (Join-Path $SkillSource "build-feature") (Join-Path $ProjectSkillDir "build-feature")
```

Das projektbezogene Ergebnis liegt dann beispielsweise hier:

```text
C:\Pfad\zu\deinem-projekt\.agents\skills\write-spec\SKILL.md
```

Starte Codex anschließend in einer neuen Sitzung. Prüfe, ob die kopierten Skills im verfügbaren Skill-Katalog erscheinen. Wird ein Skill nicht erkannt, kontrolliere zuerst, ob Verzeichnisname, `name` im YAML-Frontmatter und Dateiname `SKILL.md` exakt übereinstimmen.

Kopiere nur benötigte Skills und überschreibe vorhandene Zielverzeichnisse nicht ungeprüft. Für ein Update sollte das bestehende Zielverzeichnis zuerst gesichert oder bewusst ersetzt werden.

`deploy-feature` hat `disable-model-invocation: true` und wird aus Sicherheitsgründen in unterstützenden Clients nur manuell aktiviert.

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
5. `task-planner` erstellt die ausführbaren Aufgaben und weist sichtbare UI-Aufgaben bei Bedarf `product-interface-engineering` zu.
6. `build-feature` implementiert und verifiziert die Aufgaben; das Feature bleibt zunächst `IN_BUILD`.
7. `product-interface-engineering` bearbeitet innerhalb von `IN_BUILD` genau eine zugewiesene UI-Aufgabe und gibt nur `ABGESCHLOSSEN` mit direktem Nachweis an `build-feature` zurück.
8. `fact-based-code-review` prüft die konkrete Änderung. Nur `READY FOR QA` für den unveränderten Stand erlaubt `build-feature`, `IN_BUILD` → `IN_REVIEW` zu setzen.
9. `qa-agent` prüft Akzeptanzkriterien, Sicherheit und Revision. Bei einem Fehler mit unbekannter Ursache übergibt es zunächst an `failure-investigation`.
10. `failure-investigation` reproduziert und reduziert den Fehler, ohne Code oder Feature-Status zu verändern; nur `SUPPORTED CAUSE` geht als begrenzte Korrektur zurück an `build-feature`.
11. Nach der Korrektur und einer erneuten Codeprüfung prüft `qa-agent` die neue Revision erneut.
12. `deploy-feature` plant zunächst einen Probelauf (`DRY RUN`) und führt nur ausdrücklich autorisierte Schritte aus.

Die Skills sind bewusst getrennt. Eine spätere Phase darf fehlende Freigaben oder Nachweise einer früheren Phase nicht selbst erfinden. Der optionale UI-Spezialistenpfad lautet:

```text
task-planner → build-feature → product-interface-engineering → build-feature
```

`product-interface-engineering` verändert keinen Feature-Status und ersetzt weder Codeprüfung noch Qualitätssicherung.

Der reguläre Prüfpfad lautet:

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