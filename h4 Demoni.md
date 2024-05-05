# Kotitehtävät

Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 23/04/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 23/04/2024

Powershell + Vagrant

Tässä tehtävässä myös VirtualBox + Debian 11

## x) Tiivistelmät - Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

### 1) Ensimmäisenä https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file
- Kohdat
* Infra as Code - Your wishes as a text file
- Sisältö kohteeseen init.sls
- Syntaxi on YAML
- Sisennyksellä ON väliä
- Kahdella välilyönnillä!!
- EI tabilla

```
sudo mkdir -p /srv/salt/hello
sudoedit /srv/salt/hello/init.sls
```

```
$ cat /srv/salt/hello/init.sls
/tmp/infra-as-code:
  file.managed

$ sudo salt '*' state.apply hello
```

* top.sls - What Slave Runs What States
- top Määrittelee, mitkä tilat suoritetaan millekin orjalle

```
$ sudo salt '*' state.apply hello^C
$ sudoedit /srv/salt/top.sls
$ cat /srv/salt/top.sls
base:
  '*':
    - hello
```

- Koska kohta base: '*', ei tarvitse määrittää mitään moduuleja state.applyyn, pelkkä '*' riittää.

```
$ sudo salt '*' state.apply
```

### 2) Salt contributors: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml ja siitä kohdat:
* Rules of YAML
- YAMLin tehtävänä on kääntää tietorakenne Python-dataksi Salttia varten

Basic rules:
- Tiedot jäsennelty pareittain, esim. key: value
- Avain:- avroparejen merkitseminen (":")
- Kaikissa kirjainkoolla on merkitystä
- TABIn käyttö ei sallittu! VAIN välilyöntejä
- Kommentit hästägillä #


* YAML simple structure
YAML koostuu kolmesta peruselementtityypistä:

1) Scalars - key: value  kartoitukset, joissa arvona numero, merkkijono tai totuusarvo. Esim:

```
# key: value

vegetables: peas
fruit: apples
grains: bread
```

2) Lists - avain key: , jota seuraa lista arvoja "value", jossa jokainen arvo on eri rivillä ja kahden välilyönnin sisennyksillä ja tavuviivalla. Esim:

```
# sequence_key:
#  - value1
#  - value2

vegetables:
   - peas
   - carrots
fruits:
   - apples
   - oranges
```

3) Dictionary - Kokoelma key: value määrityksiä ja luetteloita, esim:

```
dinner:
  appetizer: shrimp cocktail
  drink: sparkling water
  entree:
    - steak
    - mashed potatoes
    - dinner roll
  dessert:
    - chocolate cake
```


* List and dictionaries - YAML block structures

- Jäsennetty lohkorakenteisiin
- TÄYTYY SISENTÄÄ yhdellä tai enemmän _välilyönnillä_, kaksi _välilyöntiä_ on se vakio
- Kokoelma lististä tai dictionarysta ilmaisee merkintä, jossa on yhdysviiva ja välilyönti ("- ")

3) ### Karvinen 2018: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh
- Oman järjestelmän asetustieto pohjana. Artikkelissa on jonkun toisen Lunix-version tiedosto
- Voi hallita valtavaa määriä demoneja konfiguraation hallintajärjestelmällä.
- Package-file-service: asenna ohjelmisto, korvaa asetustiedosto, potki demonia käyttääksesi uutta kokoonpanoa
- Artikkelissa on yksinkertainen Salt-tila SSH-palvelinportin vaihtamiseksi
- Ensin salt master-slave arkkitehtuuri
- Masterille sshd.sls ja konffitiedoston pääkopio sshd_config

SSH Staten luominen:

```
$ cat /srv/salt/sshd.sls
openssh-server:
 pkg.installed
/etc/ssh/sshd_config:
 file.managed:
   - source: salt://sshd_config
sshd:
 service.running:
   - watch:
     - file: /etc/ssh/sshd_config
```

- Tämä on lähes sama, kuin defaultti sshd_config -tiedosto openssh-serverin asentamisen jälkeen. Ainoastaan kommentit poistettu ja porttinumero laitettu "Port 8888"

```
$ cat /srv/salt/sshd_config
# DON'T EDIT - managed file, changes will be overwritten
Port 8888
Protocol 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
UsePrivilegeSeparation yes
KeyRegenerationInterval 3600
ServerKeyBits 1024
SyslogFacility AUTH
LogLevel INFO
LoginGraceTime 120
PermitRootLogin prohibit-password
StrictModes yes
RSAAuthentication yes
PubkeyAuthentication yes
IgnoreRhosts yes
RhostsRSAAuthentication no
HostbasedAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
X11Forwarding yes
X11DisplayOffset 10
PrintMotd no
PrintLastLog yes
TCPKeepAlive yes
AcceptEnv LANG LC_*
Subsystem sftp /usr/lib/openssh/sftp-server
UsePAM yes
```

