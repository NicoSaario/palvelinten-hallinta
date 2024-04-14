# Kotitehtävät

- Kotitehtävät liittyvät Palvelinten Hallinta kurssille ja tehtävänannot ovat suoraan sivulta https://terokarvinen.com/2024/configuration-management-2024-spring/

## x) Lue ja Tiivistä

- Tiivistämiseen riittää muutama ranskalainen viiva

1) https://git-scm.com/book/en/v2
* Version Control
- Versionhallinta on järjestelmä, kuten Git
- Se tallentaa tiedostoon tai tiedostojoukkoon tehdyt muutokset, jotta voi palauttaa tietyn version myöhemmin vaikka silloin, kun uudessa versiossa ilmenee ongelmia.
- Säilyttääkseen kuvien tai asettelujen kaikki versiot, puhutaan versionhallintajärjestelmästä
- Versonhallintajärjestelmä voi palauttaa valitut tiedostot entiseen tilaan, nähdä muokkaajat, verrata muutoksia jne.

2) Getting Started - What is Git? https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F
 - Suurin ero Gitin ja muiden VCS:n (Version Control System) välillä on se, että Git "ajattelee" datastaan eri lailla. Muut tallentavat tietoja tiedostopohjaisen muutoksen luettelona, mutta Git pohjautuu sarjaan tilannekuvia
 - Tavallaan Git ottaa kuvan siitä, miltä tiedostot näyttää sillä hetkellä ja tallentaa siihen viittaukset. Jos mikään ei ole mututunut, Git ei tallenna uudelleen vaan linkittää edelliseen identtiseen tiedostoon
 - Tarvitsee vain paikallisia tiedostoja ja resursseja, ei yleensä verkon toiselta tietokoneelta
 - Projektin koko historia omalla levyllä
 - Kolme _TÄRKEINTÄ_ päätilaa: Modified, Staged ja Committed
   * Modified: Olet muuttanut tiedostoa, muttet ole vielä vahvistanut sitä tietokantaan
   * Staged: Olet merkannut muokatun tiedoston nykyisen version, jotta voit siirtyä seuraavaan
   * Committed: Data on turvallisesti tallennettu paikalliseen tietokantaan
 - Kolme pääkohtaa Git projektille: the working tree, the staging area, Git directory
* The working tree: Projektin version yksittäinen kassa. Tiedostot viedään Git-hakemiston pakatusta tietokannasta ja viedään levylle. Niitä voi sitten käyttää ja muokata.
* The staging area: Valmistelualue, joka tallentaa tietoja seuraavasta commitista.
* Git directory: Hakemisto,  johon Git tallentaa metadataa ja objektitietokannan. Gitin tärkein osa, joka kopioidaan kloonatessa toisesta tietokoneesta

3) git add &&  git commit; git pull && git push  - Selitä jokainen osa omista lähteistä https://medium.com/@itsmepankaj/git-workflow-add-commit-push-pull-69adf44cf812
   * Add: Ole valmis tallentamaan mutokset
   * Commit: Ole valmis tallentamaan muutokset ja kertomaan, mitä teit
   * Push: Jaa Commit - Pusketaan etäserverille, kuten GitHubbiin
   * Pull: Hae viimeiset muutokset - Jotta vältetään konflikteja ja pysytään mukana usean ihmisen työskennellessä samanaikaisesti
   * Clone: Aloituspiste yhdessä työskentelyyn. Kloonataan
- Git add - Kertoo, mihin kiinnittää huomiota; ' Lisää - vaihe'.
- Git commit - Lyhyt viesti siitä, mitä on tehty. Selkeä, lyhyt kuvaus siitä, että muut ymmärtävät tehdyt muutokset
- Git push - "Pusketaan" Commitit tietokoneen ulkopuolelle, esimerkiksi jollekin etäserverille, kuten GitHubille. 

  4) https://github.com/terokarvinen/suolax/commits/main/ Commits - historia
  - Kaikki commitit tehty saman päivän aikana, samalta käyttäjältä (paitsi "improve usage instructions)
  - Simppelit demostraatiot siitä, mitä on tehty ja miten Committeja kannattaa kirjoittaa
  - 8 eri committia
 
  ## Varsinaiset tehtävät
 ### a) Online - Tee uusi varasto, nimi summer, description summer
    - Loin GitHubiin uuden repon, jonka nimeksi tulee summer ja descriptioniksi valitsin summer with Git
    - Pittää muistaa laittaa julkiseksi, lisätä README file sekä valita lisenssi GNU General Public Licence v3.0
    -  Sitten create repository

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/93875e1a-a08b-4c2b-8ab3-bb82be117805)

