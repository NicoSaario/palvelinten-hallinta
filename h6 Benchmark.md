# Kotitehtävät

Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 07/05/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 06/05/2024 (pvm/kk/v)

Powershell + Vagrant

# x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

Sivun alusta "Remove Package" - loppuun, poislukien "Configuration"

Windows Package - Manager https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html

- Salttia voi käyttää Windows-paketinhallintatyökalun asentamiseen, päivittämiseen, pakettien poistamiseen ja Windows etäjärjestelmien hallintaan
- Samanlainen työkalu, mikä Linuxissa esim. yum ja apt

- Ne ovat YAML/JINJA2 tiedostoja .sls - tiedostopäätteellä.
Se määrittää:
- Ohjelmapaketin koko nimen, version, latauspaikan, komentorivivaihdot hiljaiseen asentamiseen sekä sen poistamiseen ja siihen, käyttääkö Windowsin tehtävien ajoitus - työkalua paketin asentamiseen
- Asentamiseen käytettyjä tiedostoja ei jaeta oletusarvoisesti Saltin kanssa. Pitää alustaa ja kloonata winrepo GitHubissa. Se sisältää monille Windows-paketeille määäritystiedostot
- Voi käyttää Salttia tai Gittiä

  Komennot

```
pkg.install
pkg.installed
```

Asentavat paketin käyttäen host OS ja näyttävät, onko paketti asennettu minionille

- Quickstart: 1) Asenna kirjastot (vaihtoehtoinen) 2) Git-säilön täyttäminen 3) Minion-kannan päivitys 4) Ohjelmistopakettien asennus

1) Jos käyttää Saltin Windows - määritelmätiedostoja, jota Salt Git repo isännöi, asennetaan ne
2) "Täytetään Git - repo" eli kloonataan salt-winrepo ja kaikki paketin määritystiedostot haetaan Gitin lokeroista.
3) Päivitetään minionkanta, jonka suorittamalla kaikki minionit luo tietokantoihin merkinnän

```
# From the master
salt -G 'os:windows' pkg.refresh_db

# From the minion in masterless mode
salt-call --local pkg.refresh_db
```

4) Ohjelmistopaketin asennus - lyhyt ja yksinkertainen Firefoxin asennus minioneille

```
# From the master
salt * pkg.install 'firefox_x64'

# From the minion in masterless mode
salt-call --local pkg.install "firefox_x64"
```

### Käyttö

- Konfiguraatioiden jälkeen, voidaan lyödä seuraavia komentoja:

```
pkg.list_pkgs # Näyttää listan kaikista asennetuista paketeista
```

```
pkg.list_available # Näyttää kaikki saatavilla olevat versiot tietystä tiedostosta
```

```
pkg.install # asentaa halutun paketin
```

```
pkg.remove # poistaa sen
```

### Paketin asennus

```
pkg.install
```

Esimerkkinä 

```
# From the master
salt winminion pkg.install 'firefox_x64' version=74.0

# From the minion in masterless mode
salt-call --local pkg.install "firefox_x64" version=74.0
```

- Asentaa tämänhetkisen uusimman paketin Firefoxista
- Loogisesti pkg.remove poistaa paketin ylläolevan esimerkin mukaisesti korvaamalla install - remove


# a) Paketti Windowsia. Asenna Windowsiin tai Macciin ohjelmia Saltin pkg.installed -funktiolla.
- Itse en näitä tunnilla päässyt tekemään Windowsin ongelmista johtuen, joten tässä testaillaan ensimmäistä kertaa
- Asentelen Saltin Windowssille seuraavalta sivulta: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html#install-salt-on-windows
-  Nyt meni pikkasen huuruun taas. Siis törmäsin ongelmaan, että raportoin tässä samalla ja keulin yhden ainoan komennon. Eli siis tässä kävi niin, että suoritin komennon ```salt-call --local winrepo.update_git_repos``` ja se heitti jatkuvaa erroria. Lähdin selvittämään vikaa jälleen noin 400 eri foorumeilta ja käytin ChatGPT:n apua analysoinnissa, koska olin tässä kohtaa jo lopen kyllästynyt Windowsin kanssa pelleilemiseen aikaisempien ongelmien tiimoilta.

ChatGPT:n antama komento

```
salt-call --local winrepo.update_git_repos --log-level=debug
```

Antoi listan 

