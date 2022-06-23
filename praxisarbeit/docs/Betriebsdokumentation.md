---
title: Betriebsdokumentation
author: Conese Dillan, van Loo Colin
date: 2022-06-17
lastmod: 2022-06-17
---

# Betriebsdokumentation

Erstellt am 17.06.2022.

## Einführungstext 

Das User-Skript (buc) ist ein hilfreiches Tool zum erstellen vieler Nutzer auf
einmal, mit Features wie Skeleton-Strukturen und mehr.

Das Backup-Skript (bub) erstellt zuverlässige Sicherungen der Home-directories.

Beide Skripts bieten vielfältige Konfigurations-Optionen, mittels
Konfigurationsdateien oder auch direkt über die Kommandozeile.

## Installationsanleitung für Administratoren

### Installation

Unter [Releases](https://github.com/TBZedu/m122_praxisarbeit/releases) die
neueste Version herunterladen. Entweder als ZIP oder als TAR.

```sh
curl -L https://github.com/TBZedu/m122_praxisarbeit/archive/refs/tags/v0.0.1-alpha.tar.gz > bucb.tar.gz
tar -xvf bucb.tar.gz
cd m122_praxisarbeit-0.0.1-alpha
```

### Konfiguration

Eine Beispielkonfiguration mit Beschreibung findet sich unter `etc/bucb.env`.
([Repo Link](../etc/bucb.env))

Der Inhalt wird als Bash-Skript interpretiert, die Konfigurations-Syntax muss
also valide Bash-Syntax sein.

Dies ermöglicht Konfigurations-Optionen wie...

```bash
# Set the default password.
DEFAULT_PASSWORD="$(pwgen -s1 30)"
```

...um zufällige Standard-Passwörter zu generieren.

#### Beispiel Conjob Erstellen

```sh
crontab -e
```

Folgenden Inhalt einfügen:

```
0 0 * * * /opt/bucb/bub
```

Dies führt den Befehl `/opt/bucb/bub` zur nullten Minute, nullten Stunde jeden
Tag (also alle 24h um Mitternacht) aus.

#### User-Home-Templates Einrichtung

Um Gruppen spezifische User-Home-Templates zu erstellen muss man ein Verzeichnis mit dem Namen der Gruppe in `etc/skeleton` erstellen. Darin kann man dann die Template Konfiguration für alle Benutzer der Gruppe vornehmen. Beispiel für eine Gruppe namens "verkauf": `etc/skeleton/verkauf`

## Bedienungsanleitung Benutzer

### User Creation (buc)

#### Erstellen der Input Dateien
Das Input-File muss folgendem Format entsprechen:
```
<username> <group> <vorname> <nachname>
```

#### Aufruf des Skriptes

Das Skript kann über...
```sh
buc <input-file> <flags>
```
... ausgeführt werden.

Beispiel:
```sh
buc users -p siHh_r9spA2sw^r123
```
Dies würde die Benutzer mit dem Passwort "siHh_r9spA2sw^r123" erstellen. Dieses muss direkt nach der Anmeldung geändert werden.

Bitte beachten, dass hier die [User-Home-Templates Einrichtung](####User-Home-Templates-Einrichtung) vollendet sein muss.

Weitere Informationen können in der man page (`man -l man/buc.1`) gefunden werden. ([man](../man/))


#### Logfile & Bekannte Fehlermeldungen

Das Logfile befindet sich unter `var/buc.log`.

Fehlermeldungen:

- insufficient permissions, script must be run as root

### Backup (bub)

#### Erstellen der Input Dateien

Alle zu sichernden Gruppen, eine pro Zeile, in eine Datei schreiben
([Beispiel](../etc/bub.conf)). Sicherstellen, dass die Konfigurations-Variable
`BACKUP_GROUPS_FILE` gesetzt ist und auf die Datei zeigt.

Alternativ die Gruppennamen über die Kommandozeile übergeben:

```sh
# All valid syntax
bub -g root --group wheel
bub -g=root,wheel
bub --group=wheel,root,test
```

Nutzer haben die Möglichkeit einige ihrer Dateien aus dem Backup
auszuschliessen. Dafür kann eine Datei "bub.ignore" Muster (Regex) enthalten.
Sollte eine Datei aus dem selben oder tieferen Directories mit einem dieser
Muster übereinstimmen, wird es vom Backup ignoriert.

```
# /home/müller/bub.ignore
*.bash*
```

Mit dem obigen Beispiel würde die Datei `~müller/.bashrc` ignoriert werden.
Nicht aber zum Beispiel die Datei `~peter/.bashrc`, da diese sich nicht in
einem Subdirectory zum `bub.ignore` befindet.

```
/home
  /müller
    bub.ignore # Spezifiziert `*bash*`
    .bashrc    # Ignoriert
    /test
      bash_test # Ignoriert
  /peter
    .bashrc    # Nicht Ignoriert
```

#### Aufruf des Skriptes

Für eine Hilfe zu den erlaubten Kommandozeilen-Argumente `bub --help` ausführen.

Für die aktuelle Version `bub --version`. (Achtung, keine Kurzform. `bub -v`
bedeutet "verbose", dasselbe wie `bub -v=D`)

Werden die Hilfe-, oder Versionsparameter übergeben, werden alle anderen
Parameter ignoriert, Ausführung wird nach ausgeben der Hilfe oder Version
direkt beendet.

Flaggen haben normalerweise (mit Ausnahme von `--version`) eine kurze (`-g`)
und eine lange (`--group`) Form.

Flaggen die ein Argument erwarten, können dies entweder mit einem Space oder
einem Gleichzeichen erhalten.

```sh
bub -g root
bub -g=root
```

Weitere Informationen können in der man page (`man -l man/bub.1`) gefunden
werden. ([man](../man/))

#### Erzeugte Dateien

Während der Ausführung wird eine temporäre Datei unter `/tmp/` erstellt. Diese
wird lediglich als Input zu `tar` gebraucht.

Logfiles werden unter `var/bub.log` [Beispiel](../var/bub.log) gespeichert.
