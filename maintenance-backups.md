# 🛡️ Btrfs Snapshots ve Sistem Bakım Rehberi

Bu rehber, Fedora sistemini bir "kale" kadar sağlam tutmak ve Arch Linux'ta yaşadığın "sistemin bozulması" sorununu tamamen ortadan kaldırmak için hazırlanmıştır. Fedora'nın varsayılan **Btrfs** dosya sistemini bir zaman makinesi gibi kullanacağız.

---

## 🕒 1. Snapper: Sistem Zaman Makinesi

Snapper, Btrfs alt birimlerinin (subvolumes) anlık görüntüsünü (snapshot) alır. Bir güncelleme sonrası sistemin açılmazsa, saniyeler içinde düne geri dönebilirsin.

### Kurulum ve Yapılandırma
```bash
# Gerekli araçları kur
sudo dnf install snapper btrfs-assistant grub-btrfs

# Root (/) dizini için snapper konfigürasyonu oluştur
sudo snapper -c root create-config /

# İzinleri ayarla (Kullanıcı adını yaz)
sudo chmod a+rx .snapshots
sudo chown :$(whoami) .snapshots
```

### Btrfs Assistant (KDE İçin Görsel Arayüz)
Terminalle uğraşmak istemediğinde, menüden **Btrfs Assistant**'ı aç. Buradan:
* Elle snapshot alabilirsin (Örn: "NVIDIA sürücüsü kurmadan hemen önce").
* Eski snapshot'ları silebilirsin.
* Geri yükleme (Restore) yapabilirsin.

---

## 🔄 2. Boot Menüsünden Geri Dönüş (Grub-Btrfs)

Sistemin hiç açılmadığı o korkunç senaryoda, Fedora'yı eski bir tarihten başlatmak için:
1. Bilgisayarı açarken GRUB menüsünde **"Fedora Snapshots"** sekmesini göreceksin.
2. Sistemin çalışan son halini (tarihe göre) seç.
3. Sistem açıldığında `btrfs-assistant` veya `snapper rollback` ile o anı kalıcı hale getir.



---

## 🧹 3. Periyodik Sistem Temizliği

Sistemin şişmesini ve gereksiz yer kaplamasını önlemek için şu komutları `fedora-commands.md` içindeki `cls` fonksiyonuna ekleyebilirsin:

```bash
# 1. DNF önbelleğini temizle
sudo dnf clean all

# 2. Kullanılmayan Flatpak kütüphanelerini sil
flatpak uninstall --unused

# 3. Eski sistem loglarını temizle (Sadece son 2 günü tut)
sudo journalctl --vacuum-time=2d

# 4. Btrfs dengeleme (SSD ömrü ve alan yönetimi için ayda bir)
sudo btrfs balance start -dusage=50 /
```

---

## 💾 4. Harici Yedekleme (Kup Backup)

Btrfs snapshot'ları diskin fiziksel olarak bozulmasına karşı koruma sağlamaz. Kişisel verilerin (projelerin, SSH anahtarların) için KDE ile harika çalışan **Kup** kullan:

```bash
sudo dnf install kup
```
* **Strateji:** Harici bir disk veya USB belleği taktığında, Kup otomatik olarak belirlediğin klasörleri (örn: `~/Documents`, `~/Projects`) oraya senkronize eder.

---

## 🌡️ 5. Donanım Sağlığı Takibi

HP Victus'un ve SSD'nin durumunu periyodik olarak kontrol et:
```bash
# SSD sağlığı için
sudo dnf install smartmontools
sudo smartctl -a /dev/nvme0n1

# Batarya sağlığı için
upower -i /display_device
```

---

## 📌 Altın Kurallar (Yazılımcı Disiplini)
1. **Büyük Değişiklik Öncesi Snapshot:** `sudo dnf upgrade` demeden veya yeni bir Kernel/Sürücü denemeden önce bir snapshot al.
2. **Snapshot Temizliği:** Çok fazla snapshot disk alanını doldurabilir. Snapper ayarlarından "Number of snapshots to keep" kısmını 5-10 arası tut.
3. **Log Takibi:** Sistemde bir gariplik hissedersen `journalctl -p 3 -xb` komutuyla sadece hata (error) loglarını incele.

---
**"Arch Linux seni tamirci yapar, Fedora seni mühendis yapar. Mühendisler sistemlerini değil, geleceklerini inşa ederler."** 🚀


---
[<-- Oyun Optimizasyonu](gaming-optimization.md)
