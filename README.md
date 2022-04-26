# PushToRemoteRepository

Creation of back-up of a Power Automate Desktop flow and pushing it to a remote repository (Git)

Pre-requisites:
- a folder must be created named exactly like the PAD flow to be backed-up
- an environment variable with the name 'PADRobotsGeneralConfigPath' needs to be created; the value must be the folder where all the repositories of the PAD flows should stored
- since Git commands are used in the Powershell, the Powershell plugin posh-git must be installed --> https://www.pugetsystems.com/labs/hpc/Note-Setup-Git-for-PowerShell-on-Windows-10-1653/


noch zu tun:
- Issues abfragen und anzeigen
- erste Zeile und letzte Zeile (FUNCTION) entfernen ✔
- gelöschte Subflows: Textfile auch löschen ✔
- JSON getrennt für Input und Output ✔
- Commit-Kommentar ✔
- Versionpflege ✔
- Anlegen einer Datei, die die Beschreibung der einzelnen Versionen enthält (Versionsnummer und Commit-Kommentar) ✔
- über die Git-Bash einen Commit und einen Sync mit dem Remote-Repository anstoßen ✔
- Kommentar in Software (ganz oben in Main) mit der Versionsnummer)
