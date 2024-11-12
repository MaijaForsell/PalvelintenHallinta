# Infra as code
## Tiivistelmät
### X)

Two Machine Virtual Network With Debian 11 Bullseye and Vagrant (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/):

- Nyt käytössä Debian 12-Bookworm
- Vagrantin asennus Linuxilla helposti komenirivillä, Mac ja Windows ladataan ja asennetaan
- Tallenna Vagrant omaan kansioonsa


          $ mkdir twohost/; cd twohost/


          $ nano Vagrantfile

- SSH:n käyttö kirjautumiseen:

          $ vagrant ssh t001

- Kaikki Vagrantin koneeseen liittyvät tiedostot voi poistaa komennolla

         $ vagrant destroy


Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux (https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux): 

Tein aiemmin tiivistelmän, tämä on aikaisemmasta työstäni "h1-Viisikko.md" (https://github.com/MaijaForsell/PalvelintenHallinta/edit/main/h1-Viisikko.md)


- Verkossa on yksi master, monta orjaa
- Minionit voivat olla missä tahansa, masterilla pitää olla julkinen serveri ja osoite on tiedettävä
- Jos palomuuri käytössä, master tarvitsee 4505/tcp ja 4506/tcp
- Minionille voi antaa nimen tai se tulee automaattisesti

  
                  slave$ sudoedit /etc/salt/minion

   
                  master: xx.xx.xx (IP-osoite)

  
                  id: nimi

- Uudelleenkäynnistä orja-demoni
  
                                  slave$ sudo systemctl restart salt-minion.service

- Orjan avaimen hyväksyminen


                                  master$ sudo salt-key -A`
- Hyväksy halutut
- Nyt voi antaa komentoja orjalle


 Karvinen 2014: Hello Salt Infra-as-Code (https://terokarvinen.com/2024/hello-salt-infra-as-code/):

- Kaikki alkaa Salt:in asentamisella
- Micro editor ladataan myös

                                        $ sudo apt-get -y install micro

   
                                        $ export EDITOR=micro

- Modulet asentavat, konfiguroivat ja käynnistävät asioita
- "hello"-nimisen modulen luominen:
  
                                        $ sudo mkdir -p /srv/salt/hello/

- "/srv/salt/" on tässä esimerkissä kansio, jonka kaikki minioni-koneet näkevät ja "/hello" on module
- Seuraavana on mentävä hello-modulen sisään ja voi luoda "init.sls"-tiedoston. Jos micro ladattuna, sudoedit avaa sen.

                                        $ sudoedit init.sls
- Editorissa muokataan sisältöön koodi, joka käskee luomaan tiedoston "hellofile"
                                        /tmp/hellofile:
                                          file.managed

- Hello-modulen saa käyntiin modulen nimellä. Se on idempotentti, eli käskemällä uudestaan, mikään ei muutu.

                                        $ sudo salt-call --local state.apply hello


Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves
kohdat "Infra as Code - Your wishes as a text file" ja "top.sls - What Slave Runs What States" (https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file):

- Luo "/srv/salt/hello" ja sinne "init.sls"
- init.sls sisälle tämä (YAML kanssa indentaatiolla on väliä, tässä kaksi välilyöntiä):

                                        $ cat /srv/salt/hello/init.sls
                                        /tmp/infra-as-code:
                                          file.managed

                                        $ sudo salt '*' state.apply hello

- Top.sls määrittää mitä tiloja minionit käyttävät:

                                        $ sudo salt '*' state.apply hello^C
                                        $ sudoedit /srv/salt/top.sls
                                        $ cat /srv/salt/top.sls
                                        base:
                                          '*':
                                            - hello

- Nyt ei tarvitse erikseen nimetä moduleja:

                                        $ sudo salt '*' state.apply


Salt contributors: Salt overview, kohdat "Rules of YAML", "YAML simple structure" ja "Lists and dictionaries - YAML block structures" (https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml):

- Python pohjalla
- Data muodossa key: value
- Tab ei käytössä, vain välilyönnit
- Kommentit risuaidalla
- Kolme peruselementtiä:
  - Scalars: avain: arvo (arvo voi olla luku, merkkijono tai boolean)
  - Listat: avain: arvot jokainen omalla rivillään, kaksi välilyöntiä ja viiva
  - Sanakirjat: kokoelma "avain: arvo" listauksia
- YAML käyttää lohkoja, eli sisennys määrittää kontekstin
- Standardikäytäntö on kaksi välilyöntiä
- Kokoelma, eli lista tai sanakirjan lohko, merkitsee jokaisen kohteen viivalla ja välilyönnillä ("- ").


## Tehtävät
Käytin tehtävien tekemiseen kannettavaa tietokonettani, mallia Lenovo Yoga Slim 6
### a) Vagrant asennettu

Tehtävään meni kokonaisuudessaan alle minuutti.

Käytin komentoa

                                        $ vagrant --version

![image](https://github.com/user-attachments/assets/e757ba04-40a0-4ff0-958a-be9326f2f004)

### b) Linux-kone Vagrantissa

Vagrant oli minulle uusi työkalu, joten tämä tehtävä oli puhdasta oppimista. Minulla meni 20 minuuttia ennen, kuin sain tehtyä minkäänlaista konetta. Lopulta sain parhaiten apua Antti Salmisen "h6"-sivulta (https://oispadotka.wordpress.com/2020/05/12/h6/). Lopputehtävä meni noin 10 minuuttia.

Käyttämäni komennot olivat
                                        $ mkdir firstmachine
                                        
                                        $ cd firstmachine
                                        
                                        $ vagrant init
                                        
                                        $ notepad vagrantfile
                                        

Tässä kohdassa muokkasin siis notepadilla vagrantfilen sisällön lähteestä "Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant", mutta muutin sen Bullseye:sta Bookworm:iin.
![image](https://github.com/user-attachments/assets/a80a8927-49cf-4e6b-bc79-38dcad0c8662)

Tämän jälkeen näin koneet VirtualBoxissa. Poistin tietysti koneet heti sen jälkeen, jotta en ehtisi kiintyä. Jouduin myös poistamaan omalta koneeltani vagrantfilen erikseen
                                        $ del vagrantfile
                                        

### c) Kaksin kaunihimpi

Seuraavaksi latasin samat koneet, kuin aikaisemmassa osiossa. Tällä kertaa kaikki meni sujuvammin, koska olin jo aiemmin ajanut samat komennot ja tiesin mitä tehdä. Kuitenkin ongelmia nousi salt-latauksissa. Kaikkiaan tähän meni yli tunti, sillä jouduin tutkimaan Salt-sivuja. 

Kirjauduin t001-koneelle ja pingasin t002-konetta.

                                        $ vagrant ssh t001
                                        vagrant@t001$ ping -c 1 192.168.88.102
![image](https://github.com/user-attachments/assets/e28e3371-91ce-46ce-84f1-3f9052da7248)

Tein saman myös t002-koneella ja pingasin t001-konetta.
![image](https://github.com/user-attachments/assets/bdc1844f-5d18-400d-985b-ef1d907e0ce9)

Ja taas poistohommiin
                                        $ vagrant destroy


### d) Herra-orja verkossa

Tähän tehtävään meni minulta suunnilleen kolme tuntia, mutta siihen sisältyy paljon tutkimista ja kahvin keittämistä.

Samalla vagrantfilellä, aloitan vain uudestaan koneet.

                                        $ vagrant up
Päätin tehdä t001 herra-koneen ja t002 minion-koneen. Kokeilin ensin vain lisätä Salt-repot, mutta minulla ei ollut curl asennettuna. Eli asensin sen ja latasin ne repot.
                                        $ sudo apt install curl
                                        $ sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/debian/12/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg

Sain kuitenkin vastauksksi "Could not resolve host: repo.saltproject.io". Kopioin tämän suoraan Google-hakuun ja sain selville Salt Projectin sivuilta, että he muuttivat repo-osoitteita juuri lokakuun lopussa. Se selittää miksi aikaisemman tehtävän teossa tuo komento toimi, mutta nyt ei. Laitoin siis uuden repo-osoitteen, jonka sain sivuilta. Selailin hiukan, että saisin tietää mikä on oikea. Näin siellä mainittuna DEB-packages, jotka ovat Debianille sopivat. Myös ohjeiden seuraavat käskyt näyttivät tutuilta, sillä  ne olivat muotoa "sudo apt-get...". Eli näillä mentiin. Pahimmassa tapauksessa joutuisin vain tuhoamaan vagrant-koneet ja yrittämään uudestaan.

                                        $ curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring-2023.pgp
                                        $ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.pgp arch=amd64] https://packages.broadcom.com/artifactory/saltproject-deb/ stable main" | sudo tee /etc/apt/sources.list.d/salt.list

Näillä säätämisillä sain salt-masterin toimimaan! 
                                        $ sudo apt-get -y install salt-master


Seuraavaksi otin ssh-yhteyden t002-koneeseen ja latasin samat repot sinne. En kuitenkaan saanut selville miksi echo-käsky ei löytänyt konetta. En osannut korjata ongelmaa, joten päätin heittää koko koneen roskakoriin. Ylitseampuvaa, kenties, mutta toimivaa. Eli tuhosin koneet ja aloitin uudestaan.

Tein kaiken samalla tavalla, kuin aikaisemmin, mutta tällä kertaa tiesin miten. Tässä käskyt, joita käytin päästäkseni ongelmakohtaan.

                                        $ vagrant ssh t001
                                        $ sudo apt install curl
                                        $ curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring-2023.pgp
                                        $ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.pgp arch=amd64] https://packages.broadcom.com/artifactory/saltproject-deb/ stable main" | sudo tee /etc/apt/sources.list.d/salt.list
                                        $ sudo apt-get update
                                        $ sudo apt-get -y install salt-master
                                        $ hostname -I
                                        $ exit
                                        

Nyt herra-kone toimi. Otin myös IP-osoitteen talteen, jotta sen käyttö seuraavassa osassa ei olisi ongelma. Sain kaksi vastausta, eli "10.0.2.15 192.168.88.101", lisäksi myös IPv6 osoitteen, mutta en tarvitse sitä nyt. Seuraavana otin ssh-yhteyden t002-koneeseen. Nyt kiinnitin enemmän huomiota, jos vaikka saisin toimimaan.
                                        
                                        $ vagrant ssh t002
                                        $ sudo apt install curl
                                        $ curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring-2023.pgp
                                        $ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.pgp arch=amd64] https://packages.broadcom.com/artifactory/saltproject-deb/ stable main" | sudo tee /etc/apt/sources.list.d/salt.list
                                        $ sudo apt-get update
                                        $ sudo apt-get install salt-minion
Jipii! Nyt olen ylittänyt ongelmakohdan, johon eteneminen aiemmin tyssäsi. Seuraavana menin muokkaamaan minion-koneen tiedostoa.

                                        $ sudoedit /etc/salt/minion
                                        $ sudo systemctl restart salt-minion.service

Nimesin koneeni "pikkuapuri", koska nimellä ei ole varsinaisesti mitään väliä. Master-kohtaan laitoin toisen saamistani t001 Ip-osoitteista. Kirjauduin ulos minion-koneesta ja otin ssh-yhteyden herra-koneeseen (t001).

![image](https://github.com/user-attachments/assets/27e6ff65-80a4-48d7-9f82-9a51d6412b5a)

                                        $ vagrant ssh t001
Halusin hyväksyä pikkuapurin avaimen, mutta komento ei löytänyt pikkuapuria. Eli menin pikkuapuriin (t002) ja muokkasin fileen toisen IP-osoitteen. Ei siltikään toiminut. Kuitenkin pingaaminen toimii, eli koneiden pitäisi olla yhteydessä?
![image](https://github.com/user-attachments/assets/c58fb2a6-9484-4caf-817b-3c92029f2c2c)

Seuraavana kokeilin tuttua ja turvallista "oletko kokeillut laittaa sen pois päältä ja uudelleen päälle?". Menin herra-koneeseen ja katsoin onko demoni päällä. Ja sitten käynnistin sen. Sain vastauksen kysymällä ChatGPT:ltä "miten tietää onko salt-master käynnissä?"
                                        $ sudo systemctl status salt-master
                                        
![image](https://github.com/user-attachments/assets/c95d8e0e-f0e0-4f98-9138-3da7e11e8207)

No ei ollut käynnissä. Käynnistetään...

                                        $ sudo systemctl start salt-master


![image](https://github.com/user-attachments/assets/8121c3ac-2231-4fb7-872e-3a51a5b2888c)

Ja siellä se on! Yhteys!

Tässä koko tehtävän ja tutkimisen tulos:
![image](https://github.com/user-attachments/assets/2a9e4814-7851-48bd-b0ca-2885ef9e118b)

Halusin kokeilla tehdä tämän kaiken vielä kerran uudelleen, ihan vain todistaakseni itselleni ettei tämä ole niin vaikeaa. 


Annoin komennon "$ Vagrant up" klo 12.08

Koneet olivat pystyssä klo 12.14

Herra-kone toiminnassa klo 12.20

Minion-kone (apuri2) toiminnassa 12.25

Yhteys näiden välillä klo 12.28


![image](https://github.com/user-attachments/assets/cd82e81a-aeec-40b2-b80e-20a42f4b361c)


### e) Hei infrakoodi!

Tehtävään meni noin viisitoista minuuttia.

Apunani käytin Karvinen: "Hello Salt Infra-as-Code"
Käytin viimeksi tekemääni yhteyttä, jossa herra-kone on t001 ja minion-kone on t002. Ensimmäisenä otin ssh-yhteyden minion-koneeseen. Sen jälkeen loin uuden hakemiston testailua varten ja menin sinne.
                                        $ sudo mkdir -p /srv/salt/testing
                                        $ cd /srv/salt/testing
Seuraavaksi loin "init.sls"-tiedoston.

                                        $ sudoedit init.sls
Sisällöksi laitoin:

/tmp/iltaa:
  file.managed

  Annoin komennon toteuttaa kyseinen koodi.
  
                                          $ sudo salt-call --local state.apply testing
                                          
![image](https://github.com/user-attachments/assets/47c1c650-4beb-4b9b-898e-1c6667bed0f1)

Kysyin koneelta onko tälläistä tiedostoa olemassa. Ja oli.

![image](https://github.com/user-attachments/assets/99b12fe1-c919-40ec-826c-5eda494f3b68)

### f) Aja esimerkki sls-tiedostosi verkon yli orjalla.

Tehtävään meni noin puoli tuntia. Käytin lähteenä Karvinen 2023: "Salt Vagrant - automatically provision one master and two slaves"

Otin ssh-yhteyden herra-koneeseen. Loin hakemiston "overthenetwork" ja loin sinne sls-tiedoston, jolla luon tiedoston.
                                        $ sudo mkdir -p /srv/overthenetwork
                                        $ sudoedit /srv/salt/createfile.sls

Tiedoston sisälle kirjoitin:


createfile:

  file.managed:
  
    - name: /tmp/hello.txt
    

Sitten annoin minionille käskyn.

                                        $ sudo salt '*' state.apply createfile

![image](https://github.com/user-attachments/assets/66670acc-8d5a-4716-b054-0191111b0aea)

Seuraavaksi otin ssh-yhteyden minioniin, jotta voisin tarkistaa tuloksen.

Ja siellä se oli.

![image](https://github.com/user-attachments/assets/82c320b2-6953-40d3-a00f-7a1033823b86)


### g) Tee sls-tiedosto, joka käyttää vähintään kahta eri tilafunktiota näistä: package, file, service, user.

Käytin apuna Salt Project:in sivuilta löytyvää "Management of user accounts" (https://docs.saltproject.io/en/latest/ref/states/all/salt.states.user.html)

Työstin tehtävää noin 2 ja puoli tuntia.

Otin ensimmäisenä ssh-yhteyden herra-koneeseen. Loin sinne hakemiston "writestuff" ja siihen hakemistoon tiedoston "userstuff.sls".


                                        $ sudo mkdir -p /srv/salt/writestuff
                                        
                                        $ sudoedit /srv/salt/writestuff/userstuff.sls

Tässä kohdassa olin aivan hukassa siitä, miten syntaksia kirjoitetaan tai miten voin käyttää muita, kuin file.managed järkevästi. Päätin, että en tee mitään loogista, vaan lisään jotakin tavaraa sls-tiedostoon.

userstuff.sls sisältö:


![image](https://github.com/user-attachments/assets/d7aa426a-f933-452c-a064-fb22470c97f6)

  Kokeilin ajaa komennon. 
                                          $

Sain vain pitkän error-listan.
![image](https://github.com/user-attachments/assets/5f1cec92-ad24-469f-b634-7c7facf1651c)

Tajusin, että en ole antanut käyttäjälleni "nimeä", vain "fullname", joka ei ole sama asia. Katsoin myös Salt Project ohjeesta, että voin tehdä myös yksinkertaisemman käskyn. Muutin sls-tiedoston sisältöä:


![image](https://github.com/user-attachments/assets/b383ba9f-f1ab-43be-9030-c1be80c63bc4)



Taas error:
Salt request timed out. The master is not responding. You may need to run your command with `--async` in order to bypass the congested event bus. With `--async`, the CLI tool will print the job id (jid) and exit immediately without listening for responses. You can then use `salt-run jobs.lookup_jid` to look up the results of the job in the job cache later.


Tässä kohdassa turhauduin hiukan ja luin koko error-koodin huolella. Ilmeisesti herra-kone ei vastaa, joten kysyin onko salt-master demoni käynnissä. No ei ollut.


![image](https://github.com/user-attachments/assets/4cdd8dac-2046-4fb0-a1aa-646cfb4d5f41)

Pahasti vaikuttaa, että yritykseni tehdä itse YAML-koodia täysin väärällä syntaksilla teki jotakin. Aika aloittaa uudestaan. En halua kokeilla korjata jotakin, jossa lukee isolla "failed" ja sana "kill". Hyvästi apuri2.


Tuhosin koneet ja loin uudet, kuten tehtävässä c). Loin saman nimisen hakemiston ja tiedoston, samalla sls-sisällöllä.

create_user:
  user.present:
    - name: testuser

create_file:
  file.managed:
    - name: /tmp/success

                              $ sudo salt '*' state.apply userstuff
    
Sain komennosta vastaukseksi:

No matching sls found for 'userstuff' in env 'base'

Eli ei löydy oikeasta paikasta. Sls-tiedoston pitäisi olla "srv/salt/" kansiossa. Eli loin uuden tiedoston sinne ja kokeilin uudestaan samalla sisällöllä.

                              ![image](https://github.com/user-attachments/assets/9fd637fa-b124-463f-8b9b-c12db85963cd)


Toimii! Uudestaan yrittäessä mikään ei muuttunut. 

![image](https://github.com/user-attachments/assets/7a87ba0a-a04d-4202-af38-758c8430f37c)

Otin ssh-yhteyden minion-koneeseen ja tarkistin, että tiedosto "success" löytyy 

                                        $ cd /tmp/
                                        $ ls -la

                     ![image](https://github.com/user-attachments/assets/bd24e113-f3ff-4b66-bfb7-eb62259cda69)


### h) Top file
23.55

Sain tehtävään apua Salt Projectin sivuilta "The Top File".

Otin ssh-yhteyden herra-koneeseen. Loin kaksi sls tiedostoa. Nimet oli "hello" ja "apache".


                                         $ sudoedit /srv/salt/hello.sls
                                         $ sudoedit /srv/salt/apache.sls
                                         
hello.sls sisältö:

hello:

  file.managed:
  
    - name: /tmp/helloworld.txt

apache.sls sisältö:


install_apache:

  pkg.installed:
  
    - name: apache2

Seuraavaksi loin top.sls-tiedoston.

                                        $ sudoedit /srv/salt/top.sls
top.sls sisältö:
base:

  '*':
  
    - hello
    
    - apache
    
Annoin komennon toteuttaa top.sls.

                                        $ sudo salt '*' state.apply

Se loi kyllä "helloworld"-tiedoston, mutta ei asentanut apachea. Sain error-koodin:
                                        
An error was encountered while installing package(s): E: Release file for https://deb.debian.org/debian/dists/bookworm-updates/InRelease is not valid yet (invalid for another 9h 55min 5s). Updates for this repository will not be applied.

![image](https://github.com/user-attachments/assets/b381580e-fbc1-4d90-84d3-766f1129d16b)

Myöskään päivittäminen ei auttanut.

![image](https://github.com/user-attachments/assets/3fac7e68-de24-4da8-84a8-1b0bbc530042)

Päätin siis tehdä toisenlaisen top.sls-tiedoston. Poistin apache.sls ja korvasin sen user.sls

                                       $ sudo rm /srv/salt/apache.sls
                                       $ sudoedit /srv/salt/user.sls

user.sls sisältö:

create_user:

  user.present:
  
    - name: apachenkorvike

Muokkasin top.sls-tiedostoon apachen tilalle user.

base:

  '*':
  
    - hello
    
    - user

                                        $ sudo salt '*' state.apply
                                        

Ja tällä top-tiedostolla sain luotua käyttäjän ja tekstitiedosto oli jo olemassa. Idempotentti koodi siis.

![image](https://github.com/user-attachments/assets/78430071-5ff3-48d1-9c55-52fdc90569f5)








    









                                        





                              


                                        



                                        
                                        
                                        


                                        











## Lähteet:
Karvinen, 2021, "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

Karvinen, 2023, "Salt Vagrant - automatically provision one master and two slaves" (https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

Karvinen, 2014, "Hello Salt Infra-as-Code" (https://terokarvinen.com/2024/hello-salt-infra-as-code/)

Karvinen 2018, "Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux", (https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

Salt contributors, "Salt overview" (https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml)

Salt Project Team, 2024, "Salt Project Package Repository (repo.saltproject.io) Migration and Guidance" (https://saltproject.io/blog/salt-project-package-repo-migration-and-guidance/)

Salminen, 2020, "h6" (https://oispadotka.wordpress.com/2020/05/12/h6/)

Salt Project, "The Top File" (https://docs.saltproject.io/en/latest/ref/states/top.html), luettu: 12.11.2024


