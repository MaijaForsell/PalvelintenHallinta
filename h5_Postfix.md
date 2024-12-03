# Postfix-moduuli

Tarkoituksena oli luoda moduuli, jolla asennetaan ja alkukonfiguroidaan Postfix Saltilla.


### Postfix asennus
Asensin ensin mailutils master-koneeseen. Sen jälkeen asensin Postfixin käsin.

                                $ sudo apt install postfix

Valitsin "internet" muodokseni. Sitten valitsin System mail name "testiposti.org"

![image](https://github.com/user-attachments/assets/4a51e24e-32e3-41c0-9578-cf16f7db995a)


Kopioin main.cf-tiedoston ja loin uuden kansion ja sinne uuden tiedoston, jota käyttää mallina.

                                $ sudo mkdir -p /srv/salt/postfix_main/
                                $ cd /srv/salt/postfix_main/
                                $ sudoedit mainfile

Tässä mainfile sisältö:

![image](https://github.com/user-attachments/assets/66ec2153-26a4-480d-88be-8958173a604f)



Kopioin mainfile-tiedoston sisällön master-koneen main.cf-fileen.

                                $ sudoedit /etc/postfix/main.cf

Mitä olen muuttanut?

Poistin kommentoidut osat.

mynetworks: Lisäsin käyttämäni IP-verkon, jossa useamman koneen voisi olla mahdollista kommunikoida, jos haluan työstää mahdollisuutta koneiden väliseen kommunikointiin.

myorigin: Korvasin siihen $mydomain, samasta syystä kuin mynetworks.







### Pohjan luominen
Loin uuden kansion ja sinne vagrantfilen. Vagrantfile on lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta Debian 11 vaihdettu Debian 12. Halusin minimoida mahdollisuudet ongelmiin liittyen kaikkeen muuhun, kuin saltin käyttöön ja olin aiemmin käyttänyt tätä vagrantfileä. Vaihdoin hostnimet, jotta se eroaisi aiemmasta vagrantfilellä tehdystä koneesta. Nimet ovat kone1 (master) ja kone2 (minion).

Asensin kone1 salt-masterin ja kone2 salt-minionin. Lisäsin kone2 salt-minion tiedostoon masteriksi kone1 IP:n, mutta se ei yhdistä. Edes käynnistämällä uudelleen sekä masterin, että minionin demonit, en saanut yhdistettyä. 

Päätin tässä vaiheessa käyttää vanhaa master-minion -paria joka minulla on. Sen pohjana on sama vagrantfile. Poistin koneilta postfixin.

        $ apt-get purge postfix

Lopputuloksena minulla on kaksi konetta. Toinen on master ja toinen on minion. Kummassakaan ei ole postfix asennettuna.














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
