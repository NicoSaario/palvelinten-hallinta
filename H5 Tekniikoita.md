# Kotitehtävät
Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 12/05/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 12/05/2024 (pvm/kk/v)

Powershell + Vagrant


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


