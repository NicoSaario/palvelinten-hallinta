# Kotitehtävät

Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 23/04/2024 asti.

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


