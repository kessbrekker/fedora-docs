# 🚀 Arch'tan Fedora'ya Geçiş: Yazılımcı Strateji Belgesi

Bu doküman, **HP Victus 16-S0001nt (Ryzen 7840HS & RTX 4070)** donanımı üzerinde Windows 11'in hantallığından ve Arch'ın bakım yükünden kurtulup, saf performansa odaklanmak için hazırlanmıştır.

---

## 🛠️ 1. Kurulum Öncesi: Köprüleri Yakmadan Önce
Arch Linux'u silmeden önce şu kritik verileri USB'ye veya buluta yedekle:
- **SSH/Git:** `~/.ssh/` ve `~/.gitconfig`
- **Dotfiles:** `.zshrc`, `.p10k.zsh`, `.bashrc`
- **GRUB:** `/boot/grub/themes/[TEMA_ADIN]` (Sadece görsel klasör)
- **BIOS Hazırlığı:** Secure Boot'u **Disable** yap; NVIDIA akmod derlemesi için bu şarttır.

---

## ⚙️ 2. Temel Sistem Optimizasyonu
Kurulum biter bitmez paket yöneticisini (DNF) modernize et:

```bash
# DNF hızlandırma (Paralel indirme ve ayna seçimi)
echo -e "max_parallel_downloads=10\nfastestmirror=True" | sudo tee -a /etc/dnf/dnf.conf

# RPM Fusion depolarını (Sürücü ve Multimedia Codec'leri) aktif et
sudo dnf install [https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm](https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm) -E %fedora).noarch.rpm [https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm](https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm) -E %fedora).noarch.rpm

# Tam sistem güncellemesi
sudo dnf upgrade --refresh
```

---

## 🏎️ 3. GPU Mimari Yapılandırması (AMD + NVIDIA)
Masaüstü için AMD iGPU, ağır işler için RTX 4070 hibrit düzenini kur:

```bash
# NVIDIA Sürücüsü ve CUDA desteği
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda

# KRİTİK: Komut bittikten sonra 5 dakika bekle. 
# Çıktı versiyon dönene kadar reboot etme (Kernel modülü derleniyor):
modinfo -F version nvidia
```
* **Strateji:** Uygulamalara sağ tık -> *"Run with Graphics Processor"* ile NVIDIA'yı ateşle.

---

## 📂 4. GRUB Yönetimi ve Tema Entegrasyonu
`grub-customizer` gibi riskli araçlar yerine dosyaları manuel ve güvenli yönet:

```bash
# 1. Temayı taşı
sudo mkdir -p /boot/grub2/themes
sudo cp -r /run/media/user/USB/[TEMA_ADIN] /boot/grub2/themes/

# 2. Yapılandırmayı düzenle
sudo nano /etc/default/grub
# Satırı ekle: GRUB_THEME="/boot/grub2/themes/[TEMA_ADIN]/theme.txt"
# Gerekiyorsa: GRUB_DISABLE_RECOVERY="true"

# 3. GRUB konfigürasyonunu mühürle
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

## 💻 5. Yazılımcı İş Akışı: "Clean OS, Dirty Containers"
Sistemi kirletmeden Arch konforunu devam ettir:

```bash
# Dotfiles yönetimi için Chezmoi
sudo dnf install chezmoi
chezmoi init [https://github.com/USER/dotfiles.git](https://github.com/USER/dotfiles.git) && chezmoi apply

# Distrobox: Fedora içinde bir Arch terminali açmak için
sudo dnf install podman distrobox
distrobox create --image archlinux:latest --name arch-dev
distrobox enter arch-dev
# Artık Fedora'yı kirletmeden 'yay' ve 'pacman' emrinde.
```

---

## 🔋 6. Btrfs ve HP Victus Özel Ayarları
Btrfs dosya sistemi ile Windows'un yapamadığı "anlık geri dönüş" (Snapshot) gücünü kullan:

```bash
# Btrfs Sıkıştırma Kontrolü (Zstd)
cat /etc/fstab | grep btrfs # 'compress=zstd:1' olduğundan emin ol.

# HP Firmware Update
sudo fwupdmgr get-updates && sudo fwupdmgr update

# RAM Optimizasyonu (İsteğe bağlı, dosya indekslemeyi durdurur)
balooctl6 disable
```

---

## 📌 Özet Notlar
1. **RAM Karı:** Windows 10GB çalarken Fedora boşta ~1.5GB harcar.
2. **Güvenlik:** SELinux varsayılan olarak devrededir, kapatma.
3. **Wayland:** KDE Plasma 6 + NVIDIA 4070 Wayland üzerinde kusursuz çalışır; X11'e dönme.

```bash
# Bir Arch Linux konteynerı oluştur ve içine gir
toolbox create --distro arch -c arch-dev
toolbox enter arch-dev
# Artık Fedora'nın içindeki bir terminalde Arch kullanıyorsun!
```


---
[Sonraki: Temiz Kurulum Rehberi -->](installation.md)
