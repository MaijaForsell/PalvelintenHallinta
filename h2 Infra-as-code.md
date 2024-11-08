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
- Orjat voivat olla missä tahansa, masterilla pitää olla julkinen serveri ja osoite on tiedettävä
- Jos palomuuri käytössä, master tarvitsee 4505/tcp ja 4506/tcp
- Orjalle voi antaa nimen tai se tulee automaattisesti

  
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

- "/srv/salt/" on tässä esimerkissä kansio, jonka kaikki orja-koneet näkevät ja "/hello" on module
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

- Top.sls määrittää mitä tiloja orjat käyttävät:

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

Samalla vagrantfilellä, aloitan vain uudestaan koneet

                                        $ vagrant up
Päätin tehdä t001 herra-koneen ja t002 orja-koneen. Kokeilin ensin vain lisätä Salt-repot, mutta minulla ei ollut curl asennettuna. Eli asensin sen ja latasin ne repot.
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
Jipii! Nyt olen ylittänyt ongelmakohdan, johon eteneminen aiemmin tyssäsi. Seuraavana menin muokkaamaan orja-koneen tiedostoa.

                                        $ sudoedit /etc/salt/minion
                                        $ sudo systemctl restart salt-minion.service

Nimesin koneeni "pikkuapuri", koska nimellä ei ole varsinaisesti mitään väliä. Master-kohtaan laitoin toisen saamistani t001 Ip-osoitteista. Kirjauduin ulos orja-koneesta ja otin ssh-yhteyden herra-koneeseen (t001).

![image](https://github.com/user-attachments/assets/27e6ff65-80a4-48d7-9f82-9a51d6412b5a)

                                        $ vagrant ssh t001
Halusin hyväksyä pikkuapurin avaimen, mutta komento ei löytänyt pikkuapuria. Eli menin pikkuapuriin (t002) ja muokkasin fileen toisen IP-osoitteen. Ei siltikään toiminut. Kuitenkin pingaaminen toimii, eli koneiden pitäisi olla yhteydessä?
![image](https://github.com/user-attachments/assets/c58fb2a6-9484-4caf-817b-3c92029f2c2c)




                                        $ 


                                        



                                        
                                        
                                        


                                        











## Lähteet:
Karvinen, 2021: "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

Salminen, 2020: "h6" (https://oispadotka.wordpress.com/2020/05/12/h6/)