<img width="536" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/c75efd04-3fe4-46a2-a9cc-ae575c245867">

Ja itse ihmettelin kanssa, miten se herjaa tuota utf-8 koko ajan. Varmistin myös ChatGPT:ltä, mikä voisi olla vikana tuossa rimpsussa ja se vastasi näin: "
Näyttää siltä, että virhe liittyy Unicode-merkkien koodaukseen ja siitä aiheutuvaan ongelmaan, kun SaltStack yrittää hakea tietoja verkosta. Yksi mahdollinen syy tähän voisi olla, että SaltStackilla on vaikeuksia käsitellä tiettyjä merkistöjä tai että jokin verkkoasetus estää sen toiminnan."
- Lähdimme tämän jälkeen selvittämään yhdessä ympäristömuuttujia, vaikka ratkaisu oli aivan nenän edessä jo kättelyssä. Ratkaisu on niin tyhmä ja nerokas samaan aikaan, etten tiedä, itkeäkkö vai nauraa.

- Jälleen vastauksena kysymykseen ```set PYTHONIONENCODING=utf-8```toimimattomuuteen oli "Näyttää siltä, että virheesi liittyy edelleen Unicode-merkkien koodaukseen. Yleinen syy tähän voi olla se, että jokin järjestelmässäsi oleva merkkijono sisältää erikoismerkkejä, jotka eivät ole yhteensopivia UTF-8-koodauksen kanssa."

- Tässä kohtaa tajusin, mistä tilanne nähtävästi johtuu. Vaihdoin läppärin nimestä pois Ä - kirjaimen. Olin siis yön pimeinä tunteina Windowsin Fresh-starttia tehdessäni keksinyt ovelan ja hienon nimen läppärilleni nimeltään "Läpsytin". Vaihoin nimen, otin ääkkösen pois, boottasin koneen ja TA-DAA:

<img width="331" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/ca0e83f1-13d1-42db-8a43-4332080f0c55">

- Poissuljen vielä sen, että kyseessä olisi ollut koneen uudelleenkäynnistyksellä hoituva korjaus: Tein varmuuden vuoksi sen 3 - kertaa ennen kyseistä ääkkösen poistoa. Tähän operaatioon meni yhteensä aikaa 3 - tuntia. Mietin jopa hetkellisesti sitä, ollaanko sitä ihan oikealle alalle menossa.

- Homma jatkuu!

```
salt-call --local pkg.refresh_db
```

<img width="323" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/f6984bd6-c6a9-407e-a766-43caa99dfc91">

Nyt otan kyllä varman päälle ja teen tunniltakin tutun asennuksen:

```
salt-call --local state.single pkg.installed 'firefox_x64
```

Se meni läpi enkä itse asiassa sitä lähde edes poistamaan, koska satun sitä tarvitsemaan. 

<img width="411" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/64eb4c8a-bc33-425d-81cc-5b0555d578a8">


<img width="893" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/1e469cc7-199e-46af-a214-13061aa08a0e">

Lämmin halaus ketulta ja eteenpäin.


## b) b) Benchmark. Etsi 3-7 keskitetyn hallinnan projektia, esimerkiksi tämän kurssin "Oma moduli" lopputyötä.
Tässä ei tehdä testejä, vaan arvioidaan töitä kotisivujen perusteella. Listataan:
- Tarkoitus (business purpose)
- Lisenssi (nimi, missä se lukee ja mitä se tarkoittaa)
- Tekijä ja vuosi
- Riippuvuudet (Alusta, verkkoympäristö tms.)
- Mikä on kiinnostavaa
- Avoimet kysymykset ja muut huomiot


- Aloitin yksinkertaisella googletuksella "palvelinten hallinta oma moduuli".

### viivivepsalainen OMA-PROJEKTI-TILA

* Moduulin yksinkertainen tarkoitus on helpottaa vanhemman ihmisen arkea ja asentaa tarpeellinen sovellus saltin/nettisivun avulla. Lopuksi siitä tuli Firefoxin asennus, joka konfiguroi kotisivuksi Netflixin
* Lisenssi GPL-3.0 licence, joka näkyy heti README - vieressä ja tarkoittaa sitä, että se on avoin ja se suojaa ohjelmiston vapauksia, edistää avoimen lähdekoodin kehitystä ja jakelua https://www.gnu.org/licenses/gpl-3.0.html
* Tekijä oletettavasti Viivi Vepsäläinen, vuosi 2022
* Riippuvuutena joutuu tämä vanhempi ihminen käyttämään Linuxia. Se voi toki olla jopa mahdollisuus!
* Kiinnostavaa itselleni oli se, että kaikki oli tehty README - tiedoston sisään
* Siisti idea!