- Lisätään kaikille orjille

```
sudo salt '*' state.apply sshd
```

- Testataan
* Käytetään jotain orjaa kohteena sen sijaan, eli korvataan seuraavassa kohtaa tuo 'tero.exmaple.com'

```
$ nc -vz tero.example.com 8888
```

- Tai

```
$ ssh -p 8888 tero@tero.example.com
tero@tero.example.com's password:
```

- Jos SSH demoni vastaa portista 8888, homma rullaa ja kaikki toimii.

## Alkutoimet
- Koska viime kerrasta on hetki, aloitan komennoilla

```
vagrant status
vagrant up
vagrant ssh
```

- Nyt on viimeaikoina ilmestynyt sellaisia ongelmia, että saan ilmoituksen Powershellin version puutteellisuudesta ja sitä tässä alan jälleen selvittämään.. Kaikki toimi siis hyvin, kunnes löin komennon vagrant exit ja vagrant up, jolloin tuli seuraava ilmoitus

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/7ab1978d-2804-4218-b0ad-fcc51dc719f3)

- komennolla which powershell, näyttää, että käyttäisin v1.0
- Joo nyt meni hermo, joten siirryn VirtualBoxin puolelle. Kokeilin asentaa muutaman kerran päivitykset ja uudelleen koko Shellin, mutta lähetään sit Linuxiin.
- Ei tätä ongelmaa ollut aikaisemmmin, selvitän sen myöhemmin
- Nyt on sitten VirtualBoxin, Debianin puolella ongelmaa. Ei anna syystä tai toisesta asentaa virtualboxia ja herjaa jatkuvasti Libvirt erroria sekä virtualboxin puuttumista, kun käytän vagrant up - komentoa
- Etsinyt netistä turhaan ratkaisua, tällä hetkellä päädyin tekemään Vagrantin asennustiedostolle repair - setupin ja katsotaan, korjaantuuko mikään. 15 - minuuttia myöhemmin se rullaa yhä. Yritin myös asennella manuaalisesti PowerShelliin uudet päivitykset, mutta se on jo up-to-date. Se toimii aina hetkellisesti tietokoneen käynnistyessä uudelleen.
- Jos ongelma ei ratkea pian, luon uuden virtuaalikoneen ja yritän sitä kautta Linuxissa asennella kaikki alusta lähtien uudelleen. Nyt joudun kuitenkin tehtävän jättämään tähän toistaiseksi. Taitaa olla jo kolmas päivä, kun vikoja selvitellään. Sainpahan ainakin tiivistelmän tehtyä.


- Update: 23/04 klo 09.07
- Tunnilla pohdiskelin muistiota tehtäessä, että olisikohan ongelma pelkästään Vagrantissa.
- Päädyin poistamaan sen, testaamaan PowerShelliä ja kappas. Se kaikista _yksinkertaisin_ ratkaisu oli se, mikä toimi. PowerShell ei siis enää herjannut mitään käynnistyessä, vaan käynnistyi normaalisti ja katsotaan tilanne Vagrantin asennuksen jälkeen.
- Pääsen siis lopulta tekemään myöhemmin puuttuvat tehtävät.
- Jälleen UPDATE: Testi toimi vain hetken ja kaatoi jälleen koko PowerShellin
- Kokeiltu on vaikka ja mitä, mutta raportoin tästä eteenpäin:

- Kokeilin komentoja

```
vagrant plugin update
```
- Päivittää vagrantin plug-init

```
vagrant plugin expunge --reinstall
```

- Poistaa pluginit ja yrittää asentaa niitä uudelleen

- Update 5/5/2024
- Noin 400 - foorumia läpikäyneenä totesin, että fresh startti Windowssille on luultavasti ainoa vaihtoehto. Backupit ja koko kone tyhjäksi.

  <img width="577" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/a3806760-625f-4db7-a92f-054c9e2f10be">

```
sudo apt-get update
sudo apt-get install micro
sudo apt-get install salt-master
```

- Alkutoimet microlle, päivityksille ja salt-masterille

## a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

- Noudatan aika Raamatullisesti https://terokarvinen.com/2024/hello-salt-infra-as-code/ ohjeita, sillä joutuu hieman palauttamaan mieleen tuon Windows - sekoilun jälkeen, miten kaikki taas toimii
- Micro oli jo asennettu, joten Create folder - kohtaan
- Luodaan uusi moduuli "hello", jossa "hello/" - kansioon tulee kaikki moduuliin liittyvät asiat - Salt - koodi ja kaikki tiedostot tai templatet.

