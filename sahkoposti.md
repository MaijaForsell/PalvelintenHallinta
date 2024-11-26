# Oma moduli, sähköposti

Lähdin tekemään sähköpostipalvelua, jossa postfix. (Ehkä dovecot? en tiedä)

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


Tässä tämän hetken postfixin main.cf -tiedoston oleelliset osat. Tällä pitäisi pystyä lähettämään sähköpostia koneen sisällä. Snakeoil sertifikaattien käytön ei pitäisi myöskään olla ongelma. Lisäsin sinne Maildir-rivin, jotta postia voisi saada.


![image](https://github.com/user-attachments/assets/051bc029-4dbb-4ec5-9acc-141997e56c6d)


Kysyin josko olisin tehnyt jotakin väärin. En saanut vastausta, joten en varmaan.

                                    $ sudo postfix check


Tuli aika luoda käyttäjät. Sain komennot samasta lähteestä. Loin kaksi käyttäjää, "sender" ja "receiver"

                                    $ sudo useradd -m -s /bin/bash *tähän käyttäjänimi*
                                    $ sudo passwd *tähän käyttäjänimi*

Luomisen jälkeen oli vuorossa postfixin päällä pysymisen varmistaminen.

                                    $ sudo systemctl enable postfix

![image](https://github.com/user-attachments/assets/ac0b105c-a6df-4bb6-89aa-cfca68041e6d)

Halusin kokeilla postin lähettämistä.

                                    $ echo "Test email body" | mail -s "Test Email Subject" receiver@maijanposti.org

Sain kuitenkin tiedon ettei "mail"-komentoa ole olemassa. 

-bash: mail: command not found

Googlaamalla sain vastauksen, että pitää asentaa mailutils

                                    $ sudo apt install mailutils
                                    
Kokeilin uudestaan ja tällä kertaa komento saattoi mennä läpi. Kokeilin kirjautua receiver-käyttäjälle ja etsiä postin.

                                    $ su - receiver
                                    $ cd ~/Maildir
                                    $ ls ~/Maildir/new
                                    $ cat ~/Maildir/new/1732644234.V801I600017M120555.t001


![image](https://github.com/user-attachments/assets/8fad3591-7ae6-4fcc-b1ee-41171ba2f8d7)

Löytyy!





                                    



        
         








## Lähteet:

Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12" (https://reintech.io/blog/configure-postfix-mail-server-debian-12)


