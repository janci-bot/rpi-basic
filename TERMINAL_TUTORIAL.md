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
