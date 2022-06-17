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

Paste the following content:

```
0 0 * * * /opt/bucb/bub
```

Dies führt den Befehl `/opt/bucb/bub` zur nullten Minute, nullten Stunde jeden
Tag (alle 24h) aus.

TODO: Wie sind User-Home-Templates einzurichten

....

## Bedienungsanleitung Benutzer

### User Creation (buc)

TODO: Erzeugen der Input-Files beschreiben, falls noetig

TODO: beschreiben des Scriptaufruf

TODO: beschreiben der erzeugt files (falls solche erzeugt werden)

TODO: Lokation von logfiles und bekannte Fehlermeldungen beschreiben.

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

#### Erzeugte Dateien

Während der Ausführung wird eine temporäre Datei unter `/tmp/` erstellt. Diese
wird lediglich als Input zu `tar` gebraucht.

Logfiles werden unter `var/bub.log` gespeichert.
