# Kotitehtävät
Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 12/05/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 12/05/2024 (pvm/kk/v)

Powershell + Vagrant

## x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
Vapaavalintainen aiemman vuoden kotitehtäväraportti Saltin käytöstä Windowsilla. Löydät raportteja esimerkiksi Google tai Duck-haulla: salt windows karvinen.

Löysin Kristiina Kumila Viikko 6: Windows - tehtäväpalautuksen raportin, jossa käytetään Salttia Windowsilla: https://kristiinakumila.wordpress.com/2021/05/08/viikko-6-windows/

- Windowsilla uusin versio Saltista
- Samassa verkossa herra ja jokanen orja samaan verkkoon
- Raportissa asennellaan Saltti, päivitys uusimpaan versioon
- Jo Saltin Graafisessa asennuksessa voi syöttää Masterin IP
- Oikeudet kaikille kaikkeen ```sudo chmod ug+rwx /srv/salt/win/``` (En vaan oikeen ymmärrä, että Miksi? Aa.. Tässä on käynyt varmaan niin, että asentaja sekoitti olevansa kirjautunut jo root - käyttäjälle eikä siis oikeuksia olisi tarvinnut antaa kansion käyttämiseen..? Muistaakseni Salttiin ei erikseen roottia määritellä)
- ```sudo salt '*' state.apply```
- Ajaa tilan, jolla esimerkiksi init.sls ohjelmien asennuksen voi suorittaa

## a) Asenna Salt Windowsille tai Macille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut. (Jos olet asentanut jo aiemmin, tässä riittää pelkkä asennuksen testaaminen, eikä asennusta tarvitse tehdä uudelleen.)
- Tehtävä itsessään aika yksiselitteinen ja niin on ratkaisukin; Asensin Saltin H6 tehtävää varten, koska se oli prioriteetti silloisiin kotitehtäviin. Kaikki siihen liittyvä materiaali löytyy täältä:
- https://github.com/NicoSaario/palvelinten-hallinta/blob/main/h6%20Benchmark.md
- Suoritan komennot: ``` salt-call --local --version```ja ```salt-call --version```. Ja kuten huomaa, ei olla missään virtuaalikoneessa - siis toimii Windowssilla
- <img width="641" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/d0b96cc1-38f4-4255-bdac-5e86f2656d5a">


## b) Kerää Windows- tai Mac-koneesta tietoa grains.items -toiminnolla. Poimi 'grains.item' perään muutamia keskeisiä tietoja ja analysoi ne, eli selitä perusteellisesti mitä ne ovat. Kuvaile ja vertaile numeroita.

- Vilkaisen ensin, että mitkä olisivat mukavia tietoja keräillä komennolla ```salt-call --local grains.items```
- Se tulostaa listan kaikista vaihtoehdoista ja poimin sieltä: master, osfullname, virtual, timezone ja saltversion:

```salt-call --local grains.items master osfullname virtual timezone saltversion```

<img width="419" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/1c73e8d5-536b-478f-9db3-6c6e5bc1cb1e">

- Eli siis:
    "master: salt" on Saltstackin master-palvelimen nimi ja palauttaa arvon "salt", joka tarkoittaa sitä palvelimen nimeä

- osfullname on tietokoneen käyttöjärjestelmän koko nimi, tässä kohdassa "Microsoft Windows 11 Home"

- saltversion kaikessa lyhykäisyydessään tarkoittaa SaltStackin nykyistä versionumeroa

- timezone: Se on tietokoneen aikavyöhyke, jonka itse asiassa jouduin aikaisemmin muuttamaan sen virheellisyydestä johtuen. Siitä vahvistuu myös raportin aikavyöhyke

- virtual: Tietokoneen tyyppi ja se, ollaanko virtuaalikoneessa vai fyysisessä. Kuten näkyy, ollaan fyysisellä tietokoneella.
  
## c) Kokeile Saltin file -toimintoa Windowsilla tai Macilla

Teen testitiedoston seuraavalla komennolla:

```
salt-call --local -l info state.single file.managed /Users/NicoS/filetesti
```

- Kaiken järjen mukaan tiedosto pitäisi mennä kansioon /Users/NicoS

<img width="435" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/4adf1139-b2d4-4f30-bc3f-22df077e32cf">

Tarkistin vielä, että se tosiaan sieltä löytyy:

<img width="273" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/7a1b479b-0467-48c3-9467-df2458c3a71a">

Poistellaan se sieltä samantien, jotta ei tule myöhempään ihmeteltyä tuota turhaa tiedostoa:

```
salt-call --local -l info state.single file.absent /Users/NicoS/filetesti
```

<img width="404" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/4472f345-ecce-43e3-a348-2d7f033e4c87">

Eikä myöskään näy enää tietokoneella. Toimii siis vallan mainiosti!

## d) CSI Kerava. Näytä 'find' avulla viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostasi. Selitä kaikki käyttämäsi parametrit ja format string 'man find' avulla.

Ei mitään hajua, pitikö tämä tehdä Windowsilla vai Linuxilla, mutta siirryn vagrantille tekemään tämän Linux - ympäristöön, koska komennot ja polut ovat huomattavasti tutumpia. Tämä oli myös jäänyt tunnilla itseltäni käymättä, joten testailen Teron ohjeissa antamaa komentoa. 

- Hyppäsin siis Vagranttiin, jossa olin jo tehnyt alkutoimenpiteet masterille ja kahdelle orjalle. Tässä kohtaa siirryn master - koneelle komennolla ```vagrant ssh```
Kokeilen siis Teron läksyn ohjeissa antamaa komentoa ```find -printf '%T+ %p\n'|sort```. Koska loin vasta hiljattain nuo koneet, enkä muistaakseni siellä mitään ole tehnyt sekä nähdäkseni päivämäärät ovat vähän hujan hajan, teen testitiedoston kotihakemistoon ```mkdir -p näkyykötämätäällä```

- find - komento näyttää seuraavan tulosteen: 

<img width="299" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/9722d9a0-09bf-4907-adad-fe0c40d7c237">

Tulkinta man find - komennon avulla:
- Kuvassa näkyy siis myös se hetki, kun kansio sinne ilmestyi
- printf tulostaa muotoillun tekstin ja tulkitsee merkkejä - jäsentää vasemmalle. Määrittää myös sen, että se tulkitaan oikein, jos etsitään dataa monilta käyttäjiltä ja ne sisältävät useita eri merkkejä, jotka tuottavat hankaluuksia
- '%T+ %p rumakautta n'  T = aika, %p = Tiedoston nimi,  n = Uusi rivi
- sort - järjestää sen

## e) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.
- Lisään komennon Teron ohjeiden mukaan /usr/bin/ - hakemistoon

- Vähän hakuammuntaa, mutta laitan seuraavasti: ```usr/bin/bash echo 'HeisunHei'```
  
- Kokeilen ´´´#!usr/bin/bash```

  <img width="327" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/0896c545-df0c-4c20-9036-affa5aba3f54">

Ilmeisesti toimi, koska HeisunHei palautui

En kuitenkaan tätä osaa tehdä

### Lähteet 

Kristiina Kumila (2021) https://kristiinakumila.wordpress.com/2021/05/08/viikko-6-windows/ (luettu 12.05.2024)

https://github.com/NicklasHH/Palvelinten-hallinta/blob/master/h5%20Tekniikoita/h5%20Tekniikoita.md (luettu 12.05.2024)

Karvinen(2024) https://terokarvinen.com/2024/configuration-management-2024-spring/ (luettu 12.05.2024)

Omat repot

Saltin manuaali 'man find'
