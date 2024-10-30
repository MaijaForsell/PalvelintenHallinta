# H1 Viisikko
## x)
## Tiivistelmä raportointiohjeista "Karvinen 2006: Raportin kirjoittaminen"
- Käytä imperfektiä kertoessasi mitä teit
- Raportissa käsitellyn tuloksen on oltava toisen opiskelijan toistettavissa
- Raportissa olosuhteet, kuten tietokone, verkko, sovellukset ja käytetty aika on kerrottava, jotta raportti olisi täsmällinen ja toistettavissa
- Kerro mikä onnistui, mikä epäonnistui, miksi epäonnistui, miten koitit korjata, missä vika voisi olla
- Tee raportista selkeä, itselle ja muille mukavampaa seurata oikeassa formaatissa olevaa raporttia
- Merkitse lähteet huolellisesti, mutta ei tarvitse seurata Haaga-Helian lähdeviite-ohjeita web-sivuilla
- Älä valehtele, älä plagioi, käytä vain sellaisia kuvia joihin on oikeudet
- Lisää vakiotekstit



## Tiivistelmä teksteistä "Karvinen 2023: Run Salt Command Locally" ja "Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux" 
Run Salt Command Locally:

- Salt ja sen toiminnot samanlaisia Windows- ja Linux-koneilla
- Tärkeimmät funktiot on pkg, file, service, user ja cmd.
- Asennus alkaa orjan asentamisella, se tapahtuu komennolla  
        `$ sudo apt-get -y install salt-minion`
- Komennot alkavat aina "sudo salt-call", eli "soittaminen"
- Komennot
  - pkg: pakettien asentamista ja poistamista
  - file: tiedostojen hallintaa  
  - service: demonien, eli taustalla toimivien prosessien, hallintaa
  - user: käyttäjien hallintaa
  - cmd: komennon antaminen siten, että se toimii heti
- Ohjeet saa esiin komennolla  
          `$ sudo salt-call --local sys.state_doc`

  

Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux:
  - Komennot master$ ja slave$
- Verkossa on yksi master, monta orjaa
- Orjat voivat olla missä tahansa, masterilla pitää olla julkinen serveri ja osoite on tiedettävä
- Jos palomuuri käytössä, master tarvitsee 4505/tcp ja 4506/tcp
- Orjalle voi antaa nimen tai se tulee automaattisesti  
  `slave$ sudoedit /etc/salt/minion`  
  `master: xx.xx.xx (IP-osoite)`  
  `id: nimi`

- Uudelleenkäynnistä orja-demoni  
  `slave$ sudo systemctl restart salt-minion.service`

- Orjan avaimen hyväksyminen  
  `master$ sudo salt-key -A`
- Hyväksy halutut
- Nyt voi antaa komentoja orjalle
  ## Asenna Debian 12-Bookworm


a) Asentaminen sujui hyvin. Ainoa poikkeava asia verrattuna asentamisohjeisiin "Install Debian on Virtualbox - Updated 2024" oli se, että minun VirtualBox:ista ei löytynyt "Expert mode". Kuitenkin asentaminen sujui muuten ongelmitta. Kaikki toimi kuten kuuluu. Kokeilin asentaa VirtualBox:in uudestaan, mutta en silti asentamisvaiheessa löytänyt mitään vaihtoehtoa.

b) 
Yritin asentaa salt orjaa käyttäen apuna jo aikaisemmin mainittua "Run Salt Locally", mutta en kyennyt. Käytin komentoa "$ sudo apt-get -y install salt-minion", mutta komentorivi kertoi ettei se löydä pakettia.