https://github.com/viivivepsalainen/OMA-PROJEKTI-TILA?tab=readme-ov-file


### ottotoivanen Oma moduli
* Ideana tehdä master-kone, jolla hallitaan kahta minionia, joille asennetaan Steam ja Discord automaattisesti
* Lisenssiä en kyllä äkkiseltään löytänyt, mutta yleisesti Wordpressissä käytetään kyllä GPL-3.0 (ChatGPT)
* Oletettavasti Otto Toivanen tekijänä, nimimerkin perusteella
* Ei ihan kauheasti selvinny riippuvuuksiakaan, paitsi se, että automatisoinnin sijaan Steamin Installeri piti painaa manuaalisesti käyntiin
* Mielenkiintoinen idea ja kiinnosti lähinnä tuo 'valmis paketti tietylle ryhmälle'

https://ottotoivanen.wordpress.com/2020/05/21/palvelinten-hallinta-h7/

### jorilinux h7 Oma moduuli
* Ideana tehdä itelle kivat ohjelmat, jota tykkää käyttää - git, cmatrix, curl ja cowsay
* Sama homma taas ton lisenssin kanssa
* Tekijä Georgios Piperides vuonna 2020
* Riippuvuutena Linux
* Kiinnostaa se, että tässä tehdään myös aika alkeellisia toimenpiteitä, mutta tärkeällä periaatteella - itselle hyödyllistä

https://jorilinux.wordpress.com/palvelinten-hallinta-viikkotehtava-7/

# c) Testbench. Aja toisen tekemä tila.
Olisin tämän vielä halunnut tehdä, mutta rapsan tekoaikaan 07/05/2024 kello 03.50 on pakko mennä nukkumaan, jotta ehtii aamu 8 tunnille oppimaan lisää. Olen noin puoltoista tehtävää tällä hetkellä jäljessä muista. Yritin aloittaa tekemisen valmiilla master-orja kombinaatiolla, mutta Vagrantti ilmoitti muutaman tiedoston olevan korruptoitunut. Poistin twohost - kansion, Vagrantfilen ja tein uudelleen, mutta sekään ei auttanut. Kokeilin sitä toisessa kansiossa ja vagrant up toimi vallan mainiosti, mutta kellon takia joutuu nyt jättämään leikin kesken tältäerää jälleen.

<img width="469" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/8ab2fa35-a39d-48a8-af4a-cde7b6a923de">


- Update: Korjaus https://laracasts.com/discuss/channels/tips/vagrant-machine-index-corrupted-win-81-quick-fix

<img width="455" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/1ff3378b-47bc-49e9-b962-cc6038c38b9e">

UPDATE: Tässähän kävi siis niin, että noiden jälkeen Vagrant up - komennolla Windows heitti BSOD (Blue screen) joka ikisellä kerralla. Päädyin selvittämään tätäkin mukavaa vikaa, jolloin siihen meni taas muutama päivä aikaa. Ongelma ratkesi 10.05.2024, jolloin laitoin HyperV Windowsin asetuksista pois päältä, käynnistin koneen uudelleen, kokeilin - BSOD jälleen. Laitoin HyperV takaisin päälle, käynnistin koneen ja vagrant up toimi, mutta BSOD tuli ensimmäisen komennon jälkeen. Päättelin siis, että ongelma rinnastuu jollakin tavalla Oracle VM toimintaan ja koska HyperV teki pienen muutoksen, eipä ongelma hirveän kaukana voi olla..  Joten vedin Vagrantin kylmäksi, asensin sen uudelleen, poistin Oravle VM, asensin sen uudelleen ja tein varmuuden vuoksi Windowsin Troubleshoot - ominaisuudella Oracle VM version yhteensopivaksi Windowsin kanssa. En tarkistanut, tekikö se mitään muutoksia, mutta kaikki näytti toimivan. Ongelmaa selvitellessäni foorumeilta tuli usein kommentteja vastaan, jossa puhuttiin yhteensopivuuksista Oraclen ja Vagrantin/Windowsin välillä, joten päättelin sen olevan ratkaisu. Asentelin Windowssille päivitykset ja NYT ainakin hetkellisesti kaikki toimii. Koputellaan puuta. Tähänkin ongelmaan meni useampi päivä, sillä muitakin kursseja on eikä konetta voi ihan joka tunti räjäytellä sinisellä ruudulla.

