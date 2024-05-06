# Kotitehtävät

Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 23/04/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 06/05/2024 (pvm/kk/v)

Powershell + Vagrant

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

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

