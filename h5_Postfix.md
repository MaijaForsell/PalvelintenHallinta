

Tarkoituksena oli luoda moduuli, jolla asennetaan ja alkukonfiguroidaan Postfix Saltilla.



Asensin ensin mailutils master-koneeseen. Sen jälkeen asensin Postfixin käsin.

                                $ sudo apt install postfix

Valitsin "internet" muodokseni. Sitten valitsin System mail name "testiposti.org"


                                
                              




### Jinja template

Sain idean luoda jinja template, kun hain tietoa miten voisin tehdä sellaisen main.cf-filen, jota voisin käyttää kaikissa minioneissa verkossa. Vaikka minulla ei juuri nyt ole useampaa, niin haluan kokeilla.






### Harha-askel

Päätin tässä vaiheessa käyttää vanhaa master-minion -paria joka minulla on, koska en saanut uutta konetta yhdistettyä. Sen pohjana oli sama vagrantfile, jota käytin uusiin koneisiin. Poistin koneilta postfixin.

        $ apt-get purge postfix

Lopputuloksena minulla oli kaksi konetta. Toinen oli master ja toinen minion. Kummassakaan ei ollut postfix asennettuna. 

Huomasin, että joitakin konfiguraatioita oli jäänyt, joten tehtävän toistettavuuden vuoksi päätin jättää tämän tänne dokumentin loppuun. Testatessani viestin lähettämistä master-koneessa, näin siellä vanhojen konfiguraatioiden jäänteitä.

![image](https://github.com/user-attachments/assets/77074d42-8761-4a56-9679-8456aa8ebd77)

Huomaa "maijanposti.org" osoitteena. Poistin nämä koneet kokonaan.


Eli loin lopulta uudet koneet. 
















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

Salt Project, "salt.states.file" (https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html)
