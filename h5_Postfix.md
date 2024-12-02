# Postfix-moduuli

### Pohjan luominen
Tarkoituksena oli luoda moduuli, jolla asennetaan ja konfiguroidaan Postfix Saltilla. Loin uuden kansion ja sinne vagrantfilen. Vagrantfile on lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta Debian 11 vaihdettu Debian 12. Lisäsin myös kolmannen koneen. Halusin minimoida mahdollisuudet ongelmiin liittyen kaikkeen muuhun, kuin saltin käyttöön ja olin aiemmin käyttänyt tätä vagrantfileä.

Asensin salt-masterin ja salt-minionin käsin koneisiin.

Hyväksyin minionien avaimet master-tietokoneella ja testasin yhteydet.










## Ympäristö, jossa projektin tein:

Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6, jossa Microsoft Windows 11 -käyttöjärjestelmä

Työkaluina VirtualBox ja Vagrant

Käyttämäni Vagrantfile on otettu lähteestä Tero Karvinen, 2023, "Salt Vagrant - automatically provision one master and two slaves" (https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

## Lähteet

Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12" (https://reintech.io/blog/configure-postfix-mail-server-debian-12)

Postfix, "Virtual README" (https://www.postfix.org/VIRTUAL_README.html)

Postfix, "Postfix Basic Configuration" (https://www.postfix.org/BASIC_CONFIGURATION_README.html)

Linux Wiki, "/etc/hosts" (https://www.linux.fi/wiki//etc/hosts)

Wiki Debian, "Postfix" (https://wiki.debian.org/Postfix)
