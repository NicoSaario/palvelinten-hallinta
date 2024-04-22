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
Ajattelin, että en tätä harjoitusta ole vielä kertaakaan kokeillut, testaan sitä nyt. Eli noudatan ohjeita https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file ja valmistelen automaattisesti yhden isännän ja sille kaksi orjaa.

- Koska viime kerrasta on hetki, aloitan komennoilla

```
vagrant status
vagrant up
vagrant ssh
```

- Halusin nähdä, mitä koneita siellä oli muistissa ja nähtävästi aikaisemmin käyttämäni oli vielä tallella
- Nyt on yhteys koneeseen
- Voidaan skipata tuo virtuaaliympäristön asennus, sillä käytän jo windowssilla Vagranttia ja micro on asennettuna

```
mkdir saltdemo; cd saltdemo
micro Vagrantfile
```

- Kopsataan tuosta Teron linkistä valmiiksi tehty Vagrantfile (kiitos Terolle!)
- Nyt on viimeaikoina ilmestynyt sellaisia ongelmia, että saan ilmoituksen Powershellin version puutteellisuudesta ja sitä tässä alan jälleen selvittämään.. Kaikki toimi siis hyvin, kunnes löin komennon vagrant exit ja vagrant up, jolloin tuli seuraava ilmoitus

![image](https://github.com/NicoSaario/palvelinten-hallinta/assets/156778628/7ab1978d-2804-4218-b0ad-fcc51dc719f3)

- komennolla which powershell, näyttää, että käyttäisin v1.0
- Joo nyt meni hermo, joten siirryn VirtualBoxin puolelle. Kokeilin asentaa muutaman kerran päivitykset ja uudelleen koko Shellin, mutta lähetään sit Linuxiin.
- Ei tätä ongelmaa ollut aikaisemmmin, selvitän sen myöhemmin
- Nyt on sitten VirtualBoxin, Debianin puolella ongelmaa. Ei anna syystä tai toisesta asentaa virtualboxia ja herjaa jatkuvasti Libvirt erroria sekä virtualboxin puuttumista, kun käytän vagrant up - komentoa
- Etsinyt netistä turhaan ratkaisua, tällä hetkellä päädyin tekemään Vagrantin asennustiedostolle repair - setupin ja katsotaan, korjaantuuko mikään. 15 - minuuttia myöhemmin se rullaa yhä. Yritin myös asennella manuaalisesti PowerShelliin uudet päivitykset, mutta se on jo up-to-date. Se toimii aina hetkellisesti tietokoneen käynnistyessä uudelleen.
- Jos ongelma ei ratkea pian, luon uuden virtuaalikoneen ja yritän sitä kautta Linuxissa asennella kaikki alusta lähtien uudelleen. Nyt joudun kuitenkin tehtävän jättämään tähän toistaiseksi. Taitaa olla jo kolmas päivä, kun vikoja selvitellään. Sainpahan ainakin tiivistelmän tehtyä.
