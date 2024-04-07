# Kotitehtävät


## Tiivistelmät
Tehtävänä tiivistää lyhyesti muutamalla ranskalaisella viivalla seuraavat linkit:
- https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/
- https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux
- https://terokarvinen.com/2024/hello-salt-infra-as-code/

* Two machine virtual network
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

* Salt Quickstart
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


