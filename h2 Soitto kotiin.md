# Kotitehtävät

* Kotitehtävät liittyvät kurssiin Palvelinten Hallinta ja löytyvät osoitteesta:
https://terokarvinen.com/2024/configuration-management-2024-spring/

* Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 09/04/2024 asti.
* AMD Ryzen 5 4500U, RAM 8 Gt.
* Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 09/04/2024
* Powershell + Vagrant

## x) Tiivistelmät
Tehtävänä tiivistää lyhyesti muutamalla ranskalaisella viivalla seuraavat linkit:
- https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/
- https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux
- https://terokarvinen.com/2024/hello-salt-infra-as-code/

### a) Two machine virtual network
  - Vagrantin asennus
  - Luodaan projektille hakemisto ja laitetaan sinne Vagrantfile

```
$ mkdir twohost/; cd twohost/
$ nano Vagrantfile
```
- Sen jälkeen on mahdollista ssh yhteydellä liittyä ssh001 ja ssh 002 - exit=takaisin host OS
- Molemmat voivat yhdistää toisiinsa sekä internettiin

```
vagrant destroy #tuhoaa kaiken molemmista virtuaalikoneista
vagrant up #tekee uuden, kokonaan tyhjän
```

### b) Salt Quickstart
- Saltin avulla voi hallita tuhansia koneita
- Orjat voivat olla missä tahansa, NATin ja  palomuurin takana tai tuntemattomassa osoitteessa

- Masterin luonti:

```
master$ sudo apt-get update
master$ sudo apt-get -y install salt-master
master$ hostname -I
10.0.0.88
```
- Jos masterilla on palomuuri, pitää tehdä rei´ät 4505/tcp sekä 4506/tcp

- Orjan luonti:
```
slave$ sudo apt-get update
slave$ sudo apt-get -y install salt-minion
```
- Orjan pitää tietää herran sijainti. Jokaisella orjalla tarvitsee olla oma id
- Esim:

```
slave$ sudoedit /etc/salt/minion
master: 10.0.0.88
id: orja1
```

- Lopuksi potkitaan hereille:
  
```
slave$ sudo systemctl restart salt-minion.service
```
- Kokeilu
```
master$ sudo salt '*' cmd.run 'whoami'
orja1:
 root
```

- Muita komentoja:

```
master$ sudo salt '*' cmd.run 'hostname -I'
master$ sudo salt '*' grains.items|less
master$ sudo salt '*' grains.items
master$ sudo salt '*' grains.item virtual
master$ sudo salt '*' pkg.install httpie
master$ sudo salt '*' sys.doc|less
```

### c) Hello Salt
- Asennus

```
$ sudo apt-get update
$ sudo apt-get -y install salt-minion
```

- Itse ainakin suosin microa, joten sen asennus:

```
$ sudo apt-get -y install micro
$ export EDITOR=micro
```

- Sille luodaan kansio, vaikkapa "hello" moduulille.
- Moduulit asentaa, konfiguroi ja tallentaa.
- Voitaisiin luoda vaikka Apachelle ja Micro editorille omat moduulit:

```
$ sudo mkdir -p /srv/salt/hello/
$ cd /srv/salt/hello/
```

* /srv/salt - kansio jakaa kaikille orjatietokoneille
* Kansio hello/ sisältää kaiken liittyen hello worldiin, salt koodiin ja kaikkiin tiedostoihin tai templateihin.

Infra as a code
- Pitää olla kansion '/stv/salt/hello' sisällä

```
sudoedit init.sls
```

- Avaa microeditorilla tiedoston, johon voidaan laittaa idempotentteja, esim:

```
/tmp/hellonico:
  file.managed
```

- Katsotaan ja ajetaan seuraava komento, josta myös näämme yksityiskohtaisen selityksen siitä, mitä on tehty:

```
$ sudo salt-call --local state.apply hello

```

## a) Kaksi virtuaalikonetta samaan verkkoon. Osoita, että koneet voivat pingata toisiaan ja, että pystyt käyttämään kumpaakin konetta

