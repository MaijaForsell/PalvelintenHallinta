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

En tiennyt tarkkaan mitä tehtävässä pyydettiin tekemään, joten tein sen vain jotenkuten mukaillen lähdettä Karvinen, "Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port".

Ensin otin ssh-yhteyden herra-koneeseen. Etsin sieltä sshd_config-tiedoston. Tarkistin sen olevan olemassa ja muokkasin # pois "Port 22" edestä. Lisäsin sinne myös toisen portin.

                                $  sudoedit /etc/ssh/sshd_config

![image](https://github.com/user-attachments/assets/3ed89626-c2b6-4745-86fc-48e2e94a53ce)


 Käytin grep-komentoa saadakseni kaikki, jotka eivät ole kommentteja.

                                $ grep -v '^#' /etc/ssh/sshd_config

Loin sshd_config-tiedoston ja liitin sinne grep-komennosta saamani sisällön. Poistin turhat välit.

                                $ sudoedit /srv/salt/sshd/sshd_config

![image](https://github.com/user-attachments/assets/0648a3e0-b9eb-4f53-8045-9927398d22f6)

Loin sshd.sls-tiedoston ja laitoin sisällöksi hiukan muokatun version sls-tiedoston sisällöstä, joka löytyy lähteestä Karvinen, "Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port".

                                $ sudoedit /srv/salt/sshd.sls

![image](https://github.com/user-attachments/assets/cba475b8-eafc-4aba-a522-71ce5891c839)

Kokeillaan...

                                $ sudo salt '*' state.apply sshd

Ei toiminut. Salt-masteriin ei yhteyttä.

![image](https://github.com/user-attachments/assets/69318aae-6578-4194-8aba-87c34678f444)

Kysyin salt-masterin tilannetta.
                                $ sudo systemctl status salt-master


Ja nyt salt-master on kuollut? OOM-kill?

Googlasin "oom kill" ja sain vastauksen, että se on "out of memory". Kone automaattisesti tappaa prosessin, joka vie tilaa muistista. Kokeilen vain käynnistää salt-masterin uudestaan. Vastaus saatu Neo4j, "Linux Out of Memory killer"(https://neo4j.com/developer/kb/linux-out-of-memory-killer/)

                                $ sudo systemctl restart salt-master

Lähti käyntiin ongelmitta. Kokeillaan vielä kerran.

                                $ sudo salt '*' state.apply sshd
                                

  ![image](https://github.com/user-attachments/assets/e4170430-0890-495d-be9d-6b5a2b8f3b6b)

Toimii, ainakin tähän asti.

Kokeilen ottaa yhteyttä minion-koneeseen.

                                $ nc -vz apuri_jr 8888
                                
Ei toiminut.

                                $ nc -vz t002 8888
                                
Ei toiminut.

"t002: forward host lookup failed: Unknown host". Kuitenkin minionkoneen hostname on t002. Eli onko ongelma kuitenkin minun tiedostoissa?

Uudelleenkäynnistin sekä minionin, että herran. Ei muuttanut mitään.


![image](https://github.com/user-attachments/assets/32fb0b4b-4b84-4bc8-b19e-129dc7fb9823)

Localhost toimii kyllä.

                                $ nc -vz localhost 8888

![image](https://github.com/user-attachments/assets/3b4e912c-4be6-4a4e-b1df-e98b0b8f17ab)

Kokeilin ip-osoitteella. Jotakin tapahtui. Host on "unknown", mutta laittaessa portiksi 8888 se on open, mutta laittaessa 1234, se ei mene läpi.

                                $ nc -vz 192.168.88.102 8888

![image](https://github.com/user-attachments/assets/53acabae-03d2-4d54-8047-e9375f0406b0)





### c) Oma moduli.

En tiedä mitä voisin tehdä moduliksi. En ole luova tai tekninen ihminen. Mielenkiinnonkohteeni eivät varsinaisesti koske tätä. Yksi lempiasioista päivässä on aamulla kuunnella uutiset kahvin kera, joten kenties siihen liittyen? Lähin asia mitä keksin, voisi jokin uutisiin liittyvä "Päivän uutiset", joka menisi minioneille joko tiettyyn aikaan tai joka näkysi sisäänkirjautuessa. En tiedä kuinka realistinen tavoite tämä on ja miten laillista se olisi "varastaa" Ylen sivuilta pääuutiset. En oikeasti edes tiedä mitä voisin tehdä. 

Olen miettinyt myös voimanostoharrastukseni kannalta, jos sieltä löytyisi jotakin mielenkiintoista tai tekemisen arvoista. En vain tiedä miten se lopulta liittyisi tähän kurssiin ja siihen ideaan, että se olisi jaossa useille tietokoneille. Teoriassa jokin laskuri voisi olla mahdollinen, mutta sen jakamisesta en osaa sanoa yhtään mitään. Se olisi käytännössä vain erillinen "sovellus", joka menee muille koneille. 


### d) VirtualHost

Aloitin 13.29 lopetin 14.56 (18.11)
7.00-
Pystytin kaksi konetta, joista toinen on master ja toinen on minion. Testasin yhteydet ja se oli ok.

Ensimmäinen asia, jonka tein oli /srv/salt/apache - hakemiston luominen.

                                $ sudo mkdir /srv/salt/apache

Sitten loin ja editoin init.sls-tiedoston.
                                $ sudoedit /srv/salt/apache/init.sls


Tässä sen sisältö:

![image](https://github.com/user-attachments/assets/e26d6afa-c298-4a92-a500-872c6f77e683)


Loin samalla filellä käyttäjän, jonka nimi on maijaleena. Loin käyttäjälle kotihakemiston, jonne voin sitten myöhemmin sijoittaa html-tiedoston. Tiedostoa pitäisi nyt pystyä muuttamaan. Tiesin, että joudun muuttamaan lupia apachen konfiguraatio-tiedostossa, mutta kokeilin ensin näin, jotta minulle edes tulisi konfiguraatio-tiedosto. En ollut varma josko se toimisi, mutta kokeilun arvoista.

Annoin komennon.

                                $ sudo salt '*' state.apply apache

![image](https://github.com/user-attachments/assets/74ffe7b4-bcf6-4f97-a37c-f7f6785823c7)

Jotakin oli vialla. Googlauksen mukaan synteksivirhe. Kokeilin ensin tehdä asennuksen ja vasta sitten muokata kaiken muun.

Tässä init.sls apachelle:

![image](https://github.com/user-attachments/assets/52fdc0fd-cbef-4a4f-88e8-c38be741d451)


Asentaminen onnistui.

![image](https://github.com/user-attachments/assets/29ee8b82-e857-4f51-b806-98965e28b1f4)



Nyt luon käyttäjän ja käyttäjälle kotihakemiston.

                                $ sudo mkdir /srv/salt/user
                                $ sudoedit /srv/salt/user/init.sls

![image](https://github.com/user-attachments/assets/bb7376af-bed0-4e68-b6e0-85f50b8a1f1b)

                                $ sudo salt '*' state.apply user

![image](https://github.com/user-attachments/assets/7bc0c9e0-2092-4755-90f0-c92f1d04061d)

Ei toiminut. Ilmeisesti väärä syntaksi ja tämä on nimenomaan se ongelma joka aikaisemmassa yrityksessä prakasi. Tiedän YAML vaativan joko listan, sanakirjan tai boolean. Mutta miksi tämä on viallinen? Poistin välilyönnin, joka oli ennen "user.present" ja poistin seuraavilta riveiltä yhden välilyönnin myös. 

Kokeilin uudestaan. Ei toiminut. Seuraavana vertasin aikaisempiin tekemiini sls- tiedostoihin muissa tehtävissä ja tajusin miksi siellä oli ollut välilyönti. Minun pitää "nimetä" mitä teen. Muokkasin uudestaan

![image](https://github.com/user-attachments/assets/5a83e1c8-a603-48ea-bcad-11244208c63f)

Ja se toimi! 

![image](https://github.com/user-attachments/assets/0738eac7-aeb1-4a35-8fe4-04d3354c301c)                           

Nyt voin työstää apachen konfiguraatiota. Tässä käytin Geeks for Geeks "How to Change the Root Directory of an Apache server?" apuna. Ennen configurointi-filen muuttamista on kuitenkin löydettävä se. Yritin etsiä vastausta googlesta ja nettisivuilta.

Tässä kohdassa en enää ymmärtänyt mikä on vialla, kun en löytänyt etc/apache2-kansiota. Kuitenkin apachen pitäisi olla minulla?

![image](https://github.com/user-attachments/assets/18c2adcb-a937-4d4a-a07c-85d987656b5e)

Mutta sitä ei löydy?

Hetken kipuilun jälkeen tajusin että ehkä apache on vain minion-koneessa. Eli kokeilen asentaa apachen masterille käsin. 

                                $ sudo apt install apache2
                                $ sudo systemctl enable apache2


Nyt voin koittaa etsiä apache2 config-tiedostoa. Se löytyi /etc/apache2-kansiosta, kuten lähteessä, Geek for geeks, sanottiin.

![image](https://github.com/user-attachments/assets/d4ffbc82-1dd5-4d7c-aebb-7e703ae1d323)

Muokkasin tämän konfiguroinnin. En rehellisesti osannut sanoa mitä teen tai miksi, miten, toimiiko se. 

Päivitin apache/init.sls

![image](https://github.com/user-attachments/assets/40ab93fd-44bd-44d3-a3d8-9f2bc7a5039d)

Taas kerran, kokeilemalla selviää. Kokeilemalla sain selville että salt-master on taas mennyt kuolemaan. Käynnistin uudelleen.

![image](https://github.com/user-attachments/assets/cd6c64ce-1305-4f10-91dc-556915be0d58)

                                $ sudo systemctl restart salt-master
                                $ sudo salt '*' state.apply user

Sain errorin:

"The function "state.apply" is running as PID 4528 and was started at 2024, Nov 19 10:08:50.959193 with jid 20241119100850959193"

En saanut netistä selkeää vastausta mistä on kyse, joten kenties en saisi muokata olemassa olevaa sls-tiedostoa? Kokeilin kopioida päivitetyn sisällön ja kirjasin sen uuteen kansioon ja init.sls-tiedostoon.

                                $ sudo mkdir /srv/salt/apache2
                                $ sudoedit /srv/salt/apache2/init.sls
                                $ sudo salt '*' state.apply apache2
Sain taas errorin

"State 'apache_install' in SLS 'apache2' is not formed as a list"

Ei onnistunut. Olen kädetön tämän kanssa. Koitin muokata syntaksia, tarkistaa minkä verran välilyöntejä on. Mikään ei toiminut.



![image](https://github.com/user-attachments/assets/a478cbaf-7bbc-4d1e-a3c4-39c199d1bff2)




## Lähteet
Karvinen, 2018, "Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port" (https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh)

Salt Project, "How do i use salt states?",(https://docs.saltproject.io/en/latest/topics/tutorials/starting_states.html)

Geeks for geeks, "How to Change the Root Directory of an Apache server?" (https://www.geeksforgeeks.org/how-to-change-the-root-directory-of-an-apache-server/)
