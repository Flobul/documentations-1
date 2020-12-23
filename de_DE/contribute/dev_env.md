## Entwicklungsumgebung

Hier erfahren Sie, wie Sie eine effiziente Entwicklungsumgebung zwischen einem Test-Pi und einem Windows-PC einrichten, um den Code zu bearbeiten und das GitHub-Repository zu verwalten.

Diese Seite betrifft den Jeedom Core, aber diese Methode kann für die Plugin-Entwicklung verwendet werden.

Natürlich können wir für die schnelle Bearbeitung einiger Dateien das Plugin verwenden **JeeXplorer** direkt auf Jeedom. Aber es ist schnell langweilig und Sie müssen dann alle Änderungen an das lokale Repository oder direkt an GitHub melden. Dies ist nicht die praktischste.

### Principe

- Richten Sie einen Test-Pi mit Jeedom und einer Samba-Freigabe ein, um vom PC aus darauf zuzugreifen.
- Duplizieren Sie das Repository lokal mit **Erhabene Verschmelzung**.
- Implementieren **Erhabener Text** zum Bearbeiten von Code aus dem Repository mit Synchronisation auf dem Test-Pi.

**Erhabene Verschmelzung** und **Erhabener Text** werden sicherlich bezahlt (ein niedriger Preis mit 3 Jahren Update), sind aber sehr leicht, schnell, leicht anpassbar und sehr vollständig, ohne dass viele Plugins / Pakete erforderlich sind. Wenn Sie keine Lizenz erwerben, können Sie diese normal verwenden. Ab und zu wird nur ein kleines Popup mit einem Knopf angezeigt *Stornieren* !

Diese Methode ist auch mit anderen Werkzeugen möglich, wie z **Atom** (was einige Pakete erfordert) und **GitHub Desktop**.

### Pi Test / Entwicklung

Das erste, was Sie tun müssen, wenn Sie Kernfunktionen oder ein Plugin entwickeln : Richten Sie eine Testkonfiguration ein. In der Tat entwickeln wir keine Produktionskonfiguration !

Für die Installation von Jeedom ist die Dokumentation vorhanden : [Installation auf Raspberry Pi](https:/./.doc.jeedom.com/de_DE/installation/.rpi).

Achtung, ziehen Sie eine SSD einer SD-Karte vor !

Sobald Jeedom installiert ist, installieren Sie Samba in SSH :

`sudo apt-get install samba -y`

Konfigurieren Sie ein Passwort für www-Daten (die Wurzel von Jeedom) :

`sudo smbpasswd www-data` dann geben Sie Ihre *Kennwort*.

Bearbeiten Sie die Samba-Konfiguration :

`sudo nano / etc / samba / smb.conf`

Hinzufügen :

`` ''
gewinnt Unterstützung = ja

[jeedomRoot]
Pfad = / var / www / html
durchsuchbar = ja
beschreibbar = ja
Benutzer erzwingen = www-Daten
Kraftgruppe = www-Daten
schreibgeschützt = Nein
Gast ok = Ja
`` ''

Und starten Sie Samba neu:

`sudo / etc / init.d / smbd Neustart`

Geben Sie unter Windows in einem Datei-Explorer die IP-Adresse des Pi `\\ 192.168.xx` ein

Klicken Sie mit der rechten Maustaste auf "jeedomRoot" und dann auf "Netzlaufwerk verbinden ..."

Unter Windows haben Sie jetzt eine `jeedomRoot`-Netzwerkdiskette !


### Einrichten des lokalen Repositorys

Um das Repository lokal zu duplizieren und daran arbeiten zu können, werden wir es wiederherstellen [Sublime Merge tragbar](https:/./.www.sublimemerge.com/.download).

Auch erholen [Sublime Text tragbare 64bit](https:/./.www.sublimetext.com/.3).

Entpacken Sie die beiden Archive und platzieren Sie sie in `C:\ Programme`.

Geben Sie an **Erhabene Verschmelzung** Datei-Editor :

