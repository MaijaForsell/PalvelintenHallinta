# Tiivistelmä
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





# Tehtävät









# Lähteet