```
sudo mkdir -p /srv/salt/hello
```
- Loin ohjeesta poiketen hello ilman '/'. Joka tapauksessa se näkyi tiedostossa, mutta poistan varmuuden vuoksi, jotta nämäkin palautuu mieleen paremmin

```
sudo rmdir -p /srv/salt/hello
```

```
sudo mkdir -p /srv/salt/hello/
```

- Sit navigoidaan luotuun kansioon

```
cd /srv/salt/hello/
```

- Tehdään sinne sisään idempotenttia koodia Saltilla

```
sudoedit init.sls
```


```
/tmp/hellonico:
file.managed
```

- Ajetaan se
- Kuvasta näkyy, että tiedosto "/tmp/hellonico created" ja Succeeded 1 (changed=1), joten se toimii ja teki, mitä pyydettiin.

<img width="573" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/d9157787-3a20-4017-85f4-7ee5861487f6">


- Tarkistetaan vielä, että se todellakin on ilmestynyt sinne. Samalla vielä komentohistoria.

```
ls /tmp/hellonico
```

<img width="249" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/dbf00705-3644-4784-bdbf-b33d86916f45">
 
<img width="272" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/949ce54c-97a2-428c-bc12-98fe2ec9b4b9">


## b) Top. Tee top.sls niin, että useita valitsemiasi tiloja ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply" tai "sudo salt-call --local state.apply".

Tehdään muutama eri top.sls - tiedosto

```
cd /srv/salt/
sudoedit top.sls
sudo salt '*' state.apply top.sls
```

- Palauttaa ajosta:
<img width="328" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/e085d13e-cc91-4209-9079-9d2ca8b2a318">

- Tehdään siis toimiva master - minion - kombinaatio

```
sudo apt-get -y install salt-master
hostname -I
```

<img width="158" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/47bcb490-2efc-4c53-b5b2-47ac3c5bebee">

```
sudoedit /etc/salt/minion

master: 10.0.2.15
id: niso

```

- Pikku päivitys tähän väliin. Raportti on raportti, eli kaikki vaiheet tulee näkyviin.
- Mutkia taas matkassa ja ihmettelin hetken, mitä ees oon tekemässä
- Tajusin sit samalla, etten ole laittanu edes sitä VagrantFilen konffia sisään ja saisin sitä kautta suoraan homman rullaamaan
- Joten päädyin tuohoamaan koko edellisen koneen ja tekemään sen
- Vaiheet:

```
exit #takas OS
destroy vagrant
mkdir twohost; cd twohost/
vagrant init
start notepad++ ./Vagrantfile/ #koska Windows
Teron konffi sisään - krediitit sinne jälleen!
```

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/

```
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2019-2021 Tero Karvinen http://TeroKarvinen.com

$tscript = <<TSCRIPT
set -o verbose
apt-get update
apt-get -y install tree
echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"
TSCRIPT

Vagrant.configure("2") do |config|
	config.vm.synced_folder ".", "/vagrant", disabled: true
	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	config.vm.provision "shell", inline: $tscript
	config.vm.box = "debian/bullseye64"

	config.vm.define "t001" do |t001|
		t001.vm.hostname = "t001"
		t001.vm.network "private_network", ip: "192.168.88.101"
	end

	config.vm.define "t002", primary: true do |t002|
		t002.vm.hostname = "t002"
		t002.vm.network "private_network", ip: "192.168.88.102"
	end
	
end

```

- Näinkin, että Tero oli jo päivitellyt sinne kolmen koneen konffit, mutta nyt mennään tällä
- Nyt tulee toi sama rimpsu noista Salt Quickstartista, jonka tein jo aiemmin - turhaan
- Ohje noudattaa jälleen hyvin Raamatullisesti https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Hyppäsin t001 - koneelle

```
vagrant ssh t001
sudo apt-get update
sudo apt-get -y install salt-master
hostname -I
```

- Palauttaa arvot 10.0.2.15 ja 192.168.12.100, jotka käsittelin aikaisemmassa repossa jo. Repo löytyy tuosta https://github.com/NicoSaario/palvelinten-hallinta/blob/main/h2%20Soitto%20kotiin.md
- Sit orjakoneelle eli t002

```
vagrant ssh t001
sudo apt-get update
sudo apt-get -y install salt-minion
sudoedit /etc/salt/minion
```

