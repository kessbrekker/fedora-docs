# 🚀 Fedora KDE Plasma: HP Victus 16 Geliştirici Rehberi

Bu depo, **HP Victus 16-S0001nt (Ryzen 7 7840HS & RTX 4070)** donanımı üzerinde, Arch Linux'un esnekliğini Fedora'nın stabilitesiyle birleştiren bir geliştirme ortamı kurmak için oluşturulmuştur.



---

## 📂 Dokümantasyon Dizini

Aşağıdaki bağlantıları kullanarak ihtiyacın olan kurulum veya yapılandırma rehberine doğrudan ulaşabilirsin:

| Dosya | Açıklama | Hedef |
| :--- | :--- | :--- |
| 💿 **[Temiz Kurulum Rehberi](installation.md)** | FreeDOS cihazlar için sıfırdan Fedora kurulumu. | Yeni Başlangıç |
| 🛡️ **[Dual-Boot Rehberi](installation-dualboot.md)** | Windows 11 yanına Fedora kurulumu ve disk yönetimi. | Hibrit Kullanım |
| 🏎️ **[Sistem Stratejisi](fedora.md)** | GPU (NVIDIA), GRUB Teması ve Kernel optimizasyonları. | Performans |
| 📦 **[Komut Sözlüğü](fedora-commands.md)** | DNF kullanımı, Arch vs Fedora ve özel shell fonksiyonları. | Verimlilik |

---

## 💻 Donanım Hedefi (Target Specs)

Bu rehberdeki tüm ayarlar aşağıdaki konfigürasyon için test edilmiş ve doğrulanmıştır:
* **İşlemci:** AMD Ryzen 7 7840HS (Zen 4)
* **Ekran Kartı:** NVIDIA GeForce RTX 4070 (8GB)
* **Ekran:** 16.1" FHD 240Hz
* **Dosya Sistemi:** Btrfs (Subvolume yapılandırması ile)

---

## 🛠️ Hızlı Başlangıç (Post-Install)

Kurulumu bitirdikten sonra sistemdeki "kas hafızasını" güncellemek için terminale şu fonksiyonları eklemeyi unutma:

```bash
# Fonksiyonları yüklemek için:
source ~/.zshrc # veya ~/.bashrc
```

**Kritik Kontrol Listesi:**
- [ ] BIOS: Secure Boot = Disabled
- [ ] Windows: Fast Startup = Disabled (Dual-Boot ise)
- [ ] Fedora: NVIDIA Akmod derlemesi tamamlandı mı? (`modinfo -F version nvidia`)
- [ ] KDE: Yenileme hızı 240Hz olarak ayarlandı mı?

---

## 🤝 Katkı ve Notlar

Bu dokümantasyon, Arch Linux yorgunluğunu üzerinden atmak ve sadece "üretmeye" odaklanmak isteyen bir yazılımcı tarafından hazırlanmıştır. Hata alırsan veya yeni bir optimizasyon bulursan issue açmaktan çekinme.

---
**"Sistem seni değil, sen sistemi yönet."** 🚀
