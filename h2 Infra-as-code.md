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

Tämän jälkeen näin koneet VirtualBoxissa. Poistin tietysti koneet heti sen jälkeen, jotta en ehtisi kiintyä.

### c) Kaksin kaunihimpi

Seuraavaksi latasin samat koneet, kuin aikaisemmassa osiossa. Tällä kertaa kaikki meni sujuvammin, koska olin jo aiemmin ajanut samat komennot ja tiesin mitä tehdä. Menin VirtualBoxiin ja muokkasin sieltä yhteydet kuntoon, eli vaihdoin NAT-yhteydestä Bridged adapteriin. 
![image](https://github.com/user-attachments/assets/82b8ad32-46fd-426f-aea0-57447c959a0d)










## Lähteet:
Karvinen, 2021: "Two Machine Virtual Network With Debian 11 Bullseye and Vagrant" (https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

Salminen, 2020: "h6" (https://oispadotka.wordpress.com/2020/05/12/h6/)
