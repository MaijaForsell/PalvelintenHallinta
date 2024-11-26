# Oma moduli, sähköposti

Lähdin tekemään sähköpostipalvelua, jossa postfix. Dovecot on ilmeisesti myös vastaanottamiseen tarpeellinen, joten pyrin tekemään myös sen, jotta tämä toimisi.


## Ympäristö, jossa projektin teen:

Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6, jossa Microsoft Windows 11 -käyttöjärjestelmä

Työkaluina VirtualBox ja Vagrant

Aloitin lataamalla kahden koneen vagrantfilen ja yhdistämällä ne masteriksi ja minioniksi. Vagrantfile lähteestä Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta Debian 11 vaihdettu Debian 12.


## Postfix käsin

Käytin apunani lähdettä Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12".

Ensin asensin postfixin

         $ sudo apt install postfix

Kopioin itselleni talteen main.cf-tiedoston myöhempää käyttöä varten. Nyt ensimmäisellä asennuskerralla muokkaisin tiedostoa suoraan, mutta suunnittelin, että myöhemmässä vaiheessa käyttäisin kopioimaani main.cf-tiedostoa korvaamaan aiempi tiedosto kokonaan käyttämällä "pkg-file-service" -metodia.


Sain kuitenkin mystisen graafisen käyttöliittymän eteeni? Valitsin pelkän internetin postin käytön. Annoin nimeksi "maijanposti.org". 

![image](https://github.com/user-attachments/assets/ccb1c2cb-e8b1-40f4-8d45-d7a1bcbc109f)

Sain Postfixin asennettua masterille, se on myös päällä.

![image](https://github.com/user-attachments/assets/4ec10296-d062-4bd0-80aa-722ebff548bd)


Tein saman myös minionille, eli asensin postfixin.

![image](https://github.com/user-attachments/assets/8776e725-3b94-406f-8de3-394df2f2d303)



Olen hiukan pihalla mitä kukin asetus tarkoittaa, joten annoin käskyn "sudo nano" saadakseni eteeni tekstiä.

        $ sudo nano /etc/postfix/main.cf


Tässä tämän hetken postfixin main.cf -tiedoston oleelliset osat. Tällä pitäisi pystyä lähettämään sähköpostia koneen sisällä. Snakeoil sertifikaattien käytön ei pitäisi myöskään olla ongelma.


![image](https://github.com/user-attachments/assets/051bc029-4dbb-4ec5-9acc-141997e56c6d)





        
         








## Lähteet:

Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12" (https://reintech.io/blog/configure-postfix-mail-server-debian-12)


