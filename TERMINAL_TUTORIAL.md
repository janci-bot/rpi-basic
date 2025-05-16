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

## 2. Práca so súbormi a adresármi

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

## 3. Inštalácia a správa balíčkov

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

## 4. Monitorovanie systému

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

## 5. Vytvorenie systemd služby na automatické spustenie skriptu

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

## 6. Automatické pripojenie k WiFi

- Skontroluj dostupné siete:

```bash
nmcli device wifi list
```

- Pripoj sa k sieti:

```bash
nmcli device wifi connect "nazov_siete" password "tvoje_heslo"
```

- Zisti stav pripojenia:

```bash
nmcli connection show --active
```

- Nastavenia WiFi sa ukladajú v adresári `/etc/NetworkManager/system-connections/` ako `.nmconnection` súbory.
