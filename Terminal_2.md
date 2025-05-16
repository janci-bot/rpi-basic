# Terminálové operácie a správa systému na Raspberry Pi cez SSH

---

## 1. Pripojenie cez SSH a základné nastavenia

```bash
# Pripojenie k Raspberry Pi (z iného PC)
ssh pi@IP_ADRESA_PI

# Zmena hesla (ak ešte nebolo zmenené)
passwd

# Vytvorenie nového používateľa
sudo adduser meno_pouzivatela

# Pridanie používateľa do skupiny sudo (administrátorských práv)
sudo usermod -aG sudo meno_pouzivatela
```

---

## 2. Monitorovanie systému cez skripty

### 🔧 Skript 1: Využitie CPU, RAM a diskového priestoru

```bash
#!/bin/bash

# Súbor: monitor.sh

echo "=== MONITORING SYSTÉMU ==="

# CPU load
echo -e "\n--- CPU LOAD ---"
uptime

# RAM
echo -e "\n--- RAM USAGE ---"
free -h

# Disk
echo -e "\n--- DISK USAGE ---"
df -h /

# Top 5 procesov podľa využitia CPU
echo -e "\n--- TOP 5 CPU PROCESY ---"
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -n 6
```

---

### 🔧 Skript 2: Kontrola bežiacich služieb a reštart

```bash
#!/bin/bash

# Súbor: service_check.sh

# Zoznam služieb na sledovanie
services=("ssh" "cron")

for service in "${services[@]}"
do
    systemctl is-active --quiet "$service"
    if [ $? -ne 0 ]; then
        echo "$(date): Služba $service je nefunkčná. Reštartujem..."
        systemctl restart "$service"
        echo "$(date): $service bola reštartovaná." >> /var/log/service_monitor.log
    else
        echo "$(date): $service beží v poriadku."
    fi
done
```

---

### 🔔 Skript 3: Notifikácia pri nefunkčnosti služby

#### Súbor: notify.sh

#### a) Lokálna správa cez `wall`

```bash
wall "Pozor: Služba XYZ spadla!"
```

#### b) E-mail (ak je nakonfigurovaný `mail` alebo `msmtp`)

```bash
echo "Služba XYZ nefunguje!" | mail -s "Upozornenie z RPi" tvoja@adresa.com
```

---

### 💡 Automatické spúšťanie cez cron

Spustenie kontrolného skriptu každých 5 minút:

```bash
crontab -e
```

Pridaj riadok:

```cron
*/5 * * * * /cesta/k/service_check.sh
```

---

## 3. Práca so súbormi a adresármi

```bash
# Výpis obsahu adresára (podrobnejšie)
ls -la

# Kopírovanie súboru
cp zdrojovy_subor cielovy_subor

# Presun/ premenovanie súboru alebo adresára
mv zdroj ciel

# Odstránenie súboru
rm subor.txt

# Odstránenie priečinka rekurzívne
rm -r priecinok

# Editácia súboru cez nano editor
nano subor.txt

# Výpis obsahu súboru
cat subor.txt

# Zmena práv súboru (príklad: pridanie spustiteľnosti)
chmod +x skript.sh

# Zmena vlastníka súboru
sudo chown novy_majitel:novaskupina subor.txt
```

---

## 4. Inštalácia a správa balíčkov

```bash
# Aktualizácia zoznamu balíčkov
sudo apt update

# Inštalácia balíčka (napr. htop)
sudo apt install htop

# Zobrazenie nainštalovaných balíčkov
dpkg -l | grep balicek

# Odstránenie balíčka
sudo apt remove balicek
```

---

## 5. Monitorovanie systému (príkazy)

```bash
# Interaktívny monitor systémových procesov
htop

# Stav využitia diskov
df -h

# Stav využitia pamäte
free -m

# Ako dlho beží systém, záťaž CPU
uptime

# Zoznam procesov (podobné top, ale jednoduchšie)
top

# Zobrazenie systémových logov (použitie Ctrl+C na ukončenie)
journalctl -f
```

---

## 6. Vytvorenie systemd služby na automatické spustenie skriptu

1. Vytvor skript (napr. `/home/pi/myscript.sh`) a nastav mu spustiteľné práva:

```bash
nano /home/pi/myscript.sh
# ...napíš svoj skript...
chmod +x /home/pi/myscript.sh
```

2. Vytvor systemd jednotku:

```bash
sudo nano /etc/systemd/system/myscript.service
```

Obsah `myscript.service`:

```ini
[Unit]
Description=Moj vlastny startovac skriptu
After=network.target

[Service]
ExecStart=/home/pi/myscript.sh
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
```

3. Aktivuj a spusti službu:

```bash
sudo systemctl daemon-reload
sudo systemctl enable myscript.service
sudo systemctl start myscript.service
sudo systemctl status myscript.service
```

---

## 7. Automatické pripojenie k WiFi

* Skontroluj dostupné siete:

```bash
nmcli device wifi list
```

* Pripoj sa k sieti:

```bash
nmcli device wifi connect "nazov_siete" password "tvoje_heslo"
```

* Zisti stav pripojenia:

```bash
nmcli connection show --active
```

* Nastavenia WiFi sa ukladajú v adresári `/etc/NetworkManager/system-connections/` ako `.nmconnection` súbory.
