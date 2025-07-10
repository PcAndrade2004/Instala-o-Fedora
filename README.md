0# Instala-o-Fedora

#!/bin/bash

set -e

echo "==== Atualizando sistema ===="
sudo dnf update -y

echo "==== Configurando nome da máquina ===="
sudo hostnamectl set-hostname "minilboz"

##  "==== Instalando GNOME Tweaks e extensão ===="
sudo dnf install -y gnome-tweaks gnome-extensions-app

echo "==== Ativando RPM Fusion ===="
sudo dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf upgrade --refresh -y
sudo dnf install -y rpmfusion-free-release-tainted rpmfusion-nonfree-release-tainted dnf-plugins-core

echo "==== Instalando codecs multimídia ===="
sudo dnf install -y gstreamer1-plugins-{bad-*,good-*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
sudo dnf install -y lame* --exclude=lame-devel
sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf install -y gstreamer1-plugin-openh264 mozilla-openh264
sudo dnf swap ffmpeg-free ffmpeg --allowerasing -y
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin -y
sudo dnf groupupdate sound-and-video -y
sudo dnf install -y amrnb amrwb faad2 flac gpac-libs libde265 libfc14audiodecoder mencoder x264 x265 ffmpegthumbnailer

echo "==== Instalando fontes da Microsoft ====\n"
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
echo "==== Instalando compactadores e utilitários ===="
sudo dnf install -y unzip p7zip p7zip-plugins unrar

echo "==== Atualizando firmware ===="
sudo fwupdmgr refresh --force
sudo fwupdmgr update

echo "==== Instalando VirtualBox ===="
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y kernel-headers kernel-devel dkms elfutils-libelf-devel qt5-qtx11extras
sudo rpm --import https://www.virtualbox.org/download/oracle_vbox_2016.asc
sudo wget -P /etc/yum.repos.d/ https://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
sudo dnf install -y VirtualBox-7.1
sudo usermod -aG vboxusers $USER
wget https://download.virtualbox.org/virtualbox/7.1.0/Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack
sudo VBoxManage extpack install Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack

echo "==== Instalando 1Password ===="
sudo rpm --import https://downloads.1password.com/linux/keys/1password.asc
sudo tee /etc/yum.repos.d/1password.repo > /dev/null <<EOF
[1password]
name=1Password Stable Channel
baseurl=https://downloads.1password.com/linux/rpm/stable/\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://downloads.1password.com/linux/keys/1password.asc
EOF
sudo dnf install -y 1password

echo "==== Instalando Piper para mouse Logitech ===="
sudo dnf install -y piper

echo "=== Adicionando e configurando repositorio flathub ==="
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

flatpak update

echo "==== Instalando Steam e ferramentas de jogos ===="
sudo dnf install -y steam
flatpak install -y flathub com.vysp3r.ProtonPlus
flatpak install -y flathub com.steamgriddb.steam-rom-manager
flatpak install -y flathub com.steamgriddb.SGDBoop
flatpak install -y flathub info.cemu.Cemu
flatpak install -y flathub com.usebottles.bottles
flatpak install -y flathub com.heroicgameslauncher.hgl
flatpak install -y flathub dev.lizardbyte.app.Sunshine

echo "==== Instalando Flatpaks úteis ===="
flatpak install -y flathub com.discordapp.Discord
flatpak install -y flathub com.spotify.Client
flatpak install -y flathub tech.feliciano.pocket-casts
flatpak install -y flathub com.obsproject.Studio
flatpak install -y flathub io.github.celluloid_player.Celluloid
flatpak install -y flathub com.mattjakeman.ExtensionManager
flatpak install -y flathub com.github.tchx84.Flatseal
flatpak install -y flathub org.mozilla.Thunderbird
flatpak install -y flathub org.nickvision.tubeconverter
flatpak install -y flathub org.localsend.localsend_app
flatpak install -y flathub page.codeberg.libre_menu_editor.LibreMenuEditor
flatpak install -y flathub de.haeckerfelix.Fragments
flatpak install -y flathub com.rtosta.zapzap
flatpak install -y flathub com.todoist.Todoist
flatpak install -y flathub io.github.pwr_solaar.solaar
flatpak install -y flathub io.github.brunofin.Cohesion

echo "==== Script concluído com sucesso! ===="


echo "==== Instalando VS Code (via Flatpak) ===="
flatpak install -y flathub com.visualstudio.code

echo "==== Instalando Java 21 (OpenJDK) ===="
sudo dnf install -y java-21-openjdk java-21-openjdk-devel

echo "==== INstalando Icones ===="
cd ~/.icons
git clone https://github.com/vinceliuice/Tela-icon-theme.git
cd Tela-icon-theme
./install.sh -a
