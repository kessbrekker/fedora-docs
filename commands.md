# 📦 Fedora Paket Yönetimi (DNF) vs Arch (Pacman)

Bu rehber, Arch Linux'tan Fedora'ya geçen yazılımcılar için hızlı bir "kas hafızası" dönüşüm kılavuzudur.

---

## 🚀 1. Temel Paket İşlemleri

| İşlem | Arch (Pacman) | Fedora (DNF) |
| :--- | :--- | :--- |
| **Sistemi Güncelle** | `sudo pacman -Syu` | `sudo dnf upgrade` |
| **Paket Kur** | `sudo pacman -S paket` | `sudo dnf install paket` |
| **Paket Sil** | `sudo pacman -Rs paket` | `sudo dnf remove paket` |
| **Paket Ara** | `pacman -Ss kelime` | `dnf search kelime` |
| **Paket Bilgisi** | `pacman -Qi paket` | `dnf info paket` |
| **Yerel Paketi Kur** | `sudo pacman -U paket.tar.zst` | `sudo dnf install ./paket.rpm` |
| **Yetim Paketleri Sil** | `sudo pacman -Rns $(pacman -Qdtq)` | `sudo dnf autoremove` |
| **Önbelleği Temizle** | `sudo pacman -Scc` | `sudo dnf clean all` |

---

## 🛠️ 2. Yazılımcılar İçin "Gelişmiş" Komutlar

### A. "Bu Dosya Hangi Paketten Geliyor?"
Arch'ta `pacman -Qo /path/to/file` yaptığın işlemin karşılığı:
```bash
dnf provides /usr/bin/gcc
```

### B. Grup Paketleri (Toolchain Kurulumu)
Arch'ta paketleri tek tek veya `base-devel` gibi gruplarla kurarsın. Fedora'da "Environment Groups" çok güçlüdür:
```bash
# Geliştirme araçlarının listesini gör
dnf group list

# C/C++, Make, GCC gibi tüm araçları tek seferde kur
sudo dnf groupinstall "Development Tools"
```

### C. DNF History (Hata Yaparsan Kurtarıcı)
Fedora, yaptığın her `install/remove` işlemini bir ID ile kaydeder.
```bash
# Yapılan son işlemleri listele
dnf history

# Örneğin 5 numaralı işlemi (yanlışlıkla sildiğin bir paketi) geri al
sudo dnf history undo 5
```

---

## 🏗️ 3. AUR Alternatifleri: Copr ve Flatpak

Fedora'da AUR yoktur, ancak iki güçlü alternatif vardır:

### A. Copr (Fedora'nın AUR'u)
Kullanıcıların kendi depolarını barındırdığı yerdir.
```bash
# Bir depoyu aktif et (Örn: LazyGit)
sudo dnf copr enable atim/lazygit
sudo dnf install lazygit
```

### B. Flatpak (Evrensel Uygulamalar)
Geliştiricilerin (Discord, VS Code, Spotify) en güncel sürümlerini sistem kütüphanelerini kirletmeden kurmak için:
```bash
# Uygulama ara
flatpak search vscode

# Uygulama kur
flatpak install flathub com.visualstudio.code
```

---

## ⚡ 4. Bonus: DNF5 (Geleceğin Hızı)
Fedora 41/42+ ile birlikte gelen `dnf5`, Arch'ın `pacman` hızına çok yakındır. Eğer sisteminde yüklüyse `dnf` yerine `dnf5` yazarak aynı komutları çok daha hızlı çalıştırabilirsin:
```bash
sudo dnf5 install docker
```

---

## 📌 Yazılımcı Tavsiyesi
* **Alias Atama:** Eğer `pacman` komutlarına çok alıştıysan, `.zshrc` dosyana şu alias'ları ekleyerek geçiş sürecini yumuşatabilirsin:
  ```bash
  alias install='sudo dnf install'
  alias update='sudo dnf upgrade'
  alias search='dnf search'
  ```
* Arch'taki kısa komut alışkanlığını Fedora'ya taşımak için aşağıdaki kodları `~/.zshrc` (veya `~/.bashrc`) dosyanın en altına ekle. Bu fonksiyonlar, sistemin yerleşik komutlarıyla çakışmaz ve çok daha kararlı çalışır.
  ```bash
  # --- Fedora Hızlı Komutlar ---
  
  # Paket yükleme (Örn: inst firefox discord)
  inst() {
      sudo dnf install "$@"
  }
  
  # Paket silme (Örn: rem vlc)
  rem() {
      sudo dnf remove "$@"
  }
  
  # Sistem güncelleme (Tek komut: upd)
  upd() {
      sudo dnf upgrade
  }
  
  # Temizlik (Örn: cls)
  cls() {
      sudo dnf autoremove && sudo dnf clean all
  }
  ```
*Not: Değişikliklerin aktif olması için terminale `source ~/.zshrc` yazmayı unutma.*
* **Konteyner Kullanımı:** Sistem paketlerini (`dnf`) sadece temel araçlar için kullan. Projeye özel kütüphaneleri (Python env, Node modules) her zaman **Distrobox** veya **Toolbox** içinde tut.
