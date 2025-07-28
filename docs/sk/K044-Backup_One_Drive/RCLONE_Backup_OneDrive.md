# 🗂️ Návod: Konfigurácia `rclone` pre zálohovanie OneDrive

Tento návod opisuje, ako nakonfigurovať `rclone` na zálohovanie súborov z Microsoft OneDrive na lokálny disk alebo iné úložisko.

---

## 🔧 1. Inštalácia `rclone`

### MacOS (Homebrew)
```bash
brew install rclone
```

---

## 🧰 2. Spustenie konfigurácie

```bash
rclone config
```

---

## 🧩 3. Vytvorenie novej vzdialenej lokácie (remote)

1. Zvoľ možnosť `n` pre **New remote**
2. Pomenuj napr. `sth-personal-onedrive`
3. Potom sa objaví zoznam typov úložísk – zadaj:

```text
Storage> 38
```

To zodpovedá voľbe: **Microsoft OneDrive**

---

## 🔐 4. Autentifikácia

Rclone sa opýta:

- Použiť vlastné client_id a client_secret? → `n`
- Povoliť konfiguráciu cez prehliadač? → `y` alebo Enter
- Otvorí sa nové okno v prehliadači pre prihlásenie do Microsoft účtu
- Po úspešnom prihlásení sa vrátiš späť do terminálu a konfigurácia sa dokončí

---

## 🧪 5. Overenie

Zoznam priečinkov v OneDrive:
```bash
rclone lsd sth-personal-onedrive:
```

---

## 💾 6. Záloha do lokálneho priečinka

```bash
rclone sync sth-personal-onedrive:/ ~/Backup/OneDrive --progress
```

---

## 🔁 7. Pravidelné spúšťanie

- Pridať do crontabu (`crontab -e`) alebo
- Použiť Automator / launchd na MacOS pre GUI nastavenie

---

## ✅ Tipy

- 🔍 `--dry-run` – simuluje prenos bez zmien
- ⏱️ `--progress` – zobrazí priebeh kopírovania
- 📧 Možnosti emailovej notifikácie sú podporované cez wrapper skripty alebo nástroje ako `msmtp`/`mail`

---

## 🧹 Odstránenie konfigurácie

```bash
rclone config
# vybrať d, vymazať remote
```