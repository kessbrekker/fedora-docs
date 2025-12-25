# 💻 Yazılım Geliştirme ve Mühendislik Üssü Yapılandırması

Bu rehber; Web, Mobil, Oyun Geliştirme ve AI/ML mühendisliği için Fedora'yı en verimli hale getirmeyi amaçlar. RTX 4070 ve Ryzen 7840HS ikilisini birer "hesaplama canavarına" dönüştürüyoruz.

---

## 🤖 1. AI/ML ve AI Engineering (CUDA Gücü)

RTX 4070'in 5888 CUDA çekirdeği ve Tensor çekirdekleri, yerel LLM çalıştırmak ve model eğitmek için harikadır.

### CUDA ve Sürücü Kontrolü
Fedora'da NVIDIA sürücülerini kurduğunda CUDA hazır gelir. Şunları kurarak AI dünyasına gir:
```bash
sudo dnf install xorg-x11-drv-nvidia-cuda libva-nvidia-driver
```

### Python ve AI Paket Yönetimi (Modern Yaklaşım: `uv`)
Python paketlerini yönetmek için `pip` yerine ışık hızındaki `uv` aracını kullan:
```bash
curl -LsSf [https://astral.sh/uv/install.sh](https://astral.sh/uv/install.sh) | sh
# Yeni bir AI projesi başlatırken:
uv venv
uv pip install torch torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu121](https://download.pytorch.org/whl/cu121)
```

### Yerel LLM Çalıştırma (Ollama)
Kendi bilgisayarında Llama 3 veya Mistral çalıştırmak için:
```bash
curl -fsSL [https://ollama.com/install.sh](https://ollama.com/install.sh) | sh
# RTX 4070 üzerinde koştur:
ollama run llama3
```

---

## 🌐 2. Web Development (Backend & Frontend)

### Runtime Yönetimi (Node.js/Bun/Go)
Sistem paketlerini kirletme; `fnm` (Fast Node Manager) kullan:
```bash
curl -fsSL [https://fnm.vercel.app/install](https://fnm.vercel.app/install) | bash
fnm install --lts
```

### Konteynerize Servisler (Podman)
Postgres, Redis veya MongoDB'yi sistemine kurma, Docker uyumlu Podman ile ayağa kaldır:
```bash
alias docker=podman
podman run --name redis-dev -p 6379:6379 -d redis
```

---

## 🎮 3. Game Development (Unity, Unreal, Godot)

240Hz ekranın avantajını Game Dev sırasında da kullanmalısın.

* **Godot Engine:** Fedora'da en stabil çalışan motordur. `dnf` veya Flatpak ile kurabilirsin.
* **Unity/Unreal:** Wayland üzerinde bazen "flicker" (titreme) yapabilirler. Bunu önlemek için şu ortam değişkenini kullan:
    ```bash
    export QT_QPA_PLATFORM=xcb  # Eğer Wayland'da sorun çıkarsa IDE'yi X11 modunda başlatır
    ```
* **Blender:** RTX 4070 ile "OptiX" render motorunu seçmeyi unutma; render süresini 4-5 kat düşürür.

---

## 📱 4. Mobile App Development (Android & Flutter)

Android Studio, Linux üzerinde Windows'tan çok daha hızlı çalışır çünkü **KVM (Kernel-based Virtual Machine)** doğrudan donanım seviyesinde sanallaştırma yapar.

### Emülatör Hızlandırma
```bash
sudo dnf install @virtualization
sudo usermod -aG libvirt $(whoami)
# Emülatörü NVIDIA GPU ile çalıştırmak için Studio ayarlarından "Hardware GLES" seç.
```



---

## 🛠️ 5. Genel Programlama ve Araçlar

### Distrobox (Arch AUR'dan Kopamayanlar İçin)
Eğer sadece AUR'da olan bir geliştirme aracına (örn: çok özel bir SDK) ihtiyacın varsa:
```bash
distrobox create --name dev-box --image archlinux:latest
distrobox enter dev-box
# (Kutunun içinde) sudo pacman -S base-devel
```

### IDE Ayarları (240Hz & Wayland Fix)
VS Code'un yağ gibi akması için `~/.config/code-flags.conf` içine şunları ekle:
```text
--enable-features=UseOzonePlatform
--ozone-platform=wayland
--enable-gpu-rasterization
--enable-zero-copy
```

---

## 📌 Yazılımcı Tavsiyeleri (Best Practices)

1.  **Font:** Kodun 240Hz'de daha net görünmesi için **JetBrains Mono Nerd Font** kullan.
2.  **Terminal:** GPU hızlandırmalı **Kitty** veya **Alacritty** tercih et; işlemci yükünü azaltır.
3.  **Zsh/Oh-My-Zsh:** Terminal deneyimini `p10k` temasıyla özelleştirerek Git branch'lerini anlık takip et.
4.  **SELinux:** Fedora'nın güvenlik duvarıdır. Geliştirme yaparken engellere takılırsan tamamen kapatmak yerine `setenforce 0` ile geçici olarak esnetmeyi öğren.

---
**Bir sonraki adım:** [Btrfs Snapshots ve Sistem Bakımı](maintenance-backups.md) (Hazırlanıyor...)


---
[<-- Multimedya & Codec](multimedia-codecs.md) | [Oyun Optimizasyonu -->](gaming-optimization.md)
