
# KNIFE #044 – Záloha OneDrive na QNAP cez rclone + rsync

> **Cieľ**: Zálohovať obsah osobného OneDrive účtu na lokálny disk (QNAP) bez závislosti na GUI aplikáciách a minimalizovať riziko straty dát.

---

## 🧭 1. Detekcia potreby

V rámci čistenia cloudových úložísk (OneDrive a Google Drive) kvôli plánovanému zrušeniu platených plánov bolo identifikované riziko straty dát. Zámerom je:

- Zálohovať všetky súbory z OneDrive na lokálny QNAP disk.
- Overiť a pochopiť obmedzenia (napr. streamované súbory, chýbajúce offline dáta).
- Vyhnúť sa zbytočným duplicitám pri opakovaných synchronizáciách.

---

## 🧠 2. Porozumenie a príprava

- **Streamované súbory** (OneDrive on-demand files) nie sú fyzicky uložené na disku. Na ich stiahnutie musí byť súbor "dostupný offline".
- Na prístup k osobnému OneDrive sa použije nástroj `rclone`, ktorý umožňuje autentifikovaný prístup cez Microsoft API.
- Lokálne kopírovanie zabezpečí `rsync` (alebo FreeFileSync).

---

## 🔍 3. Analýza riešení

Porovnávané prístupy:

| Prístup        | Výhody                       | Nevýhody                            |
|----------------|------------------------------|-------------------------------------|
| Rclone         | Prístup priamo na OneDrive   | Nevidí súbory, ktoré sú len "online" |
| Rsync z OneDrive priečinka | Kopíruje iba stiahnuté súbory | Vyžaduje mať všetky súbory offline |
| FreeFileSync   | GUI, intuitívne              | Žiadny progres v CLI móde           |

---

## 🧩 4. Návrh riešenia

- Použiť `rclone config` na vytvorenie vzdialeného profilu `sth-personal-onedrive-02`.
- Ak sú niektoré súbory len online, stiahnuť ich manuálne alebo spustiť OneDrive klienta a povoliť „Always keep on this device“.
- Na kopírovanie použiť `rclone copy` alebo `rsync` s `--progress` a `--stats`.

---

## 🛠️ 5. Implementácia

### 🏗️ Rclone – konfigurácia OneDrive

```bash
rclone config
# Nový remote -> názov: sth-personal-onedrive-02
# Typ: onedrive
# Autorizácia cez browser (prihlásiť sa do MS účtu)
```

### 🧪 Testovanie prístupu

```bash
rclone lsd sth-personal-onedrive-02:
```

### 🧱 Zálohovanie dát cez rclone

```bash
sudo rclone copy sth-personal-onedrive-02:/ "/Volumes/vol_Data18TB/OneDrive-STH-Personal" \
  --progress --stats 5s --exclude ".DS_Store"
```

> Ak narazíš na chybu `invalidResourceId` pri `Personal Vault`, môžeš ho ignorovať alebo vylúčiť cez `--exclude "Personal Vault/**"`

### 🧱 Alternatíva – rsync z lokálneho OneDrive priečinka

```bash
sudo rsync -avh --progress --stats --exclude=".DS_Store" \
  "/Users/romankazicka/Library/CloudStorage/OneDrive-Personal/" \
  "/Volumes/vol_Data18TB/OneDrive-STH-Personal/"
```

---

## 🧪 6. Overenie a testovanie

- **Prekontroluj štatistiky** z `--stats` výstupu.
- V prípade potreby si over cez `du -sh` veľkosť adresárov.
- Chyby ako `Permission denied` riešiť cez `sudo`.

---

## 🧠 7. Poučenie a odporúčania

- **Rclone** je výkonný nástroj pre prácu s cloudom, ale potrebuje správnu konfiguráciu.
- **Rsync** je spoľahlivý pre lokálne zálohy, vyžaduje však, aby boli dáta už fyzicky na disku.
- Budúce zálohy možno zautomatizovať cez `cron` alebo `launchd`.

---

## 📦 Prílohy a odkazy

- `rclone.conf` – konfigurácia vzdialeného OneDrive
- `rsync.log` – výpis zálohovania (voliteľné)
- Odkaz: https://rclone.org/onedrive/

---

