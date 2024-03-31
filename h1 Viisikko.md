# Kotitehtävät
Kotitehtävät liittyvät kurssiin Palvelinten Hallinta ja löytyvät osoitteesta:

https://terokarvinen.com/2024/configuration-management-2024-spring/

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
- Kokeilin komennoila:

  vangat status
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
- pkg
Komennot 
  $ sudo salt-call --local -l info state.single pkg.installed tree
  $ sudo salt-call --local -l info state.single pkg.removed tree
* Käytin vähän teköälyä (Gemini) apuna tulosten analysoimisessa ja selvisi sitä kautta omatkin epäilykseni koemnnosta. Ensimmäisessä komennossa asennetaan tree-paketti paikalliselle virtuaalikoneelle ja toisella komennolla se poistetaan. Tuloksista näkyy molempien komentojen vaatimat ajat, pakettien polut, luonti sekä poistoaika ja se, suoriutuiko komento annetusta tehtävästä

![pkg](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/c8c0ba22-586e-4693-b7bf-f51277eaa62d)

![pkg remove](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/78b67d51-2246-4a03-a6ff-a9809550aa2f)