- Sisään masterin t001 iippari ja id:t002

```
sudo systemctl restart salt-minion.service
```

t002:

```
sudo salt-key -A
```

Ja hyväksytään sieltä

<img width="565" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/fb8e5338-5803-41a8-9d4b-f9eb14cd7c33">

Sitten top.sls - tiedoston kimppuun vihdosta viimein.

Luodaan tiedosto top.sls ja laitetaan sinne muutama rimpsu sisään:

```
sudoedit top.sls
```

<img width="282" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/9da84f58-9d19-489d-899d-cf426373b45c">

- Loin kansiot hello, relax, testi

```
sudo mkdir relax hello testi
```

Luon relax - kansioon init.sls - tiedoston ja sen sisään

<img width="377" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/b546f99d-6e6d-45ad-8bec-1ce77aac9523">



Luon myös testi - kansioon seuraavanlaisen tiedoston:

<img width="383" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/13804f65-c35c-4dfd-a1fe-9714e6b33c16">

Seuraavaksi käytän komentoa ja kokeilen, toimivatko tehdyt muutokset:

```
sudo salt '*' state.apply
```


<img width="476" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/4af95b8e-f7b1-4fdc-a605-8ce81ca8704c">

<img width="497" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/652d66dc-b621-4e29-b899-6e23cb0ed8ef">

Testasin samalla orjakoneella t002, että micron asennus onnistui ja menivätkö ne tosiaan läpi. Tein vain komennon ```micro index.html```

<img width="469" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/37adb22d-d619-4e56-823e-c92fd888dde2">




Eli kaikki muut muutokset menivät läpi - micron asennus, hellonico ja hellotestin luomiset, mutta apachen asennus ei tällä kertaa mennyt läpi. Hyvä sinänsä, sillä nähtävästi keulin hieman ja sen asennus tulee seuraavassa tehtävässä. Saan jatkuvasti virheilmoituksen tuosta t001 - minionin "did not returnista", tämä johtuu todennäköisesti siitä, että aluksi laitoin väärän ID:n tiedostoon, jolloin konffattiin minionin ID. Vaihdoin sen myöhemmin, jolloin master vastaanotti 2 eri avainta, jotka molemmat olivat samalta orjalta.


##  c) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy.
Ensin käsin, vasta sitten automaattisesti.

Tämän onneksi pitäisi olla jo hyvin muistissa kurssilta Linux - Palvelimet ja voin hyödyntää myös aikaisempia repoja

```
sudo apt-get update
sudo apt-get install apache2
sudo apt-get install curl
```

defaultti pois päältä
navigoidaan apache2-kansioon ulkomuistista

```
cd /etc/apache2/sites-enabled
sudo a2dissite 000-default.conf
```

Apachen defaulttisivu on nyt näkyvissä

<img width="473" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/7d623013-7641-4211-b24e-64ce1a91dd15">

Sit mennään tekeen sivua - lainailen vähän aikaisempaa ohjetta täältä https://github.com/NicoSaario/Linux_Palvelimet/blob/main/H5%20Koko%20juttu.md

- Eli teen kansion webbisaiteille, editoin konffitiedoston osoittamaan oikeisiin polkuihin

```
sudo mkdir -p /etc/publicsites/saltti.example.com/
EDITOR=micro sudoedit saltti.example.com.conf
sudo a2ensite saltti.example.com.conf
sudo systemctl restart apache2
curl localhost
```

<img width="367" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/9fc48eaa-7d52-4dbf-8b1b-fdaff7e53347">

Testisivu on nyt korvattu, pitää vain laittaa jotain tekstiä sisään ja katsoa, mitä tapahtuu.
Navigoidaan ja tehdään index.html

```
micro index.html
curl localhost
```

<img width="398" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/15527e73-3616-4a60-b249-127222309157">

Ja homma toimii!! Kiva huomata, että jotain on vielä muistissa ja meni kyllä todella nopeasti käsin asennus.

- Nyt yritetään automatisoida se
- Tein Teron tunnilla pienet muistiinpanot ja yritän sen perusteella ratkaista tämän

```
sudo mkdir -p /srv/salt/apache
cd /srv/salt/apache
EDITOR=micro sudoedit init.sls
```

Seuraavat tekstit sinne sisään:

```
  apache2:
   pkg.installed
 
apache2/sites-available/...
   file.managed:
   -source: "salt://publicsites/saltti.conf"

 /etc/apache2/sites-enabled/saltti.conf:
   file.symlink:
    - target: "/etc/apache2/sites-available/saltti.conf"
/etc/apache2/sites-enabled/000-default.conf:
 file.absent

apache2.service
 service.running:
- watch:
     - file: /etc/apache2/sites-available/saltti.conf
    - file: /etc/apache2/sites-enabled/000-default.conf
     - file: /etc/apache2/sites-enabled/saltti.conf

```

