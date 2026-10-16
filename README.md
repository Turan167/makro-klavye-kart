# Makro Klavye – kart yazılımı

Makro Klavye uygulaması klavyeye bağlanınca bu depodaki `surum.txt` dosyasına bakar.
Karttakinden yeni bir sürüm varsa sorar, onay verilirse zip'i indirip karta yükler.

## Yeni sürüm yayınlama
1. Yeni `MakroKlavye-Kart-X.Y.zip` dosyasını bu depoya yükle.
2. `surum.txt` dosyasını güncelle:
   - `SURUM=` zip'teki code.py'nin SURUM değeri
   - `ZIP=` zip dosyasının adı
   - `SHA256=` zip'in SHA-256 özeti (PowerShell: `Get-FileHash .\MakroKlavye-Kart-X.Y.zip`)
   - `NOT=` yenilikler (tek satır)

Eski zip'leri silmek zorunlu değil; uygulama yalnızca `surum.txt`'de yazanı indirir.
