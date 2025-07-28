# K044 – Záloha OneDrive pomocou `rclone` (Mac)

Tento návod pokrýva kompletný postup, ako zálohovať OneDrive na lokálny disk (napr. QNAP) pomocou nástroja `rclone` na macOS.

---

## 🧰 1. Inštalácia `rclone` (macOS)

### A. Pomocou Homebrew:
```bash
brew install rclone
```

### B. Overenie inštalácie:
```bash
rclone version
```

---

## 🔑 2. Nastavenie autorizácie (prvý prístup k OneDrive)

### A. Spusti `rclone config` a vytvor nový remote:
```bash
rclone config
```

Zvoľ `n` pre nový remote a postupuj podľa otázok:

- **name**: `onedrive`
- **storage**: `19` (OneDrive)
- Otvorí sa prehliadač, prihlás sa do Microsoft účtu
- Po úspešnej autorizácii sa vráť do terminálu a dokonči konfiguráciu

---

## 💾 3. Testovanie pripojenia

```bash
rclone ls onedrive:
```

Mal by sa zobraziť zoznam súborov a priečinkov z OneDrive.

---

## 🗂 4. Zálohovanie na lokálny disk

### A. Príkaz pre plné zrkadlenie OneDrive na disk:

```bash
rclone sync onedrive: /Volumes/NAS/backup/OneDrive --progress
```

- `onedrive:` – vzdialený OneDrive disk
- `/Volumes/NAS/...` – mountovaný priečinok QNAP
- `--progress` – priebežné zobrazenie stavu

---

## 🔁 5. Automatizácia

### A. Crontab (spúšťanie denne):
```bash
crontab -e
```

Pridaj napr.:

```
0 3 * * * /opt/homebrew/bin/rclone sync onedrive: /Volumes/NAS/backup/OneDrive --log-file=/Users/meno/logs/rclone.log
```

---

## 📬 6. Stav, kontrola, logy

- Priebežne:
```bash
tail -f ~/logs/rclone.log
```

- Overenie úspešného zálohovania:
```bash
rclone check onedrive: /Volumes/NAS/backup/OneDrive
```

---

## ✅ 7. Bonusy

- Môžeš použiť aj:
  - `rclone mount` (namapuje OneDrive ako disk)
  - `rclone serve webdav` (pre iné aplikácie)
  - `rclone crypt` (šifrovanie dát)

---

## 🧠 Odporúčanie

- Prvú synchronizáciu rob **manuálne**
- Sleduj priestor na disku (OneDrive môže byť väčší ako NAS!)
- Nepoužívaj WebDAV – nespĺňa požiadavky na stabilitu pri veľkom počte súborov
