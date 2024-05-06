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
- 
