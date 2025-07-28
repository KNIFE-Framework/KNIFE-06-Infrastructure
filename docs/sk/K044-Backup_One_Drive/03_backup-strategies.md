# K044 – Scenáre a stratégie zálohovania

Možnosti zálohovania:

## 1. Full mirror

Zrkadlenie všetkého obsahu:

```bash
rclone sync onedrive:/ ~/Zalohy/OneDrive/ --progress
```

## 2. Inkrementálny backup

Použi `rclone copy` pre neexistujúce súbory:

```bash
rclone copy onedrive:/ ~/Zalohy/OneDrive/ --progress --update
```

## 3. Filtrovanie obsahu

Zálohuj len konkrétny priečinok alebo typy súborov:

```bash
rclone sync onedrive:/Dokumenty/ ~/Zalohy/Dokumenty/ --include "*.docx"
```

> Upozornenie: `--delete-excluded` môže spôsobiť stratu dát, používaj opatrne.


[Home](../index.md)