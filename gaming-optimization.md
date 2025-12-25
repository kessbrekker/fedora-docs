# 🎮 Oyun Optimizasyonu ve 240Hz Performans Rehberi

Bu rehber, RTX 4070 ekran kartının ve 240Hz yenileme hızına sahip ekranın Fedora (Wayland) üzerinde en yüksek performansta çalışması için gereken yapılandırmaları içerir.

---

## 🏎️ 1. Temel Oyun Araçlarının Kurulumu

Oyun performansını izlemek ve artırmak için şu üçlü silahı mutlaka kurmalısın:

```bash
# MangoHud (FPS ve Sistem İzleme), GameMode (Performans Kilidi), Steam
sudo dnf install mangohud gamemode steam
```

---

## 📺 2. 240Hz ve Akıcılık Ayarları (Wayland)

Fedora KDE Plasma 6, Wayland üzerinde **Explicit Sync** desteğiyle gelir. Bu, NVIDIA kartlarda yırtılmayı (tearing) önler ve gecikmeyi (input lag) minimize eder.

### Ekran Ayarları
1. **Sistem Ayarları > Ekran ve Monitör** kısmına git.
2. Yenileme hızını **240Hz** olarak seç.
3. **Adaptive Sync (VRR):** Eğer monitörün destekliyorsa (G-Sync uyumlu), bunu "Always" veya "Automatic" yap. Bu, FPS düşüşlerinde takılma hissini yok eder.

---

## 🚀 3. GameMode ve MangoHud Yapılandırması

### GameMode Nedir?
Feral Interactive tarafından geliştirilen bu araç, oyun başladığında:
* İşlemciyi "Performance" moduna alır.
* I/O önceliğini oyuna verir.
* Ekran koruyucuyu devre dışı bırakır.

**Kullanımı:** Steam'de oyunun özelliklerine gir ve başlatma seçeneklerine şunu ekle:
`gamemoderun %command%`

### MangoHud (FPS Sayacı)
Ekranın bir köşesinde sıcaklık, FPS ve GPU kullanımını görmek için:
**Kullanımı:** `mangohud %command%`

---

## 🛠️ 4. Steam ve Proton GE (En Önemli Kısım)

Windows oyunlarını Linux'ta çalıştırmak için Steam'in "Proton" katmanını kullanıyoruz. Ancak topluluk tarafından geliştirilen **Proton GE**, en güncel oyun yamalarını ve video kodeklerini içerir.

1. **ProtonUp-Qt Kurulumu:**
   ```bash
   flatpak install flathub net.davidotek.pupgui2
   ```
2. Bu uygulamayı aç, "Add Version" de ve en güncel **GE-Proton** sürümünü kur.
3. Steam'i yeniden başlat, oyunun özelliklerinden **Compatibility** sekmesine gel ve kurduğun GE sürümünü seç.

---

## ⚡ 5. "nvrun" ile NVIDIA'yı Ateşle

Eğer bir oyunu Steam dışından (örneğin Lutris veya Heroic) çalıştırıyorsan ve oyun yanlışlıkla AMD iGPU ile açılırsa, `fedora-commands.md` dosyasında oluşturduğumuz alias'ı kullan:

```bash
nvrun ./oyun_dosyasi
```

---

## 📊 6. Performans İzleme ve Test

Oyun sırasında her şeyin yolunda olduğunu doğrulamak için MangoHud üzerinden şu değerleri kontrol et:
* **GPU Load:** %90+ (Ekran kartın tam güçte çalışıyor).
* **VRAM:** 8GB limitine dikkat et.
* **FPS:** 240Hz ekranında 240 FPS'i hedefle (Explicit Sync sayesinde artık takılma olmayacak).

---

## 📌 Özet Strateji
* **Steam Oyunları:** Başlatma seçeneğine `gamemoderun mangohud %command%` yaz.
* **Wayland:** X11'e dönme; KDE 6 + NVIDIA 555+ sürücüsü 240Hz için en stabil ortamdır.
* **Güç Profili:** Oyun oynarken KDE pil simgesinden "Performance" moduna geçmeyi unutma.

---
**Bir sonraki adım:** [Yazılım Geliştirme Ortamı (Podman & Distrobox)](development-env.md) (Hazırlanıyor...)
