# Oma moduli, sähköposti

Lähdin tekemään sähköpostipalvelua, jossa postfix. Dovecot on ilmeisesti myös vastaanottamiseen tarpeellinen, joten pyrin tekemään myös sen, jotta tämä toimisi.


## Ympäristö, jossa projektin teen:

Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6, jossa Microsoft Windows 11 -käyttöjärjestelmä

Työkaluina VirtualBox ja Vagrant


## Postfix käsin

Aloitin lataamalla kahden koneen vagrantfilen ja yhdistämällä ne masteriksi ja minioniksi. Vagrantfile lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta Debian 11 vaihdettu Debian 12.

