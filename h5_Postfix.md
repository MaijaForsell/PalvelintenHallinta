# Postfix-moduuli

### Pohjan luominen
Tarkoituksena oli luoda moduuli, jolla asennetaan ja konfiguroidaan Postfix Saltilla. Loin uuden kansion ja sinne vagrantfilen. Vagrantfile on lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta Debian 11 vaihdettu Debian 12. Halusin minimoida mahdollisuudet ongelmiin liittyen kaikkeen muuhun, kuin saltin käyttöön ja olin aiemmin käyttänyt tätä vagrantfileä. Vaihdoin hostnimet, jotta se eroaisi aiemmasta vagrantfilellä tehdystä koneesta. Nimet ovat kone1 (master) ja kone2 (minion).














## Ympäristö, jossa projektin tein:

Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6, jossa Microsoft Windows 11 -käyttöjärjestelmä

Työkaluina VirtualBox ja Vagrant

Käyttämäni Vagrantfile on otettu lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant"


## Lähteet

Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12" (https://reintech.io/blog/configure-postfix-mail-server-debian-12)

Postfix, "Virtual README" (https://www.postfix.org/VIRTUAL_README.html)

Postfix, "Postfix Basic Configuration" (https://www.postfix.org/BASIC_CONFIGURATION_README.html)

Linux Wiki, "/etc/hosts" (https://www.linux.fi/wiki//etc/hosts)

Wiki Debian, "Postfix" (https://wiki.debian.org/Postfix)
