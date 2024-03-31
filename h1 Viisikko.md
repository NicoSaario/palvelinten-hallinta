# Kotitehtävät
- Kotitehtävät liittyvät kurssiin Palvelinten Hallinta ja löytyvät osoitteesta:


https://terokarvinen.com/2024/configuration-management-2024-spring/

* Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmä, päivitykset ajettu 04.01.2024 asti.
* AMD Ryzen 5 4500U, RAM 8 Gt.
*  Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 14.03.2024
* Powershell

### Tiivistelmä
Tehtävänä on tiivistää kolme seuraavaa artikkelia lyhyesti ja yrimekkäästi:

https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/

https://terokarvinen.com/2021/salt-run-command-locally/

https://terokarvinen.com/2023/create-a-web-page-using-github/

a) Salt
- Käytetään ohjaamaan useita slave-koeneita verkon yli
- Hyötyinä se, että voidaan testata, ajaa paikallisesti ja nähdä tulokset välittömästi
- Toimii sekä Linux -, että Windows - ympäristössä
- Tärkeimmät tilatoiminnot pkg, tiedosto, palvelu, käyttäjä ja cmd

b) GitHub
- Voi nopeasti julkaista nettisivun

* Tärkeimmät asetukset:
  - Uusi repo
  - Nimeksi joku simppeli aiheeseen liittyvä hakukoneoptimointia varten (esim. linux-course).
  - Public, jos on valmis näyttämään sivun julkisesti
  - GNU General Public Licence
  
 * New file
- Markdown muuntaa suoraan webbisivuksi
- Äärimmäisen nopea

* Tärkeimmät jutut markdownille:
  - Hashtagit tekee otsikoita, h1,h2,h3 #, ##, ###
  - Tyhjä rivi tekee kappalevälin
  - Urlt renderöityy automaattisesti
  - Koodinpätkät joko neljä välilyöntiä, `` tai Tab

  c) Raportin kirjoittaminen
  - Täsmällinen kuvaus siitä, mitä teki ja mitä tapahtui
  - Ei niin, että tekee ensin, kirjoittaa myöhemmin - Raportoi aina samalla, kun teet!
  - Toimii hyvinä muistiinpanoina ja kuten edellisellä Linux-kurssilla huomasin, tarkkaa rapottia on todella helppo seurata ja suorittaa täten omien raporttien avulla tehtäviä
  - Auttaa selkeyttämään ajatuksia sekä paikantamaan virheitä
  - Raportoi ympäristö - kaikki ei välttämättä toimi samanlailla eri ympäristössä
  - Jokin toinen opiskelija tekee raportin perusteella työn, pitäisi olla sama lopputulos
  - Täsmällisyys! Komennot, klikkaukset, kellonajat, onnistumiset, epäonnistumiset.
  - Helppolukuisuus
  - Lähdeviittaukset

  ## Vagrant ja Salt
  * Olin asentanut Vagrantin jo valmiiksi oppitunnilla. Asennus tehtiin sivulta

  https://developer.hashicorp.com/vagrant/install?product_intent=vagrant

  Ja kuvan mukaisesti valittiin omaan OS sopiva, eli tässä tapauksessa Windows ja AMD64
  
![asennus](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/809e6865-86d5-4b08-8e3c-7630a0d5afc2)

a) Hello Salt
Olin asentanut Saltin jo aikaisemmin oppitunnilla ja kokeilin sen toimivuutta Powerhsellillä komennolla

  salt-call --local grains.items

![salt-call](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/45de1c76-a892-4aaa-a602-8a41ea527685)


b) Hello Vagrant! 
- Kokeilin komennoila


    vagrant status
  
    vagrant global-status

Vagrantin toimivuuden Windowsin Powrshellillä ja se palautti seuraavat vastaukset:
![vagtant-status](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/da0f0436-ccd9-4075-ab69-64aa894154c7)

- Raporttia kirjoittaessani, oli itselläni yksi kone jo luotuna vagrantilla, joten se näytti sen. Vagrant global-status taas näyttää useiden luotujen ympäristöjen tilan. Lähteenä tähän käytin apuna artikkelia https://opensource.com/article/19/12/beginner-vagrant

## Uusi Linux-virtuaalikone Vagrantilla, viisi tärkeintä komentoa, idempotentti, Salt sekä koneen tiedot
* Loin ensin uuden virtuaalikoneen jo asennettuun Vagranttiin


  vagrant up
  vagrant ssh


  ![vagrantup_vagrantssh](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/c5789f59-b2b3-47c7-8ff0-365a1b751867)

* Saltin asennus
- Ensin taas peruskomennot


  sudo apt-get update
  
  sudo apt-get upgrade

- Sitten asennetaan se (olisin voinut vain tarkistaa sudo salt-call --version - komennolla, onko se olemassa)


  sudo apt-get -y install salt-minion

- Se olikin jo asennettuna (uusin versio)

  
  ![salt-call](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/502b87c2-a388-4f8f-bfd2-4acac3dd4e71)


### Viisi tärkeintä komentoa

Alkutiedot (pätee kaikkiin tuleviin komentoihin):
- sudo suorittaa järjestelmänvalvojan oikeuksilla
- salt-call kutsuu Salttia
- --local suorittaa vain paikallisella koneella
- -l info tulostaa info-tilan, jolla saa yksityiskohtaisempia tietoja
- state.single määrittää yksittäisen tila-moduulin

1) pkg

