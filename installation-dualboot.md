# 🐧 Windows 11 ve Fedora KDE Plasma: Dual-Boot Kurulum Rehberi

Bu rehber, mevcut Windows 11 sistemine zarar vermeden, yanına güvenli bir şekilde Fedora kurman için gereken tüm adımları içerir.

---

## 🏗️ 1. Windows Tarafında Hazırlık (Kritik)

Fedora'yı kurmadan önce Windows üzerinde şu iki işlemi mutlaka yapmalısın:

### A. Hızlı Başlatmayı (Fast Startup) Kapat
Windows "Hızlı Başlatma" açıkken diskleri kilitlemekte ve Fedora'nın bu disklere yazmasını engellemektedir.
1. **Denetim Masası > Güç Seçenekleri**'ne git.
2. **"Güç düğmelerinin yapacaklarını seçin"** kısmına tıkla.
3. **"Şu anda kullanılmayan ayarları değiştir"** seçeneğine tıkla.
4. **"Hızlı başlatmayı aç (önerilen)"** kutucuğundaki işareti **kaldır** ve değişiklikleri kaydet.

### B. Fedora İçin Yer Aç (Disk Bölümleme)
1. Başlat menüsüne sağ tıkla ve **"Disk Yönetimi"** (Disk Management) aracını aç.
2. Windows'un kurulu olduğu (genelde C:) birime sağ tıkla ve **"Birimi Küçült"** (Shrink Volume) de.
3. Küçültülecek miktar alanına Fedora için ayırmak istediğin miktarı MB cinsinden yaz (Örn: 100 GB için `102400`).
4. **"Küçült"** de. Artık diskin sonunda siyah renkli bir **"Ayrılmamış Alan"** (Unallocated Space) görmelisin. **Buraya dokunma, Fedora burayı kullanacak.**

---

## ⚙️ 2. BIOS Ayarları

Bilgisayarı yeniden başlat ve BIOS (HP Victus için **F10**) ekranına gir:
1. **Secure Boot:** Mutlaka **Disabled** (Kapalı) yap. (NVIDIA sürücüsü için hayati).
2. **Boot Menu (F9):** Hazırladığın Ventoy USB belleğini seçerek Fedora Live ortamını başlat.

---

## 🚀 3. Fedora Kurulum Adımları

Masaüstündeki **"Install to Hard Drive"** simgesine tıkla.

### A. Kurulum Hedefi (En Önemli Kısım)
1. **"Installation Destination"** (Kurulum Hedefi) kısmına gir.
2. Windows'un olduğu ana diski seç.
3. Alt taraftaki **Storage Configuration** kısmında **"Automatic"** seçeneğini işaretle. 
4. Sol üstteki **"Done"** butonuna bas.
5. Karşına bir pencere çıkacak. Fedora burada senin Windows'ta açtığın o boş alanı görecektir. **"Modify software selection"** veya doğrudan boş alanı kullanma onayı isteyecektir. 
   * Fedora, Windows'un yanındaki o siyah bölgeye (boş alan) kendini otomatik olarak **Btrfs** formatında kuracaktır.

> **⚠️ DİKKAT:** Eğer elle (Custom) bölümleme yapacaksan; Windows'un EFI bölümünü bul, onu seç ve mount point olarak `/boot/efi` olarak ata. **SAKIN FORMATLAMA (Don't Format)!** Sadece kullan de.

---

## 🏁 4. Kurulum Sonrası: Windows'u GRUB Menüsüne Ekleme

Kurulum bittiğinde bilgisayarı yeniden başlat. Eğer doğrudan Fedora açılırsa ve Windows'u listede görmezsen panik yapma. Fedora terminalini aç ve şu komutları sırayla gir:

```bash
# 1. Diğer sistemleri tarayan aracı kur
sudo dnf install os-prober

# 2. GRUB'ın Windows'u taramasına izin ver
echo "GRUB_DISABLE_OS_PROBER=false" | sudo tee -a /etc/default/grub

# 3. GRUB listesini güncelle
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

## 📌 Özet ve İpuçları
* **Boot Seçimi:** Bilgisayar her açıldığında karşına gelen menüden (GRUB) Windows veya Fedora arasında seçim yapabilirsin.
* **Saat Senkronizasyonu:** Eğer Windows ve Fedora arasında saat farkı (Windows saati geri kalırsa) olursa, Fedora terminalinde şu komutu çalıştır:
  `timedatectl set-local-rtc 1 --adjust-system-clock`
* **Hız:** 240Hz ekranını Windows'ta oyunlar için, Fedora'da ise profesyonel kod yazma deneyimi için kullanabilirsin.

**Dual-Boot sistemin hazır! Artık hantal Windows servislerinden sıkıldığında Fedora'ya sığınabilirsin.**
