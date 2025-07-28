# 🧰 KNIFE_Backup_OneDrive.md – Záloha OneDrive na QNAP cez `rclone`

## 1️⃣ Inštalácia `rclone`
Na Macu:
```bash
brew install rclone
```

---

## 2️⃣ Spustenie konfigurácie
```bash
rclone config
```

## 3️⃣ Vytvorenie nového „remote“
- `n` → new remote
- zadaj názov: `sth-personal-onedrive-02`
- typ: `onedrive`

## 4️⃣ OAuth autorizácia
- nechaj `client_id` a `client_secret` prázdne
- `region`: `1` (global)
- `tenant`: prázdne
- **Edit advanced config?** → `n`
- **Use web browser to authenticate?** → `y`

Otvorí sa prehliadač, autorizuj prístup.

## 5️⃣ Výber typu a drive-u
- `config_type`: `1` (OneDrive Personal)
- vyber drive, napr. `3` = `OneDrive (personal)`
- potvrď: `y`

## 6️⃣ Dokončenie
- zobrazí sa URL a token, ktoré sa uložia.
- zvoľ `y` → remote sa uloží ako `sth-personal-onedrive-02`.

---

## 🧪 Test pripojenia
```bash
rclone lsd sth-personal-onedrive-02:
```

## 📥 Záloha na QNAP

Príkaz na **mirror zálohu z OneDrive do QNAP zložky**:
```bash
rclone sync "sth-personal-onedrive-02:" "/Volumes/NAS_Backup/OneDrive_Backup" --progress --create-empty-src-dirs
```

> 📝 Ak QNAP je pripojený ako `/Volumes/NAS_Backup`, môžeš si to overiť cez:
```bash
ls /Volumes
```

---

## 🧾 Pridanie notifikácií (voliteľné)
Môžeš použiť `terminal-notifier` alebo `osascript` pre zobrazenie po skončení:
```bash
rclone sync ... && osascript -e 'display notification "Backup finished" with title "Rclone"'
```

Alebo pre emailové notifikácie použiť `rclone --log-file` + vlastný shell script cronom.