<img width="488" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/534d0b2e-869d-459a-8a8d-de58585555b1">


```
sudo salt-call --local state.apply
```

<img width="383" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/b29ac01c-b1a5-4f1d-be66-ec72df2b5ce6">

Muistio:

```
sudo salt-key -A #avainten haku masterilla
``` 

```
sudo salt-key -L #Näkee kaikki avaimet
```

```
sudo salt-key -d #ja perään minionin/avaimen id
```

- Tuli siis virheilmoitus, ettei minionin avainta hyväksytä ja lähdin selvittämään sen uudelleen laittoa
- Käytän ylläolevia komentoja ja sain sen toimimaan
- Sain tällaisen ilmoituksen enkä kyllä rehellisesti tiedä, miten edetä

<img width="338" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/42211732-e19b-45e4-a305-5c9523eec7b0">


## d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.
- Teen tämän suoraan Teron linkistä https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh
- Luon siis tiedostot sshd.sls ja sshd_config ohjeiden mukaan
- sshd.sls tulee seuraavat sisään:

```
openssh-server:
 pkg.installed
/etc/ssh/sshd_config:
 file.managed:
   - source: salt://sshd_config
sshd:
 service.running:
   - watch:
     - file: /etc/ssh/sshd_config
```

ja sshd_config sisään, nyt jätetään vielä portti 22 auki ja lisätään se tuohon ihan alkuun: 'Jos käytät Vagrantia, muista jättää portti 22/tcp auki - se on oma yhteytesi koneeseen. SSHd:n asetustiedostoon voi tehdä yksinkertaisesti kaksi "Port" riviä, molemmat portit avataan.'- Tero

```
# DON'T EDIT - managed file, changes will be overwritten
#Port 22
Port 8888
Protocol 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
UsePrivilegeSeparation yes
KeyRegenerationInterval 3600
ServerKeyBits 1024
SyslogFacility AUTH
LogLevel INFO
LoginGraceTime 120
PermitRootLogin prohibit-password
StrictModes yes
RSAAuthentication yes
PubkeyAuthentication yes
IgnoreRhosts yes
RhostsRSAAuthentication no
HostbasedAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
X11Forwarding yes
X11DisplayOffset 10
PrintMotd no
PrintLastLog yes
TCPKeepAlive yes
AcceptEnv LANG LC_*
Subsystem sftp /usr/lib/openssh/sftp-server
UsePAM yes
```

Näiden jälkeen 

```
sudo salt '*' state.apply sshd
```

<img width="354" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/323edfcb-fa9f-4146-a0df-92a0dc259b19">

<img width="389" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/07df5c27-da3e-4c75-8d66-43f9c18466fd">

- Testataan vielä ohjeen mukaan

```
nc -vc t002 8888
```

Joo ei toimi, heittää virheen "t002: forward host lookup failed: Unknown host"

- Mennyt jossain kohtaa jo, mistä taisinkin mainita nuo avaimet vähän sekaisin. Sit muuttelin t001 id ja vahingossa muuttelin masterin konffitiedostoa, joten siinä taitaa piillä seuraavan virheilmoituksen syy:

<img width="476" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/6dbf0ddf-d488-4b4b-8e75-25468dfc109e">

- Teen nyt niin, että poistelen nuo vanhat avaimet, käyn /etc/salt/minion - konffissa ja teen sinne nyt t005, jotta kaikki ei oo ihan hujan hajan. Katsotaan, muuttuuko mikään. Komentoina aikaisemmat "muistio"-kohdasta:

<img width="386" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/10a3e50e-2d19-49b0-bfb5-049292ec0502">

Erroria jälleen

<img width="469" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/e63a2e5b-36b7-486e-93d7-45963fb6a99b">

- Noniiin! Muutin komentoa muotoon:

```
sudo salt-call --local state.apply sshd
```

<img width="332" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/62660fd5-ed3a-4add-b655-4889d0692cab">

<img width="446" alt="image" src="https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/e1bf4a3e-ad03-4bce-a121-ad8f3051bcc9">

- Mutta eipä vieläkään tuo testaus toimi


### Lähteet: 

- Salt contributors: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml (luettu 23/04/2024)
- Tero Karvinen: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file (luettu 23/04/2024)
- Karvinen 2018: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh (luettu 06/05/2024)
- https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ (luettu 06/05/2024)
