# Makro Klavye

9 tuş ve 1 encoder (döner düğme) olan, RP2040 + CircuitPython ile çalışan bir makro klavye ve onu yöneten Windows uygulaması.
Tuşlara kısayol, metin, uygulama, makro ya da medya komutları atanır. Ayarlar klavyenin içinde saklanır;
uygulama kapalıyken ve başka bilgisayarda da çalışır.

Bu depo, uygulamanın ve kart yazılımının yayınlandığı yerdir. Uygulama açılınca buradan yeni sürüm olup olmadığına bakar.

---

## 1. Kurulum

### Uygulama
1. Depodaki `MakroKlavye-x.y.exe` dosyasını indir ve çalıştır.
2. İlk açılışta "bilgisayara kurulsun mu?" diye sorar. **Evet** dersen kendini
   `%LOCALAPPDATA%\Programs\MakroKlavye` klasörüne kurar, masaüstüne ve Başlat menüsüne kısayol koyar.
   İndirdiğin exe'yi sonra silebilirsin.
3. Kaldırmak için: Ayarlar > Uygulama > **KALDIR…** ya da Windows'ta *Uygulamalar ve özellikler*.

Güncellemeler kendiliğinden gelir: yeni sürüm çıkınca uygulama sorar (Ayarlar > Uygulama'dan sormadan kurması da seçilebilir).

### Kart (ilk kez)
Kartta CircuitPython kurulu olmalı. Sonra:
1. Klavyeyi **9 numaralı tuşa (encoder'ın altındaki) basılı tutarak** USB'ye tak. Böylece `CIRCUITPY` sürücüsüne yazılabilir.
2. Depodaki son `MakroKlavye-Kart-x.y.zip` içindeki dosyaları (`code.py`, `boot.py`, `macro_keys.py`,
   `led_animations.py`, `settings_server.py`) `CIRCUITPY` sürücüsünün köküne kopyala.
3. `CIRCUITPY\lib` klasöründe `adafruit_hid` klasörü ve `neopixel.mpy` olmalı (Adafruit CircuitPython kütüphane paketinden).
4. Klavyeyi çıkarıp **normal** tak.

Bundan sonra kart yazılımı da uygulamadan güncellenir, dosya kopyalamak gerekmez.

> Normal takıldığında `CIRCUITPY` sürücüsü bilgisayarda **görünmez**. Bu doğrudur: ayarları kart kendisi yazar,
> yazılım güncellemesi de uygulamadan yapılır. Sürücüye yine ulaşmak gerekirse 9 numaralı tuşa basılı tutarak tak.

---

## 2. Bağlanma
Uygulama açılınca klavyeyi kendiliğinden bulur (daha önce bağlandığı portu önce dener). Bulamazsa **BAĞLAN**'a bas.
Üst satırda klavyenin adı yazar; yanındaki **kalem** simgesiyle ad verebilirsin.

Bağlanmıyorsa: Mu, PuTTY, Arduino IDE gibi seri porta bağlanan bir program açıksa kapat; USB kablosunu ve girişini değiştir.

---

## 3. Tuş atama
1. Pencerede bir tuşa tıkla (ya da klavyenden **1-9**, encoder için **0**).
2. Basış türünü seç: **Kısa**, **Uzun**, **Çift** basış. Encoder'da ayrıca **Sağa** ve **Sola** çevirme var.
3. Komut kutusuna tıkla ve ne yapacağını seç (aşağıya bak).
4. **AYARI KAYDET** (Ctrl+S). Kaydedilmemiş değişiklik varsa düğmenin üstünde kırmızı yazı çıkar.

Yanlış olursa **Geri al** (Ctrl+Z). İki tuşu sürükleyip bırakarak yerlerini değiştirebilirsin; sağ tıkla kopyala / yapıştır / boşalt.
Tuşa sağ tıklayıp **ad, renk ve simge** verebilirsin (yalnızca uygulamada görünür, klavyede de saklanır).

### Komut menüsü
- Üstte **arama**: "ses", "kopyala", "sekme", "uyku"… yaz, Enter'la ilk sonucu seç (oklarla gezilebilir).
- Altında beş tür: **Kısayol** (kendi tuş kombinasyonun), **Metin** (yazı yazdır), **Uygulama** (program, klasör, site aç),
  **Makro** (çok adımlı), **Boş**.
- En altta kategoriler: Genel, Düzenleme, Medya ve Ses, Tarayıcı, Pencere ve Sistem, Özel Tuşlar, Fare.
  En çok kullandıkların "Sık kullanılan"da toplanır.
- **F13–F24**: gerçek klavyede olmayan fonksiyon tuşları. Bir tuşa ata, sonra OBS, Discord, AutoHotkey gibi
  programların kısayol ayarında o tuşa bas: hiçbir kısayolla çakışmaz.

### Metin
Türkçe harfler için Ayarlar > Kart > **KLAVYE AYARLARI**'ndan bilgisayarındaki düzeni seç (**Türkçe Q**, **Türkçe F** ya da İngilizce).
é, ä, ñ, ß gibi yabancı harfler de yazılır. Metnin içine `{ENTER}`, `{TAB}`, `{CTRL+A}` gibi tuşlar yazılabilir: `sifre{ENTER}`.
Emoji yazılamaz.

### Uygulama, klasör, site
Bir program, klasör, dosya ya da web adresi seç. Klavye bunları Windows'un Ctrl+Alt+harf kısayoluyla açar;
uygulama kapalıyken de çalışır.

---

## 4. Makro
Bir tuşa en fazla 20 adımlık bir dizi atanır. Adım türleri:

| Sekme | Ne yapar |
|---|---|
| Kısayol | Tuş kombinasyonu (Alt+Tab, Ctrl+Shift+T…) |
| Metin | Yazı yazdırır |
| Bekle | Sonraki adımdan önce bekler (en fazla 5000 ms) |
| Komut | Hazır komut (ses, medya, sol tık…) |
| Fare | Tekerleği çevirir ya da imleci taşır (x: sağa +, y: aşağı +) |
| Tekrar | Üstündeki adımları birkaç kez çalıştırır |
| Tut | Tuşu ya da fare tuşunu basılı tutar veya bırakır |

- **Kayıt al**: Bastığın tuşlar, yazdığın metin, fare tıklamaları ve tekerlek adım olarak eklenir. Bitince **Durdur**.
- Adıma **çift tıkla**: düzenle. **Sürükle**: sırala. **Ctrl+D**: çoğalt. **Delete**: sil.
- **Ctrl+C / Ctrl+V**: adımları başka bir makroya kopyala.
- Sağ tık > **Not ekle**: kendin için açıklama (klavye çalıştırmaz).
- **Sürükle-bırak örneği:** Tut "Sol tık basılı tut" → Fare "İmleci taşı" → Tut "Bırak".
- **Makrolarım**: makroyu ad vererek sakla, başka tuşta kullan.

---

## 5. LED
**LED** düğmesi: efekt, renk, parlaklık, hız ve akış renkleri. Kaydetmeden önce klavyede görürsün.
Klavyedeki LED tuşu (varsayılan 9 numaralı tuş) efektler arasında geçer.

---

## 6. Profiller, geçmiş, yedek
- **Profiller**: farklı işler için ayrı tuş düzenleri (ör. "Oyun", "Toplantı"). Üst çubuktan ya da tepsi menüsünden tek tıkla geçilir.
  Ayarlar > Profiller'deki **HAZIR PROFİLLER…** ile hazır düzenlerden başlanabilir: Ofis, Müzik ve medya, Tarayıcı,
  Toplantı (Teams), Oyun ve yayın, Video düzenleme, Sayı tuş takımı, OBS Studio, Visual Studio, Sunum (PowerPoint).
- **Geçmiş**: Klavyeye her yazışta o anki hâl otomatik saklanır (son 20). Ayarlar > Geçmiş'ten istediğin ana dönebilirsin;
  neyin değişeceği önceden gösterilir.
- **Tam yedek**: Ayarlar > Profiller > **YEDEKLE…** her şeyi (profiller, makrolar, geçmiş, klavyedeki ayarlar) tek zip'e koyar.
  Başka bilgisayara taşımak için **GERİ YÜKLE…**

---

## 7. Klavye ayarları
Ayarlar > Kart > **KLAVYE AYARLARI…**

| Ayar | Açıklama |
|---|---|
| Metin yazma düzeni | Türkçe Q / Türkçe F / İngilizce: bilgisayarındakiyle aynı olmalı |
| Harfler arası bekleme | Bazı programlar harf kaçırırsa artır |
| Uyku engelleyici | Fare arada bir 1 piksel kıpırdar, bilgisayar uyumaz. Tuşa da atanabilir: *Pencere ve Sistem > Uyku Engelleyici* |
| Uzun basış / çift basış | Süreler |
| Tuş titreşim filtresi | Bir basış iki kez algılanıyorsa artır |
| Encoder | Yön, hassasiyet ve tık başına kaç kez çalışacağı |

Ayarlar penceresinde aradığını üstteki kutuyla bulabilirsin (**Ctrl+F**).

---

## 8. Güncellemeler
- **Uygulama**: yeni sürüm çıkınca sorar. Kurunca kapanıp yeni sürümle açılır, ayarların korunur.
- **Kart yazılımı**: bağlanınca sorar ya da Ayarlar > Kart > **GÜNCELLE**. Tuş atamaların değişmez.
  - **Güvenli güncelleme**: yeni yazılım açılamazsa ya da ilk saniyelerde hata verirse kart kendiliğinden eski sürüme döner.
  - **ÖNCEKİ SÜRÜME DÖN…**: yeni sürümü beğenmezsen bir önceki sürüme döner.
- Windows'ta klavye **"Makro Klavye"** adıyla görünür (kart güncellendikten sonra bir kez çıkarıp tak).

---

## 9. Birden çok klavye
Her klavye kendini tanıtır; uygulama hangisinin bağlı olduğunu bilir. Her klavyenin **geçmişi ve seçili profili ayrıdır**.
Üst satırdaki kalemle klavyelere ad ver. Ayarlar > Geçmiş > **DİĞER KLAVYEDEN AL…** bir klavyenin ayarlarını ötekine kopyalar.

---

## 10. Sorun giderme
| Sorun | Çözüm |
|---|---|
| Klavye bulunamıyor | Seri porta bağlanan programları (Mu, PuTTY…) kapat; başka kablo / USB girişi dene |
| "Salt okunur, kaydedilemedi" | Klavyeyi 9 numaralı tuşa basmadan yeniden tak |
| Metinde harfler yanlış çıkıyor | KLAVYE AYARLARI'nda düzeni bilgisayarındakiyle aynı yap |
| Bazı programlarda harf kaçıyor | Harfler arası beklemeyi artır |
| Tek basış iki kez çalışıyor | Tuş titreşim filtresini artır |
| Kartta ne olduğunu görmek | Ayarlar > Kart > **TANILAMA…** (sürüm, boş alan, sıcaklık, son hatalar) |

Uygulama bir hatayla karşılaşırsa ayrıntılar `%APPDATA%\MakroKeyboard\hatalar.txt` dosyasına yazılır.
Olay günlüğü: Ayarlar > Günlük.

---

## Geliştirici: yeni kart sürümü yayınlama
1. Yeni `MakroKlavye-Kart-X.Y.zip` dosyasını bu depoya yükle.
2. `surum.txt` dosyasını güncelle:
   - `SURUM=` zip'teki code.py'nin SURUM değeri
   - `ZIP=` zip dosyasının adı
   - `SHA256=` zip'in SHA-256 özeti (PowerShell: `Get-FileHash .\MakroKlavye-Kart-X.Y.zip`)
   - `NOT=` yenilikler (tek satır)

Uygulama sürümü Ayarlar > Uygulama > **YAYIN HAZIRLA…** ile hazırlanır (yalnızca imza anahtarının olduğu bilgisayarda görünür).
Çıkan exe ve `uygulama.txt` bu depoya yüklenir. Eski zip / exe dosyalarını silmek zorunlu değil;
uygulama yalnızca `surum.txt` ve `uygulama.txt`'de yazanı indirir.
