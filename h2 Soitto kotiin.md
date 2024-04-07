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


