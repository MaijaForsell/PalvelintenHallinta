# Infra as code
## X)

Two Machine Virtual Network With Debian 11 Bullseye and Vagrant:

- Nyt käytössä Debian 12-Bookworm
- Vagrantin asennus Linuxilla helposti komenirivillä, Mac ja Windows ladataan ja asennetaan
- Tallenna Vagrant omaan kansioonsa


          $ mkdir twohost/; cd twohost/


          $ nano Vagrantfile

- SSH:n käyttö kirjautumiseen:

          $ vagrant ssh t001

- Kaikki Vagrantin koneeseen liittyvät tiedostot voi poistaa komennolla

         $ vagrant destroy


Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux: 

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


 Karvinen 2014: Hello Salt Infra-as-Code:

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
kohdat "Infra as Code - Your wishes as a text file" ja "top.sls - What Slave Runs What States":

- Mene init.sls sisälle tämä (YAML kanssa indentaatiolla on väliä, tässä kaksi välilyöntiä):

                                        $ cat /srv/salt/hello/init.sls
                                        /tmp/infra-as-code:
                                          file.managed

                                        $ sudo salt '*' state.apply hello
