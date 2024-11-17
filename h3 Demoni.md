# h3

## Tiivistelmä
### Karvinen 2018, "Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port" (https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh)

- Yleinen tapa ohjata demoneita on pkg-file-service
- Asenna ohjelma, korvaa konfigurointi-tiedosto ja uudelleenkäynnistä demoni
- Luo herra-koneella SSH-tila (sshd.sls) ja kopio konfiguraatio-tiedostosta (sshd_config)
- SSH-tila:

        $ cat /srv/salt/sshd.sls
  
        openssh-server:
  
         pkg.installed
  
        /etc/ssh/sshd_config:
  
         file.managed:
  
           - source: salt://sshd_config
  
        sshd:

         service.running:

           - watch:
  
             - file: /etc/ssh/sshd_config

  
- Tilan asentaminen:

        $ sudo salt '*' state.apply sshd
  
- Testaus nc-komennolla

        $ nc -vz minionin-nimi 8888

- Testaus ssh -p

        $ ssh -p 8888 minionin-nimi





## Tehtävät

Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6, jossa Microsoft Windows 11 -käyttöjärjestelmä

Prosessori on Intel64 Family 6 Model 154 Stepping 3 GenuineIntel (???)

Työkaluina VirtualBox ja Vagrant

### a) Apache easy mode

Käytin vagrantissa kahta virtuaalikonetta. Otettu lähteestä "Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta muokattu Bullseye:n sijaan Bookworm:iin. Toinen konfiguroitu herra-koneeksi ja toinen minion-koneeksi.

Apachen manuaaliseen asentamiseen käytin apuna Vultr, "How to Install Apache Webserver on Debian 12" (https://docs.vultr.com/how-to-install-apache-webserver-on-debian-12)

Otin etäyhteyden herra-koneeseen ja päivitin paketit.

                                $ vagrant ssh t001
                                $ sudo apt-get update
                                $ sudo apt-get upgrade

Seuraavana asensin Apachen. 

                                $ sudo apt install apache2 -y

Poistin Apachen oletuksella luotua index.html sivun ja korvasin sen uudella.

                                $ sudo rm /var/www/html/index.html
                        
                                $ sudoedit /var/www/html/index.html
                        
Kävin katsomassa, että se toimii, nettisivuilla "http://192.168.88.101" joka on siis herra-koneen ip-osoite.

![image](https://github.com/user-attachments/assets/e1eae434-f3ef-496a-875e-bed210aac23a)


![image](https://github.com/user-attachments/assets/335487df-dbbf-40f8-96dc-3c65790dd3bb)

Määritin, että koneen uudelleenkäynnistymisen jälkeen apache automaattisesti käynnistyy.

                                $ sudo systemctl enable apache2 

Käskin master-koneen käynnistyä uudelleen ja odottamisen jälkeen otin taas ssh-yhteyden siihen.

                                $ vagrant reload t001
                                $ vagrant ssh t001

Tarkistin onko apache päällä automaattisesti.
                        

                                $ sudo systemctl status apache2

                                ![image](https://github.com/user-attachments/assets/c908a468-db64-4880-9cc9-22262ab162dc)

Ja oli.

Poistin apachen, jotta voisin asentaa sen uudelleen.

                                $ sudo apt-get remove apache2

![image](https://github.com/user-attachments/assets/5f5d27a9-71aa-4cc9-ad97-b12963dd6050)


Loin uuden hakemiston apachelle ja siirryin sinne. Loin sinne init.sls-tiedoston. 
                                $ sudo mkdir /srv/salt/apache
                                
                                $ cd /srv/salt/apache

                                $ sudoedit /srv/salt/apache/init.sls

init.sls sisältö:

![image](https://github.com/user-attachments/assets/776cbee2-0bea-4d0f-98cf-3ab57cf6de85)

Loin myös html-tiedoston, jonka sisällön haluan index.html sisälle. En tiennyt miten tämä onnistuisi, joten kokeilin vain tehdä näin.

                                $ sudoedit /srv/salt/apache/nettisivu.html
                                

![image](https://github.com/user-attachments/assets/d9ad4d79-d7ba-4e98-a77d-0cd4d0d3c2bc)

Kohtalon hetki. Annoin komennon suorittaa tämä.

                                $ sudo salt '*' state.apply apache


![image](https://github.com/user-attachments/assets/b49b7281-a1a0-4bb5-94d2-86d908ff65ef)

Minion-kone ei löytänyt tiedostoani. Muokkasin siis sourcen tarkemmaksi, sillä tiedosto oli vain herra-koneella.

Päivitetty init.sls:

![image](https://github.com/user-attachments/assets/17e2179b-6129-4670-a438-4c7ccdcbbf74)

Uudestaan kokeillaan komentoa.

                                $ sudo salt '*' state.apply apache



![image](https://github.com/user-attachments/assets/12e9dabe-5c37-4a53-b3fe-920b6ed63aac)

Ainakaan masterin ip-osoitteella en saanut siihen yhteyttä, joten kokeilin minionin ip-osoitteella.

![image](https://github.com/user-attachments/assets/bef89926-8aa2-468a-aea2-7a23d9665925)

Toimii!


### b) SSHouto

En tiennyt tarkkaan mitä tehtävässä pyydettiin tekemään, joten tein sen vain yksinkertaisimmalla tavalla, miten sen voisin tehdä.

Ensin otin ssh-yhteyden herra-koneeseen. Etsin sieltä sshd_config-tiedoston. Tarkistin sen olevan olemassa ja muokkasin # pois "Port 22" edestä. Lisäsin sinne myös toisen portin.

                                $  sudoedit /etc/ssh/sshd_config

 Käytin grep-komentoa saadakseni kaikki, jotka eivät ole kommentteja.

                                $ grep -v '^#' /etc/ssh/sshd_config

Loin sshd.sls-tiedoston ja liitin sinne grep-komennosta saamani sisällön. Poistin turhat välit.

                                $ sudoedit /srv/salt/sshd/sshd_config

![image](https://github.com/user-attachments/assets/7217d538-f94c-4b5c-a194-eedfaa31b282)



![image](https://github.com/user-attachments/assets/6b0af4f6-f0d3-4414-ae58-224fc3160685)

Käynnistin demonin uudelleen.
                                $ sudo systemctl restart sshd


Annoin komennon, jotta koskisi myös minion-konetta.

![image](https://github.com/user-attachments/assets/d25d0f9f-d996-4d41-848f-5dc76d5045a2)


Jotakin meni vikaan

![image](https://github.com/user-attachments/assets/2628cfa9-f642-44de-8699-efd02c53bcc1)

Katsotaas uudestaan kaikki. 

Sourcen pitäisi olla:

/srv/salt/sshd/sshd_config

![image](https://github.com/user-attachments/assets/ede9f5db-c393-4289-893b-1ef89e4e644f)


Käytin grep-komentoa saadakseni kaikki, jotka eivät ole kommentteja.

                                $ grep -v '^#' /etc/ssh/sshd_config

//sshd_config


/etc/ssh/sshd_config


sshd:
 service.running:
   - watch:
     - file: /etc/ssh/sshd_config

## Lähteet
