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

Eli tässä vaiheessa toimi paikallisesti, master-koneessa. Seuraava haaste oli saada yhteys toimimaan master-koneesta minion-koneeseen. Loin minion-koneeseen uuden käyttäjän, joka ei ole master-koneessa. Minun piti myös päivittää main.cf kummassakin koneessa, jotta ne voisivat olla yhteydessä. En tiedä miten se tehdään, mutta kokeilin ensin vain lisätä "sallittuihin" yhteyksiin kummankin koneen hostnamet.


![image](https://github.com/user-attachments/assets/83a86d32-9a4b-4a62-94a2-3003163b60fc)


Latasin minion-koneeseen mailutils, päivitin main.cf ja käynnistin postfixin uudelleen. Tein uuden käyttäjän "minionkone". 

![image](https://github.com/user-attachments/assets/859dcd9d-2601-47f0-b397-fa5e71fb9e59)

Kokeilin lähettää master-koneen sender-käyttäjällä postia minionkone-käyttäjälle, joka sijaitsee minionkoneessa. Se ei kuitenkaan löydä maildir-kansiota.

![image](https://github.com/user-attachments/assets/6cb01ad1-e4f4-48eb-8e07-e062e24cf705)

Kokeilin poistaa postfixin kokonaan minionkoneelta. Latasin sen uudelleen heti.

                                    $ sudo apt purge postfix

                                    
Lataamisen jälkeen konfiguroin ja uudelleenkäynnistin postfixin. Loin myös uuden käyttäjän. Silti en löytänyt maildir-kansiota. En ollut varma miksi. Kokeilin jos maildir-kansion olemassaoloon liittyy se, että postia on saatava. Lähetin siis paikallisesti postia minionkoneen käyttäjien välillä.

                                    $ echo "Test email body" | mail -s "Testi" minionreceiver@maijanposti.org

![image](https://github.com/user-attachments/assets/424d4800-88f1-4830-9269-c824f3d53ed1)

Ja se toimii. Eli ongelma on yhteydessä siihen, etten ole osannut yhdistää koneiden postfixejä. 

Tässä kohtaa tiedän sen, että postfix toimii paikallisesti kummassakin koneessa. Tiedän myös, että aloittaessani koodin kirjoittamisen ja saltin käyttämisen oikeasti, minun kannattaa tehdä main.cf-tiedostosta jinja-template, jotta minun ei tarvitse tehdä erillistä tiedostoa jokaiselle eri minionille. Voin siis todennäköisesti korvata tiedostossa hostnamen "{{ grains['host']". 

Seuraavaksi ratkaisua lähdin hakemaan sekä porttien, että verkkojen sallimisen kautta. Minun kohdalla ni Postfix käyttää SMTP, eli simple mail transfer protokollaa, eli avoimena on oltava portti 25. 

                                    $ nc -zv host_nimi 25

![image](https://github.com/user-attachments/assets/dfd08221-d4e2-4003-80cd-98027d115650)

![image](https://github.com/user-attachments/assets/eea70820-504d-4bef-a934-55268728b16f)

Portit ovat siis auki.

IP-osoitteiden salliminen olisi vuorossa. Koska main.cf-tiedostossa on mynetworks-kohta, oletin siellä olevan vastaus. Etsin netistä tietoa, mitä siinä kohdassa kuuluu olla. Postifix Basic Configuration () ohjeessa mainitaan kohta:

"mynetworks = 127.0.0.0/8 168.100.189.2/28 (authorize local networks)" 

Joten ajattelin, että voisin seuraavana kokeilla omien ip-osoitteiden sallimista. Eli ainakin tässä kohtaa minulla oli ongelma main.cf-tiedostossa. Nykyisessä tilanteessa minulla oli vain kaksi konetta, joiden IP-osoitteissa oli vain yhden numeron ero. Pyrin kuitenkin tekemään tästä tiedostosta sellaisen, että samaa voi käyttää useammalle minionille. Tässä kuvitteellisessa tilanteessa minulla olisi alle 100 konetta, joten riittäisi, että kaikkien koneiden IP-osoitteissa olisi vain viimeisen bitin kohdalla ero. Eli pitäisi riittää "mynetworks = 127.0.0.0/8 192.168.88.0/24" lisääminen.

Muutin kumpaankin koneeseen tiedostoa.

![image](https://github.com/user-attachments/assets/20424baa-bcbb-49fe-b0e6-02e58a8bfbc2) 

Käynnistin koneissa Postfixin uudelleen. Sitten kokeilin lähettää minionille postia master-koneelta.

![image](https://github.com/user-attachments/assets/5b02c3af-ca06-4224-bb54-51db2a90f74c)

Se ei toiminut. Oli aika tutkia lisää. 







                                    



        
         








## Lähteet:

Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Reintech, 2024 "Configuring Mail Server with Postfix on Debian 12" (https://reintech.io/blog/configure-postfix-mail-server-debian-12)