![image](https://github.com/user-attachments/assets/13ebc5f5-8fea-41da-9f46-55b2f0e6ef9b)


Tässä vaiheessa etain netistä mikä voisi olla ongelma, kunnes palasin tehtävä-sivuille ja tajusin lukea huolellisesti koko sivun. Ja siellä vastaus olikin. Ohjeet Salt:in asentamiseen. Asensin repot, kuten sivuilla lukee.


![image](https://github.com/user-attachments/assets/f433af42-9bfc-446a-b45c-1bceb847e9f3)


Tämän jälkeen pääsin asentamaan itse salt-minionia


c)
Tässä käytin esimerkkinä "Run Salt Command Locally" -sivulta löytyviä komentoja.  

PKG:


$ sudo salt-call --local -l info state.single pkg.installed tree
![image](https://github.com/user-attachments/assets/1b451e6a-3bd8-4fed-8839-b2b9ee9e4376)



pkg-tilafunktio on siis pakettien hallintaa. Tässä näkyy, että paketti "tree" on nyt asennettu ja se kertoo muutokset. Kuvassa näkyy, että se onnistui ja, että tämä oli muutos. Jos annan saman komennon uudestaan, se kertoo ettei muutoksia ole lainkaan (changed=0). 


FILE:


$ sudo salt-call --local -l info state.single file.managed /tmp/tervehdys
![image](https://github.com/user-attachments/assets/a89dca55-161f-40f4-8f97-49d60dd0a596) 

files on tiedostojen hallintaa. Tässä komennossa loin uuden tiedoston, jonka nimi on "tervehdys". Se on tyhjä, mutta esimerkin vuoksi annan sen olla näin. 


SERVICE:


$ sudo salt-call --local -l info state.single service.running apache2 enable=True
![image](https://github.com/user-attachments/assets/f3de5a83-9c59-4ba7-b602-8aa07e393e59)


service on demonien hallintaa. Tässä nähdään Apache-demonin olevan käynnissä jo, eli "service.running" ei muuttanut mitään.


![image](https://github.com/user-attachments/assets/bf66f487-6fb0-4b0d-b8db-6d4b9c50b102)


Tässä esimerkki epäonnistuneesta komennosta. Jos Apache ei ole asennettu, eli sitä ei edes voida käynnistää, se tuottaa tämän tuloksen.

USER:

$ sudo salt-call --local -l info state.single user.present maijaleena
![image](https://github.com/user-attachments/assets/5e655bdb-27ab-417c-b3d8-518ba6cace5e)

user on käyttäjien hallintaa. Tässä esimerkissä tietoja käyttäjästä maijaleena. Ja käyttäjä löytyy, käyttäjän tiedot on ajan tasalla.

CMD:

sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"
![image](https://github.com/user-attachments/assets/8469648b-55d5-4ded-9625-c1465fb3a443)
cmd antaa komennon, joka tehdään vain kerran. Usein käytetään vain testausmielessä, kuten scriptien kanssa. Tässä esimerkissä loin tiedoston "foo", mutta komennossa käsketään komennon tekevän sen vain, jos kyseistä tiedostoa ei ole jo olemassa.

d)

Idemponttius tarkoittaa, että saman komennon käyttäminen ei muuta tilaa jos sitä ei tarvoitse. Se estää bikatiloja hallitessa suurta määrää koneita. Se näkyy siinä, että kaikissa toistuvissa komennoissa kerrotaan onko muutoksia tullut (changed=0).


Ensimmäinen kerta, kun käytin komentoa  

                                $ sudo salt-call --local -l info state.single user.present testaaja

![image](https://github.com/user-attachments/assets/6e4c376c-4e69-47ca-bb0b-e412c3627bea)


Toinen kerta
![image](https://github.com/user-attachments/assets/10003ff6-3750-46e8-84ba-13c879b599e1)


Eli ensimmäisellä kerralla se luo käyttäjän "testaaja" ja toisella kerralla se vain kertoo sen olevan paikalla. Sama käy myös komennolla, jossa luodaan käyttäjästä absent "user.absent". Ensimmäisellä kerralla tila muuttuu ja toisella komentokerralla mikään ei muutu.


e)

Asensin koneelle myös herran. 

Määritin localhostin herraksi orja-koneessa ja käynnistin molemmat.


![image](https://github.com/user-attachments/assets/a9da136d-f5e3-472f-87ec-c4e52f64066b)


Hyväksyin minionin avaimen


![image](https://github.com/user-attachments/assets/587e2b6e-205b-495d-9091-4a75273ec9bb)


![image](https://github.com/user-attachments/assets/f19e7148-6a4c-4de5-b9fa-1a6f7a3cb190)

Orja-kone minioni vastaa kysymykseen


![image](https://github.com/user-attachments/assets/221ecaf5-8ff9-4b3d-8ffb-fdfba77cc0d1)


Hyvin tottelee!


## Lähteet:
https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

https://terokarvinen.com/2021/salt-run-command-locally/

https://terokarvinen.com/palvelinten-hallinta/#esitiedot

https://docs.saltproject.io/salt/install-guide/en/latest/topics/configure-master-minion.html




