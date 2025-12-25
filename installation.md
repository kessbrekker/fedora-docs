# 💿 Fedora KDE Plasma: Sıfırdan Kurulum Kılavuzu

Bu rehber, yeni bir bilgisayara (FreeDOS) veya diskini tamamen temizlemek istediğin bir sisteme Fedora'yı en verimli şekilde kurman için hazırlanmıştır.

---

## 🛠 1. Hazırlık Aşaması

Kuruluma başlamadan önce ihtiyacın olanlar:
1. **USB Bellek:** En az 8 GB kapasiteli.
2. **ISO Dosyası:** [Fedora KDE Spin](https://fedoraproject.org/spins/kde/) adresinden indirilen en güncel dosya.
3. **Yazıcı Araç:** **Ventoy** (Önerilen) veya **Fedora Media Writer**.

> **İpucu:** Ventoy kullanıyorsan indirdiğin ISO dosyasını sürükleyip USB içine bırakman yeterlidir.

---

## ⚙️ 2. BIOS Ayarları (Kritik Adım)

Bilgisayarını açarken (HP Victus için **F10**) tuşuna basarak BIOS'a gir. Şu iki ayar hayati önem taşır:

1. **Secure Boot:** Mutlaka **Disabled** (Kapalı) yapılmalıdır. (NVIDIA sürücülerinin çalışması için şarttır).
2. **SATA Mode:** Eğer varsa **AHCI** olarak seçilmelidir (Intel sistemlerde RST yerine).
3. **Boot Order:** USB belleğini ilk sıraya al veya bilgisayarı açarken **F9** (Boot Menu) tuşuyla USB'yi seç.



---

## 🚀 3. Kurulum Ekranı (Adım Adım)

USB'den başlattığında karşına gelen menüden **"Start Fedora-KDE-Live"** seçeneğini seç. Masaüstü açıldığında **"Install to Hard Drive"** simgesine tıkla.

### A. Dil ve Klavye
* Dilini seç (Türkçe veya English).
* Klavye düzeninin doğruluğunu test et (Türkçe Q).

### B. Kurulum Hedefi (Disk Seçimi) - EN ÖNEMLİ KISIM
1. **Device Selection:** Kurulum yapacağın ana diski (SSD) seç.
2. **Storage Configuration:** * Eğer her şeyi silip temiz bir kurulum yapacaksan: **"Automatic"** seçeneğini işaretle.
   * Fedora senin için en modern dosya sistemi olan **Btrfs** yapısını otomatik kuracaktır.
3. **Done** tuşuna bas. Eğer diskte eski veriler varsa "Reclaim Space" ekranı gelir, **"Delete All"** diyerek devam et.



### C. Ağ ve Zaman Ayarı
* Sağ alttan Wi-Fi'ye bağlan.
* Saat dilimini "Europe/Istanbul" olarak doğrula.

---

## 🏁 4. Kurulumu Başlat ve Bitir

1. **"Begin Installation"** butonuna tıkla.
2. İşlem bittiğinde (yaklaşık 5-10 dk) **"Finish Installation"** de.
3. USB belleği çıkart ve bilgisayarı yeniden başlat.

---

## 🛠 5. İlk Açılış ve Kullanıcı Oluşturma

Bilgisayar açıldığında Fedora seni bir karşılama ekranıyla karşılar:
1. **Kullanıcı Adı:** Yazılımcı kimliğinle bir kullanıcı oluştur.
2. **Şifre:** Güçlü bir şifre belirle.
3. **Sistem Güncelleme:** Masaüstü geldiğinde bir terminal aç ve ilk iş olarak şunu yaz:
   ```bash
   sudo dnf upgrade --refresh
   ```

---

## 📌 Kurulum Sonrası Notlar (HP Victus & Yazılımcılar İçin)

* **240Hz Ayarı:** *Sistem Ayarları > Ekran* kısmına giderek yenileme hızını **240Hz** yapmayı unutma.
* **NVIDIA:** Ekran kartının tam performansı için hazırladığımız `fedora.md` dosyasındaki sürücü kurulum adımlarına geçebilirsin.
* **Snapshot:** Btrfs dosya sistemi sayesinde artık "sistemim bozulur mu" korkun bitti.

**Tebrikler! Artık gerçek bir mühendislik harikası olan Fedora dünyasındasın.**


---
[<-- Sistem Stratejisi](fedora.md) | [Dual-Boot Rehberi -->](installation-dualboot.md)