## Update: Uusi yritys

Nyt näyttää kaikki toistaiseksi toimivan, joten lähden https://jorilinux.wordpress.com/palvelinten-hallinta-viikkotehtava-7/ moduulin kimppuun täysin voimin. 

Tämä on siis yksinkertainen ja simppeli moduuli, jossa asennellaan omaan käyttöön ohjelmia, joita tykkää käyttää.

Ohje menee kutakuinkin näi; asennetaan  muutama ohjelma: Git, cmatrix, curl ja cowsay. Luodaan salt - kansiolle "Ohjelmat" - niminen kansio, johon tulee init.sls sisään.

- Saltti on jo asennettu valmiiksi kotitehtävien aikana, joten hypätään suoraan "Ohjelmat" kansion luomiseen ja init.sls - tiedoston tekemiseen
- Vähän raportista poiketen, kerron lisävaiheet myös
- Eli navigointi srv/salt - kansioon ja sinne sisään Ohjelmat - kansio, johon init.sls

```
cd /srv/salt/
```

```
sudo mkdir -p Ohjelmat
```

```
micro init.sls
```

Ja sisään tismalleen samat:

<img width="851" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/61bcfad2-fe39-4106-8ead-5ffccef32397">

- Seuraavaksi Apachelle kansio, johon Moduuli.html - tiedosto ja pieni pätkä tekstiä

<img width="437" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/68c7e9c5-9b3b-4d18-9d2f-5a122f381906">

- Vähän hämärää, miks tässä tehdään tuo php - modi, mutta eipä siinä näytä mitään ihmeellistä olevan. Perus komennot, jotka näyttävät CPU:n ja OS
- Asentelen siis php ```sudo apt-get install php ```
- 



# d) Viisi ideaa. Listaa viisi ideaa omalle modulille, kurssin lopputehtävälle. Modulilla tulee olla tarkoistus. Sen ei tarvitse silti ratkaista mitään oikeaa liiketoiminnan ongelmaa, vaan tarkoitus voi olla keksitty. 

Päivittelen tätäkin tunnilla jahka nuo aivot vähän virkoavat.

1) Tunnilla tulleen 'speed-dating' - osion, jossa keskusteltiin parin kanssa ideoista, heittelin ilmaan ajatuksia siitä, kun näin jonkun hyödyntävän DigitalOeania herra-orja-arkkitektuurin muodossa ja mietin, että sehän olis erittäin tuotantoläheinen ratkaisumalli. Yksi kysymys on myös se, pystyykö VirtualBoxia hyödyntämään testaamisessa esimerkiksi Graafisiin käyttöliittymiin.

2) Tässä lukuisten ongelmien kirjossa, jota itselleni näihin kahteen kurssiin on liittynyt aina vesivahingoista Windows - uudelleenasennuksiin ja siitä erilaisiin korruptoituneisiin tiedostoihin, mietin jotain valmista konfiguraatiota, jota voisin hyödyntää aina uuden koneen 'starttipakettiin'. Esimerkiksi Windows-ympäristön Fresh startti oli työläs erinäisten sovellusten ja pakettien asentamisen muodossa ja niiden pähkälemisessä, mitä sitä oikein tarvitsee. - Niihin asetukset

3) Yksi olisi myös tiedostojen siirtäminen/backupien tekeminen ja sit fresh startti, mutta se on kyl yks mysteeri vielä et miten se toimis käytännössä.. Tai sit livetikun säädöt

### Lähteet
- Oma moduli, ottotoivanen: https://ottotoivanen.wordpress.com/2020/05/21/palvelinten-hallinta-h7/ (Luettu 7.05.2024)
- OMA-PROJEKTI-TILA, viivivepsalainen: https://github.com/viivivepsalainen/OMA-PROJEKTI-TILA?tab=readme-ov-file (Luettu 7.05.2024)
- GNU General Public License: https://www.gnu.org/licenses/gpl-3.0.html (Luettu 7.05.2024)
- ChatGPT
- https://laracasts.com/discuss/channels/tips/vagrant-machine-index-corrupted-win-81-quick-fix