- Tähän väliin täytyy sanoa, että itseltäni jäi tuo oppitunti välistä, joten aloitin tekemään tehtävää täysillä nollatiedoilla. Pyrin noudattamaan https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ ohjeistusa, mutta jostain syystä sain aina vastauksen "the machine with the name was not found configured for this vagrant environment".
- Tarpeeksi, kun hakkasin päätä seinään, yritin eri lähteistä todettuja metodeja, esimerkiksi https://stackoverflow.com/questions/23482341/the-machine-with-the-name-c6401-was-not-found-configured-for-this-vagrant-env ohjeessa komentoa, jotka eivät tuottaneet tulosta

```
vagrant global-status --prune
```

- Päädyin hieman lunttaamaan ohjeita kurssitoverilta ja seuraava show on täysin toteutettu https://github.com/KebabGarva/Linux-palvelinten-hallinta-bgu248/blob/main/h2.md ohjeiden mukaan.
- Sakun ohjeiden mukainen suoritus alkaa siis tästä:

* Aloitin tuhoamalla aiemmin tehdyt tiedostot, jotka olin luonut ohjeiden mukaan. Ne eivät kuitenkaan tuottaneet toivottua lopputulosta, joten päätin kokeilla Sakun esimerkkituhoamista

```
Remove-Item -Path C:\Users\sakus\twohost -Recurse
```

* En kuitenkaan päätynyt käyttämään tismalleen tuota, sillä syystä tai toisesta tuo Recurse jäi huomaamatta, joten poistin ne yksitellen saaden samalla varmuuden siitä, mitä olen tekemässä

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/88af1d9b-c6b9-4cbc-a3ff-6ff331a73b4e)

* Kuten näkyy, varmistin vielä komennolla

```
cd ./Users/NicoS/twohost
```
ettei tiedostoa ole edes olemassa ja näinhän se oli.

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/eb77a683-102b-44e7-b1ba-6027aab427da)

- Loin siis Vagrantfilen init - komennolla ja muokkasin komennolla

```
  start notepad++ ./Vagrantfile
```

- Laitoin https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ konffiohjeen mukaisen tekstirimpsun sisään, tallensin ja poistuin notepadista.

* Ongelma näyttää siis ratkenneen ainakin osittain. Kuvittelin ongelman olevan jostain syystä tavallisesssa notepadissa, jota käytin aikaisemmin.. Nappasin siis aikaisemminkin käyttäneeni Notepad++ asennuksen nopealla surffauksella ja sain varmuuden omillle harhaluuloilleni. Uskoisin kuitenkin, että ongelma oli ensinnäkin siinä, ettei PowerShell ollut auki adminioikeuksilla sekä se, että jokin tiedoston polussa/tiedostonimessä oli harhautunut. Muutaman lähteen mukaan Vagrantin default - nimisen koneen nimi pitäisi muuttaa/muuttua, mutta mielestäni ajetun konffin olisi pitänyt se tehdä(?); näin ei kuitenkaan ollut.

* Toinen huomio oli tuo vagrant init - komento suoraan luotuun kansioon. Unohdin sen tehdä kokonaan ja kuvittelin, että pelkkä "start notepad++ Vagrantfile" olisi luonut tarvittavan tiedoston, mutta nyt kun tutkin sitä tarkemmin, twohost - polusta puuttui aikaisemmin .vagrant sekä shared - kansiot. Ongelma taisi piillä myös siinä.

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/ae56a1fd-fd6f-42e6-becf-234cbfa72422)

* Homma siis rullaa eteenpäin vihdoin. Kiitokset Sakulle kattavasta ohjeesta.
-  Tähän päättyy https://github.com/KebabGarva/Linux-palvelinten-hallinta-bgu248/blob/main/h2.md noudattamani "lunttausohje"

## a) Uusi startti
- Suoritin komennon

```
vagrant up
```

ja huomasin heti muutokset, jotka oli tehty. Aikaisemmin yhteyden muodostamisen koodirivit olivat simppelimpiä, lyhyempiä, default - nimellä ja kestivät vain muutaman sekunnin. Nyt koneen luomisessa meni noin 60s ja toiminnot olivat hyvinkin erinäköisiä. Muutoksen huomaa jo ensimmäisiltä riveiltä.