### b) Dolly. Kloonaa edellisesä kohdassa tehty varasto, tee muutoksia omalla koneella, puske palvelimelle ja näytä, että ne ilmestyvät webbiliittymään
- Olin SSH - avaimen GitHubissa liittänyt jo valmiiksi virtuaalikoneelle, joten pitää ainoastaan kloonata se
- Teen seuraavat komennot:

```
mkdir summer ; cd summer
git clone
```
- git clone - komentoon tulee ssh url GitHubin summer reposta

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/6bc32930-72f1-489d-907f-1e19c25e44a8)

- Kokeilin piruuttaan uudelleen kloonata summer - kansioon summer - repon ja huomasin, että se tekee ilmeisesti linkitetyn kansion sen sisään. Ajattelin samalla vähän testata sen funktiota
- Jotta en liikaa eksy ensin aiheesta, teen tiedoston ja sisältöä GitHubiin gitillä seuraavasti:

```
micro jokutiedosto.md
```

```
git add . && git commit; git pull && git push
```

- Laitan ensimmäiseen committiin "Make File", koska halusin testata tuon toimivuutta
- Toiseen committiin "Add Text File summer"
- Päivitän GitHubin selaimesta ja nähtävästi se tuli esiin. Hyvä juttu siis. Myös tuo summer - kansio näyttää ilmaantuneen

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/e9a235ce-8c6c-4192-a6a1-bd1ff832128b)

- Kokeilen seuraavaa: teen md - tiedoston tuohon summer - kansioon, mutta en saa avattua edes koko kansiota. Tuntuu vähän siltä, ettei se toiminutkaan halutulla tavalla, joten kokeilu on toistaiseksi siinä.
- Siinä vielä jokutiedosto.md sisältö

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/bb8b8561-dbd6-4ffe-abb2-4d2c19ee09fa)


### c) Doh! Tee tyhmä muutos gittiin, älä tee committia. Tuhoa huonot muutokset

- Voisin ehkä jatkaa tuota aikaisempaa testiä ja tehdä sen kautta hölmön muutoksen.

```
mv summer/ kesä
```

- Nyt ennen tehtävän suoritusta haluan vielä kokeilla, vaikuttaako tuo "summer" - nimi asiaan.

```
git add . && git commit; git pull && git push
```

- Itse asiassa "tyhmä kokeilu" olikin hyvä kokeilu! Tuolla tavalla voi siis lisätä kansioita ja sen sisään tiedostoja:

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/cec83243-fc67-4260-b2b9-ef6573af5ce9)

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/86de082e-c079-459b-b44b-dce36f3ce946)

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/469810a6-e252-46c9-8463-5f1896b10b5b)


- Hyvin toimii siis ja kätevä tapa tehdä kansioita gitillä GitHubin sisään.
- Tuossa vielä history:

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/3667bfa0-ae2f-429a-b4ba-2d448adbf108)

- No niin ja itse tehtävään.
- Muutan vähän tiedostonimiä ja tiedoston sisältöä seuraavilla komennoilla:

```
mv jokutiedosto.md hömppähomma.md
micro hömppähomma.md
```
- Kuten kuvassa näkyy, tiedoston nimi on muuttunut ja seuraavalla komennolla:

```
git reset --hard
```

- Pitäisi toivon mukaan muutokset resetoitua aikaisempaan committiin.
- Näin näyttää olevan!

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/ca04c313-6a2b-4e05-9c5f-945690b5b3b4)


### d) Tukki. Tarkastele ja selitä varastosi lokia. Tarkista, että nimesi ja sähköposti näkyy haluamalla tavalla ja korjaa tarvittaessa

- Käytin komentoa

```
git log
```

- Sain aikaisemmin asettamani sähköpostin sekä käyttäjän näkyviin! Hieno homma siis. Olin tunnilla asettanut koulun tunnukset sekä user - nameksi kana.
- Analysointi: kuvassa näkyy tismalleen luettelemani muutokset, jossa kellonajat on kyllä hieman kyseenalaisia, sillä teen tätä itse asiassa 15/04/2024 ja kello on 01:26
- Ja logit menee järjestyksessä alhaalta ylös
- Mutta juu. Vaihdoin testaamani summer - kansionimeksi - 'kesä' siinä testissä, jonka mainitsin.
- Julkaisin/yhdistin kansiorakenteen
- Committasin jokutiedosto.md ja tein sinne muutoksia
- Aloitin commitin
- Loin tiedoston "jokutiedosto.md"

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/30c56994-d051-40d6-b419-4f06ff47de54)

### e) Suolattu rakki. Aja Salt-tiloja omasta varastostasi





