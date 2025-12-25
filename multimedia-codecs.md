# 🎬 Multimedia Codecs ve Donanım Hızlandırma

Fedora, özgür yazılım felsefesi nedeniyle lisanslı video ve ses kodeklerini (H.264, H.265, AAC vb.) kutu içeriğinde sunmaz. Bu rehber, RTX 4070 ve Ryzen 7840HS donanımının medya yeteneklerini tam kapasite kullanmanı sağlar.

---

## 🛠️ 1. Temel Codec Paketlerinin Kurulumu

Öncelikle RPM Fusion depolarının aktif olduğundan emin ol (bkz: `fedora.md`). Ardından şu komutla temel multimedia gruplarını kur:

```bash
# Temel multimedia ve ffmpeg paketleri
sudo dnf groupinstall "Multimedia" "Sound and Video"

# Ekstra kodekler (H.264, H.265 ve diğerleri)
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-libav --plugin-exclude=gstreamer1-plugins-bad-free-devel
sudo dnf install lame\* --exclude=lame-devel
sudo dnf install libva-utils
```

---

## 🏎️ 2. Donanım Hızlandırma (Hardware Acceleration)

Videonun CPU yerine RTX 4070 veya AMD iGPU tarafından işlenmesini sağlamak, pil ömrü ve sistem serinliği için kritiktir.

### NVIDIA VA-API Sürücüsü
NVIDIA kartlar için VA-API desteğini şu komutla kur:

```bash
sudo dnf install nvidia-vaapi-driver libva-utils
```

### AMD iGPU (Radeon 780M) İçin
Eğer tarayıcıyı dahili grafik kartı ile çalıştırıyorsan:

```bash
sudo dnf install libva-mesa-driver
```

---

## 🌐 3. Web Tarayıcı Optimizasyonu (Firefox & Chrome)

Kodları kurmak yetmez, tarayıcıya "ekran kartını kullan" demen gerekir.

### Firefox
1. Adres satırına `about:config` yaz.
2. `media.ffmpeg.vaapi.enabled` değerini `true` yap.
3. `media.rdd-ffmpeg.enabled` değerini `true` yap.

### Chromium / Chrome / Brave
Uygulamayı başlatırken şu bayrakları (flags) kullanabilir veya `chrome://flags` altından aktif edebilirsin:
* `Hardware-accelerated video decode` -> **Enabled**

---

## 🧪 4. Kurulumu Test Etme

Kurulumun başarılı olup olmadığını anlamak için terminale şu komutu yaz:

```bash
vainfo
```

**Başarılı Sonuç:** Ekranda `VAEntrypointVLD` satırlarını ve kodek listesini (H.264, HEVC, VP9 vb.) görmelisin. Eğer hata alıyorsan sürücüler henüz tam yüklenmemiş olabilir.

---

## 📌 Neden Bu Ayarlar Gerekli?
* **Pil Ömrü:** Donanım hızlandırma kapalıyken 4K bir video işlemciye %30-40 yük bindirir. GPU ile bu yük %1-2'ye düşer.
* **Akıcılık:** 240Hz ekranında videoların kare atlamadan (stutter-free) oynaması için GPU senkronizasyonu şarttır.
* **Yazılımcı Notu:** Eğer OBS Studio ile ekran kaydı alacaksan, bu kodekler olmadan NVENC (NVIDIA Encoder) çalışmayacaktır.

---
[<-- Komut Sözlüğü](fedora-commands.md) | [Geliştirme Ortamı -->](development-env.md)