![vagrantinit_up](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/0aa895c3-ebce-4626-bfdb-5f1a928e902a)

- Nyt jatketaan https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ ohjeen mukaisesti.
- Otetaan testiyhteys t001:

```
vagrant ssh t001
```

- Hurraa. Se toimii. 

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/5b01d184-c885-497a-904b-8a469bac3192)

- Testataan nyt samalla t002 varmuuden vuoksi sekä exit

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/d6fbd54e-a81a-46ac-a50a-a6f7e393ee57)

- Seuraavaksi ping- komennolla varmennetaan t002 ja t001 yhteys toisiinsa sekä Googlen nameserverille, jonka osoitteena toimii 8.8.8.8. Tämä varmistaa sen, että ollaan yhteydessä myös ulkomaailmaan.
- Kuvassa valkoisella on t002, keltaisella t001 ja vastaukset punaisella:


![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/fa8da81a-b2f1-427f-a530-b9ed3e64941e)



## b) ja c) Salt herra-ohja. Asennetaan toimimaan ne verkon yli sekä suoritetaan komento

- Olin jo valmiiksi t001 - koneella, joten ajattelin tehdä siitä masterin.
- Noudatan aikaisemmin tiivistettyä ohjetta https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux
- Aloitan asentamalla t001 Masterin komennoilla

```
master$ sudo apt-get update
master$ sudo apt-get -y install salt-master
master$ hostname -I
10.0.0.88
```
- Halusin myös selvittää samalla, mitä nämä tarkoittavat ja käytin Gemini - tekoälyä apuna asian ratkaisemiseksi. hostname -I "hakee verkkokortin IP-osoitteen ja tarvitsee tiedon kommunikoidakseen minionien kanssa" ja "10.0.0.88 konfiguroi sen kuuntelemaan IP-osoitteella 10.0.0.88". Käsitän siis, että tuo hostname hakee sen IP-osoitteen, jota hyödynnetään orjien yhdistämisessä
- Itselläni se palautti arvot:
* 10.0.2.15 ja 192.168.88.101

- Seuraavaksi avasin sekä t001 ja t002 yhteyden samanaikaisesti omiin ikkunoihinsa, jotta toimiminen on helpompaa. Avasin orjalle t002 komennolla

```
sudoedit /etc/salt/minion
```
orjatiedoston, johon tarvitsee syöttää seuraavat arvot:
* Orjan tarvitsee tietää, missä master on. Kokeillaan ensin 10.0.2.15, vaikka epäilen hieman sen toimivuutta
* Laitoin tiedot orjatiedoston alareunaan
* Orjan ID. Tässä tapauksessa id:testiorja


- Hakkasin hetken päätä seinään taas tehtävän parissa, mutta "brute-force" toimi tälläkertaa ja muutaman kerran kokeilun jälkeen teksti: "The following keys are going to be accepted..." ilmestyi, kun käytin osoitetta 192.168.88.101 tuon toisen sijaan. Vaihdoin myös parametrit orjatiedoston yläreunaan. Potkin myös orjaa hereille komennolla:

```
sudo systemctl restart salt-minion.service
```
- Eli tässä toimi 192 osoite, parametrien asetus tiedoston yläreunaan sekä id:n asetus testiorjaksi. Lopputulos:

 ![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/4a093254-f5d8-4b2b-ab60-c50c1c8e55a6)
 
- Testiorjan testi oli myös positiivinen:

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/0b9d1893-4efc-40a7-9a05-f0eebfd4efd5)

- Geminiltä pyysin lähettämään jonkun shell-komennon ja se ehdotti seuraavaa:

```
sudo salt '*' cmd.run 'echo "Hei maailma!" >> /tmp/output.txt'
```

* Tämä siis tekee output.txt - tiedoston, johon tulostaa "Hei maailma!", jonka master-kone lähetti. Tarkistin sen menevän perille orjakoneelta menemällä tuodon tiedostoon komennolla

