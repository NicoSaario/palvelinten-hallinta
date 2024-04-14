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
  a) Online - Tee uusi varasto, nimi summer, description summer
    - Loin GitHubiin uuden repon, jonka nimeksi tulee summer ja descriptioniksi valitsin summer with Git
    - Pittää muistaa laittaa julkiseksi, lisätä README file sekä valita lisenssi GNU General Public Licence v3.0
    -  Sitten create repository

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/93875e1a-a08b-4c2b-8ab3-bb82be117805)

b) Dolly. Kloonaa edellisesä kohdassa tehty varasto, tee muutoksia omalla koneella, puske palvelimelle ja näytä, että ne ilmestyvät webbiliittymään
- Olin SSH - avaimen GitHubissa liittänyt jo valmiiksi virtuaalikoneelle, joten pitää ainoastaan kloonata se
- 
