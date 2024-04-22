# Kotitehtävät

Kotitehtävät on tehty Palvelinten hallinta - kurssille ja löytyvät osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/


## x) Tiivistelmät - Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

1) Ensimmäisenä https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file
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

2) Salt contributors: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml ja siitä kohdat:
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



* List and dictionaries - YAML block structures
* 