Komennot 

  
  $ sudo salt-call --local -l info state.single pkg.installed tree
  
  $ sudo salt-call --local -l info state.single pkg.removed tree


* Käytin vähän teköälyä (Gemini) apuna tulosten analysoimisessa ja selvisi sitä kautta omatkin epäilykseni koemnnosta. Ensimmäisessä komennossa asennetaan tree-paketti paikalliselle virtuaalikoneelle ja toisella komennolla se poistetaan. Tuloksista näkyy molempien komentojen vaatimat ajat, pakettien polut, luonti sekä poistoaika ja se, suoriutuiko komento annetusta tehtävästä. Kysyin siltä siis, mitä komennot tarkoittavat ja lopputulos oli sana, kuin itselläni.

![pkg](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/c8c0ba22-586e-4693-b7bf-f51277eaa62d)

![pkg remove](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/78b67d51-2246-4a03-a6ff-a9809550aa2f)

2) file
* Komennot


  $ sudo salt-call --local -l info state.single file.managed /tmp/hellonico

  $ sudo salt-call --local -l info state.single file.managed /tmp/moinico contents="foo"

  $ sudo salt-call --local -l info state.single file.absent /tmp/hellotero

- Näyttäisivät tekevän tiedoston.
1) Ensimmäinen komento etsii ja katsoo, onko teidostoa olemassa. Jos sitä ei ole, se luo tiedoston. Muuten samat infot, kuin pkg - kohdassa.

![file_managed](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/69ba240e-41f7-4223-912a-047ba069cdef)

2) Käsittääkseni ja jälleen tekoälyn avustamana, contents="foo" tekee tuon moinicon sisälle arvon foo

![managed_foo](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/5e5b99dd-9ca6-4682-9f40-e42b873f8826)

4) Poistaa tiedoston, jos se on olemassa

![file-absent](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/f3f6c99a-be65-4015-abca-d018517dbb96)

3) service.running
   Yksinkertaisuudessaan tämä komento potkisi Apachea hereille ja automatisoisi apache2 automaattisen käynnistymisen, mutta itselläni sitä tässä harjoituksessa ei ole asennettu, joten se antaa virheilmoituksen, koska service apache2 is not available. Hyvä analysoida virheilmoituksiakin välillä! 
   
$ sudo salt-call --local -l info state.single service.running apache2 enable=True

$ sudo salt-call --local -l info state.single service.dead apache2 enable=False

![apache2_service](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/5dcaf9e6-0aee-42da-9848-0cdeb77e2db5)

4) user. present - User Should Exist
- Komennnot


  $ sudo salt-call --local -l info state.single user.present nicote10

  $ sudo salt-call --local -l info state.single user.absent nicote10

- Etsii ja luo käyttäjän, jonka nimi on tässä nicote10. Absent poistaa käyttäjän. Tiedoista näkee, miten komento tekee groupin, pystyy asettamaan nimet, puhelinnumerot, salasanat jne.
  
Present ![nicote10](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/33cf86e0-290e-4a68-a556-43183b1120a8)

Absent ![nicote10absent](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/836deb5d-cee9-4f47-ab35-6cdb26067552)

4) CMD.RUN - Running a Command

    $ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"

- Luo tyhjän tiedoston /tmp/foo ja varmistaa, ettei sitä muokata jos se on jo olemassa ja suorittaa komennon järjestelmässä.

### Idempotenssi
- Suoirin komennon


  salt-call --local grains.items

- Se tulosti järjestelmän ominaisuuksia ja suorittamalla sen uudestaan, se tekee ihan saman tulosteen uudelleen. Se ei myöskään tee muutoksia järjestelmän tilaan tai kokoonpanoon. Nämä tekevät siitä idempotenssin. Käytin apuna jälleen Gemini - tekoälyä, sillä en ihan täysin ymmärtänyt tehtävänantoa. Komennot kuitenkin suoritin sekä päättelin itse. Teoriapuolessa se oli apuna.

![idempotenssi](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/e8be2280-0e5c-45a6-a4ec-938eea9e111b)

- Testasin vielä ja tein saman komennon uudelleen, joka teki tismalleen saman tulosteen

 ![ipmotenssi2](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/8fb663aa-ab76-44d6-81ff-dee74febcb8f)

### Tietoa koneesta
  sudo salt-call --local grains.item osfinger virtual

![osfinger](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/bc12f995-d971-4ec2-a424-fed48ba8a0e2)

- Näkyy tuloste siitä, että ajetaan Debianin 11 versiota VirtualBoxin kautta

  sudo salt-call --local grains.item cpu_model

- Näyttää CPU:n


![cpu_model](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/93ea559b-b351-412f-bb14-6b560ea6b572)

  sudo salt-call --local grains.item pythonpath

- Näyttää pythonin asennuskansiot

![pythonpath](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/854f7acb-b430-4634-8c1d-f9967d695678)


- Käytännössä jokaisen grains.items - tulosteen kohdan voi yksilöllisesti hakea eri komennoilla ja saada tietoa, jos vain tietää, mitä hakee.


### Lähteet
- Tekoäly (Gemini)
- Kotitehtävien anto: https://terokarvinen.com/2024/configuration-management-2024-spring/
- https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/
- https://terokarvinen.com/2018/control-windows-with-salt/
- https://foxutech.com/salt-commands/
- https://docs.saltproject.io/salt/user-guide/en/latest/topics/grains.html
- https://terokarvinen.com/2023/create-a-web-page-using-github/
- https://terokarvinen.com/2021/salt-run-command-locally/