![Editeur](images/.sbm_settings1.jpg){:height="432px" width="867px"}

Klonen Sie dann das Repository. Wenn Sie Rechte am Core-Repository haben, klonen Sie es andernfalls *Gabel* es auf Ihrem GitHub-Konto und klonen Sie Ihre *Gabel*.

**Datei- / Klon-Repository ...**

![Clone Repository](images/.sbm_clonerepo.jpg){:height="364px" width="697px"}


### Einrichten der Edition

IN **Erhabener Text**, *Projekt* /. *Projekt bearbeiten*, Definieren Sie das Verzeichnis Ihres Repositorys :

`` ''json
{
  "folders":
  [
    {
      "name": "__GitHub Jeedom Core__",
      "path": "W:\\_ GitHub-Repos _ \\ JeedomCore"
    },
    {
      "name": "___Pi_JeedomAlpha___",
      "path": "\\\\ 192.168.0.110 \\ jeedomRoot"
    }
  ]
}
`` ''

Hier ist das Hinzufügen des Pfades des Test-Pi nicht obligatorisch, aber dennoch praktisch.

So können Sie jetzt in **Erhabener Text**, Bearbeiten Sie direkt die Dateien des lokalen Repositorys. Änderungen an diesen Dateien werden in angezeigt **Erhabene Verschmelzung**, Hier können Sie jede Datei ganz oder teilweise festschreiben oder die Änderungen rückgängig machen, wenn dies nicht funktioniert.

Jetzt müssen diese Codeänderungen noch auf dem Test Jeedom getestet werden.

Dazu können Sie natürlich die geänderten Dateien mit der Samba-Freigabe auf Ihrem PC auf Ihren Pi kopieren. Oder nicht ! Wenn Sie zehn Dateien an verschiedenen Stellen bearbeiten, wird dies schnell schmerzhaft !

Wir werden daher konfigurieren **Erhabener Text** Wenn Sie eine Datei speichern, wird sie direkt auf den Pi kopiert !

Gehen Sie in das Verzeichnis `C:\ Programme \ SublimeText3 \ Data \ Packages \ User` und erstellen Sie eine `onSaveCopy.py`-Datei. Bearbeiten Sie es und speichern Sie nach dem Ändern der richtigen Pfade den folgenden Code:

`` ''py
Importieren Sie Sublime, Sublime_plugin, Bone
von Shutil Import Copyfile

gitHub_repoCore = "W:\\_ GitHub-Repos _ \\ JeedomCore"
rpi_root = "\\\\ 192.168.0.110 \\ jeedomRoot"

Klasse EventListener (sublime_plugin.EventListener ):
  def on_post_save_async (self, view):
    fullPath = view.file_name()
    Pfad, baseName = os.path.split (fullPath)
    if gitHub_repoCore im Pfad:
      rpi_path = fullPath.ersetzen (gitHub_repoCore, rpi_root)
      copyfile (fullPath, rpi_path)
`` ''

Und los geht's !

Wann immer Sie eine Datei speichern, wenn sie Teil des lokalen Repositorys ist, **Erhabener Text** kopiert es auch an die richtige Stelle auf Ihrem Pi. Strg-S, F5 auf dem Pi und das wars ! Wenn alles in Ordnung ist, inszenieren / festschreiben / einschieben **Erhabene Verschmelzung**.

Wenn Sie Änderungen rückgängig machen, nehmen Sie a *Verwerfen* IN **Erhabene Verschmelzung**, Denken Sie daran, mit der rechten Maustaste zu klicken, *Im Editor öffnen*, und Strg-S, um es wieder auf den Pi zu setzen.

Und natürlich sollten Sie beim Aktualisieren des Pi vorsichtig sein, da Sie die von Ihnen geänderten Core-Dateien überschreiben.


Sie können natürlich dieselbe Methode anwenden, um Ihr Repository und die Synchronisierung auf Ihren Plugins einzurichten.