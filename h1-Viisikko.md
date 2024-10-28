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


