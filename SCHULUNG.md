# Einführung Jitsi Meet 


## Vorbereitungen:

* Github Account anlegen 
* SSH Key erstellen
* SSH Key bei Github hinterlegen
* Hetzner Cloud Account anlegen  https://accounts.hetzner.com/signUp
* Sich zu Github, Hetzner einladen lassen (dazu Username/Mailaddresse mit der registriert wurde mitteilen) und SSH Key auf den Rechnern hinterlegen lassen
* Python3 installieren
* Github Einladung bestätigen (wenn Einladung passiert ist)
* Entwicklungsumgebung installieren (z.B. https://code.visualstudio.com/ oder https://www.vim.org/)
  ggf. Module für
  * YAML, 
  * Ansible
  * Syntaxhighlight
  * Trailing Spaces remove
  * Tab to Spaces
  * Git

```
# ggf in ein Verzeichnis wechseln wo das Repo später liegen soll, bei mir /home/sven/src
pip install ansible ansible-lint
git clone git@github.com:ADFC-Hamburg/adfc-ansible.git

cd adfc-ansible
ansible-galaxy install -r requirements.yml
```


## Einführung


Überblick über die Maschienen für Jitsi

* proxmox01.adfc-intern.de (Proxmox / Stats)
* int-master.adfc-intern.de
* meet.adfc-intern.de
* vbdh01.meet.adfc-intern.de bis vbdh04.meet.adfc-intern.de

## Struktur Ansible

* Inventory / Hostvars / Groupvars
`ansible-inventory -y --host proxmox01.adfc-intern.de`
* Playboooks
* Rollen

## Nützliche Playbooks

* setup-ssh.yml
* setup-meet-vb-for-test.yml
* setup-meet-vb.yml
* delete-meet-vb.yml
* update-os.yml

## Neue Test Maschine anlegen:

* In die inventory.yml aufnehmen
* Hostvars setzen
* Playbook aufrufen
* Rechner erreichen

## Pull Requests für Anfänger

```
cd src/adfc-ansible
git checkout master
git pull origin master
git checkout -b mein-neues-feature
# Änderungen machen
# Testen ob Syntaxverbesserungen vorliegen
ansible-lint --offline
git add pfad/zu/neuer-datei.txt
git rm pfad/zu/datei-die-weg-soll.txt
git commit -m 'Sinnvoller Kommentar' pfad/zu/neuer-datei.txt pfad/zu/datei-die-weg-soll.txt pfad/zu/geaenderter-datei.txt
# Sicherstellen das alles eingescheckt ist
git status
git diff
# Änderungen zu github spielen
git push origin mein-neues-feature
# Auf den Angezeigten Link klicken und Pull-Request machen
# Pull-Request Diskutieren
# ggf. Änderungen machen
# Wenn alles okay in Gitub in den Master Mergen
git checkout master
git pull origin master
# Alten Branch löschen
git branch -d mein-neues-feature
```

## Dynamic Jitsi Scaler

### Proxmox01
Ist ein Skript wass regelmäßig ausgeführt wird
/usr/local/bin/dynamic-jitsi-scaler

### Logfile
journalctl -u djs.service  --since -1h

```
# ggf. mal anhalten:
systemctl stop djs.timer 
# Auch bei systemstart
systemctl disble djs.timer 

# timer starten
systemctl start djs.timer 

systemctl enable djs.timer 

# Das Skript einmalig starten:
systemctl start djs.timer

# Wie ist der Service / Timer genau definiert
systemctl cat djs.service
systemctl cat djs.timer
# Status
systemctl status djs.timer
```


## Logfiles und Config Files

* /etc/jitsi
* /etc/nginx/sites-available/meet.adfc-intern.de.conf
* /etc/prosody/
* /var/log/jitsi/
* /var/log/prosody/

https://stats.adfc-intern.de

