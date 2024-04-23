# Muistio Palvelinten Hallinta - kurssilta
- Pitäjänä ja kurssin pohjana toimii https://terokarvinen.com/2024/configuration-management-2024-spring/

## Micro

- ctrl - d duplicate
- asetuksia
- ctr-e
- set ftoptions off
- set tabstospaces false
- set autoclose false
- set colorscheme simple

- komentoja
- retab


- micro -plugin install cheat f1 tai ctrl-e cheat
- micro -plugin install palettero
- micro -plugin install runit
- foo.py ja ctrl-e runit makeup makeupgb
- palettero

# Varastorakennus (Konffataan One workstation with Salt)

Eli siis GitHub - repo ja sinne tavaraa sisään
```
sudo salt-call --local
```

```
git clone ssh@
```

- Tehdään Hello Woorld

```
mkdir srv/salt -p
cd srv
cd salt
mkdir hello
nano init.sls
sudo salt-call --local state.apply hello
```

```
sudo salt-call --local state.apply heippa --file-root=srv/salt
```

Sinne sisään

/tmp/saltharjoitus:
  file.managed

- Nähdään hakemistopuu
  
```
tree
```
