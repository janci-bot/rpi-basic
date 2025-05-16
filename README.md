# RPI BASIC – Praktický kurz práce s Raspberry Pi cez SSH

Tento kurz je určený pre používateľov Raspberry Pi, ktorí pracujú výhradne cez SSH (bez grafického prostredia). Zameriava sa na reálne úlohy, automatizáciu, skriptovanie, GPIO a senzory.

---

## 🎯 Úroveň 1: Základy systému (Linux, SSH, súbory)

### ✅ Cieľ:
- Ovládnuť bežné terminálové operácie a správu systému

### 🔧 Úloha:
- [ ] Pripojenie cez SSH, zmena hesla, vytvorenie vlastného používateľa  
- [ ] Práca so súbormi: `ls`, `cp`, `mv`, `rm`, `nano`, `cat`, `chmod`, `chown`
- [ ] Inštalácia balíčkov: `apt`, `apt update`, `apt install`, `dpkg -l`
- [ ] Monitorovanie systému: `htop`, `df -h`, `free -m`, `uptime`, `top`, `journalctl`
- [ ] Vytvorenie `systemd` jednotky pre automatické spustenie skriptu pri štarte
- [ ] Automatické pripojenie k WiFi (kontrola cez `nmcli` alebo `.nmconnection`)

---

## 🎯 Úroveň 2: Skriptovanie (bash, cron)

### ✅ Cieľ:
- Automatizovať opakujúce sa úlohy

### 🔧 Úloha:
- [ ] Bash skript na zálohovanie adresára do `.tar.gz`
- [ ] Cron úloha na spustenie skriptu každý deň o 23:00
- [ ] Kontrola využitia disku + email/log pri prekročení 80 %
- [ ] Automatický update balíčkov + logovanie výsledku

---

## 🎯 Úroveň 3: GPIO (cez Python)

### ✅ Cieľ:
- Naučiť sa ovládať fyzický svet pomocou GPIO

### 🔧 Úloha:
- [ ] Rozsvietenie LED (Python + `gpiozero` alebo `RPi.GPIO`)
- [ ] LED + tlačidlo
- [ ] LED blikajúce podľa času
- [ ] Logovanie stlačenia tlačidla

---

## 🎯 Úroveň 4: Senzory (I2C, SPI, UART)

### ✅ Cieľ:
- Získať dáta zo senzorov (napr. BME280, MPU6050, GPS)

### 🔧 Úloha:
- [ ] Aktivácia I2C a SPI (`raspi-config`, `lsmod`, `i2cdetect`)
- [ ] BME280 – teplota, vlhkosť, tlak
- [ ] MPU6050 – akcelerácia, gyroskop
- [ ] GPS cez UART
- [ ] Logovanie dát do CSV

---

## 🎯 Úroveň 5: Web a API

### ✅ Cieľ:
- Zverejniť dáta zo senzorov cez web alebo API

### 🔧 Úloha:
- [ ] `Flask` web server pre aktuálne dáta
- [ ] REST API
- [ ] Web so zobrazením posledných 10 logov
- [ ] Automatické spustenie servera cez `systemd`

---

## 🎯 Úroveň 6: Pokročilé

### ✅ Cieľ:
- Prepojiť Pi s inými systémami, zabezpečiť spoľahlivosť

### 🔧 Úloha:
- [ ] MQTT klient (odosielanie a príjem)
- [ ] Prístup k súborom cez Samba alebo SFTP
- [ ] Monitorovanie pomocou Prometheus/Grafana
- [ ] Záloha a obnova (napr. `dd`, `rpi-clone`)

---

📝 Pokojne pridaj svoje vlastné poznámky, skripty alebo linky. Tento kurz je čisto CLI (bez desktopu) a je vhodný aj pre serverové nasadenie alebo automatizáciu.