```
cat output.txt
```

ja huomasin, että se oli toiminut:

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/40db7f3f-f692-4e41-bc0a-915d710b466f)

## f) Aja useita idempotentteja (state.single) komentoja master-slave yhteyden yli (EDIT= Tehtävä d ja f meni sekaisin, joten tämä on F! "Tee infraa koodina, aja tila orjalle, voi tehdä esimerkkitiedoston hakemistoon. Testaa komento")

- Ilmeisesti tässä on hyvä noudattaa https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/ ohjeita ja tehdä sen mukaan
- Ollaan jälleen master t001 - koneella
-  Kokeilin tällä kertaa vähän ensin, miten tuo toimii ja raportoin vähän jäljessä.
-  Loin masterille kansion, johon kaikki ohjeet tulevat orjille

```
sudo mkdir - p /srv/salt/
```

- Tein ohjeen mukaisen sls-tiedoston "states"

```
/tmp/terveorja.txt:
  file.managed:
    - source: salt://terveorja.txt
```

- Muokkasin tekstiä terveorja.txt ja lisäsin sinne "Nähdään www.google.fi"

```
sudoedit /srv/salt/terveorja.txt
Nähdään www.google.fi
```

- Suoritin komennon, jonka uskoin lähtevän suoraan orjalle ja testasin samalla, miten se toimii

```
sudo salt '*' state.apply hello
```


![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/2e8b936b-4e0f-45a5-8c2d-e8a77fdf9853)

- Kävin katsomassa orjan /tmp - tiedostoja, johon tuon txt - tiedoston pitäisi tulla
- Siellähän se on!

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/79b0c330-7ccc-4ba1-aa05-39b911ef9fb1)

## e) Kerää teknistä tietoa orjista verkon yli (grains.item)

- Vilkaistaan nyt kernel ja  master

```
sudo salt '*' grains.item master kernel |less
```

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/9ebbedba-49f4-469c-a537-284bc286fdcd)


## d) Aja useita idempotentteja (state.single) komentoja master-slave yhteyden yli
- Aloitin (h1) tutusta komennolla

```
sudo salt '*' -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"
```

Joka luo tyhjän tiedoston. Tarkistin sen orjakoneelta ja siellähän se oli.

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/1fbc65d6-8904-4fef-bf96-a8e1e7c0f9f8)


- Aikaisemmin asensin Microeditorin orjalle testinä (raportin ulkopuolella) ja kokeilin pkg.installed sekä .removed- komentoja

```
sudo salt '*' -l info state.single pkg.installed tree

sudo salt '*' -l info state.single pkg.removed tree

```

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/f932104a-13ee-41eb-bb32-7ee33db9ba4c)



### Lähteet

* Gemini (Tekoäly)
* Kotitehtävät: https://terokarvinen.com/2024/configuration-management-2024-spring/ (luettu 09/04/2024)
* Soitto kotiin by Saku Laitinen: https://github.com/KebabGarva/Linux-palvelinten-hallinta-bgu248/blob/main/h2.md (luettu 09/04/2024)
* Salt States - I want My Computers Like This, Tero Karvinen (2018), https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/ (luettu 09/04/2024)
* Hello Salt Infra-as-Code, Tero Karvinen https://terokarvinen.com/2024/hello-salt-infra-as-code/ (luettu 09/04/2024)
* Two Machine Virtual Network With Debian 11 Bullseye and Vagrant, Tero Karvinen https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ (luettu 09/04/2024)
*  Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux, Tero Karvinen (2018) https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux (luettu 09/04/2024)
*  StackOverflow/Questions https://stackoverflow.com/questions/23482341/the-machine-with-the-name-c6401-was-not-found-configured-for-this-vagrant-env (luettu 09/04/2024)
* How to Create Multiple Virtual Machines Using Vagrant Tool, Surya Raj Ghimire (2023), https://medium.com/@srghimire061/how-to-create-multiple-virtual-machines-using-vagrant-tool-d4b074d5bdcc (luettu 09/04/2024)



