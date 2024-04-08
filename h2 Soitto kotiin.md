# Kotitehtävät


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

* Ongelma näyttää siis ratkenneen ainakin osittain. Kuvittelin ongelman olevan jostain syystä tavallisesssa notepadissa, jota käytin aikaisemmin.. Nappasin siis aikaisemminkin käyttäneeni Notepad++ asennuksen nopealla surffauksella ja sain varmuuden omillle harhaluuloilleni. Uskoisin kuitenkin, että ongelma oli ensinnäkin siinä, ettei PowerShell ollut auki adminioikeuksilla sekä se, että jokin tiedoston polussa/tiedostonimessä oli harhautunut. Muutaman lähteen mukaan Vagrantin default - nimisen koneen nimi pitäisi muuttaa/muuttua, mutta mielestäni ajetun konffin olisi pitänyt se tehdä(?); näin ei kuitenkaan ollut.

* Toinen huomio oli tuo vagrant init - komento suoraan luotuun kansioon. Unohdin sen tehdä kokonaan ja kuvittelin, että pelkkä "start notepad++ Vagrantfile" olisi luonut tarvittavan tiedoston, mutta nyt kun tutkin sitä tarkemmin, twohost - polusta puuttui aikaisemmin .vagrant sekä shared - kansiot. Ongelma taisi piillä myös siinä.

* 


