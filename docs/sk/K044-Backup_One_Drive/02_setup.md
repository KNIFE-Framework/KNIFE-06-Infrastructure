# K044 – Nastavenie a autorizácia rclone

1. Stiahni si a nainštaluj [`rclone`](https://rclone.org/downloads/)
2. Spusti `rclone config`
3. Vytvor nový remote:
   - typ: `onedrive`
   - autorizuj cez web prepojenie účtu Microsoft
4. Over pripojenie príkazom: `rclone ls onedrive:`

> Odporúčame remote pomenovať `onedrive`, ale môžeš použiť ľubovoľné meno.