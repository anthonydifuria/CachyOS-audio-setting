# CachyOS / ThinkPad X13 — Configuration History 2026

> Operational history of the configuration done so far.
> System: CachyOS rolling + Hyprland
> Shell: Fish
> Hardware: ThinkPad X13, Ryzen 5 PRO 4650U, 32 GiB RAM, Radeon Vega/Renoir iGPU.

---

## 1. Toolbox

Installed:

```fish
sudo pacman -S --needed \
  base-devel git curl wget unzip zip tar rsync tree file which \
  man-db man-pages less nano neovim htop btop mc zellij tmux \
  fzf ripgrep fd bat eza ncdu jq fastfetch procs dust duf openssh lazygit
```

Main tools:

* Git
* Neovim
* htop / btop
* Midnight Commander
* Zellij / tmux
* fzf / ripgrep / fd
* bat / eza
* lazygit
* OpenSSH

---

## 2. C/C++ Toolchain

Installed:

```fish
sudo pacman -S --needed \
  gcc clang llvm cmake ninja gdb lldb make meson ccache pkgconf \
  autoconf automake libtool python-pip python-virtualenv
```

Main versions verified:

* GCC 16.2
* Clang/LLVM 22.1
* CMake 4.4
* Ninja 1.13
* GDB 17.2
* LLDB 22.1
* Python 3.14
* pip 26.2

---

## 3. Python / DSP

Installed:

```fish
sudo pacman -S --needed \
  python-numpy python-scipy python-matplotlib python-pandas \
  python-numba python-sympy python-soundfile python-librosa \
  python-pyaudio python-mido python-pyqt6
```

DSP Venv:

```fish
python -m venv --system-site-packages ~/Dev/venvs/dsp
source ~/Dev/venvs/dsp/bin/activate
pip install sounddevice jupyterlab python-osc
```

Venv path:

```text
~/Dev/venvs/dsp
```

`python-osc` is installed in the venv.

---

## 4. Audio / PipeWire

Installed:

```fish
sudo pacman -S --needed \
  alsa-card-profiles alsa-firmware alsa-lib alsa-plugins \
  alsa-topology-conf alsa-ucm-conf alsa-utils \
  gst-plugin-pipewire libpipewire libsrtp libwireplumber \
  pavucontrol pipewire pipewire-alsa pipewire-audio \
  pipewire-jack pipewire-pulse realtime-privileges \
  vlc-plugin-alsa vlc-plugin-jack wireplumber
```

Services verified:

```fish
systemctl --user status pipewire
systemctl --user status wireplumber
```

PipeWire and WirePlumber are working.

The user was added to the `realtime` group.

Verifications:

```fish
ulimit -r
ulimit -l
wpctl status
pactl info
```

Result:

```text
ulimit -r = 98
ulimit -l = unlimited
```

---

## 5. Low latency

Configuration tested and working:

```text
Sample rate: 48 kHz
Quantum:     64
```

Setting used:

```fish
pw-metadata -n settings 0 clock.quantum 64
```

At 48 kHz / quantum 64:

```text
about 1.33 ms per buffer
```

The quantum 64 mode was tested and proved to be stable.

---

## 6. JACK / Pro Audio

Installed:

```fish
sudo pacman -S --needed \
  carla qpwgraph jack-example-tools jack_delay jaaa \
  jack_utils a2jmidid mididings
```

Verified:

* Carla
* qpwgraph
* jack_lsp
* jack_iodelay
* jack_delay
* jaaa
* a2jmidid
* pw-jack
* pw-top
* aconnect
* aseqdump

PipeWire JACK is working.

Virtual MIDI verified with:

```fish
aconnect -l
aseqdump -l
```

No physical MIDI hardware is present.

---

## 7. Audio plugins

Installed:

```fish
sudo pacman -S --needed \
  lv2 ladspa calf lsp-plugins x42-plugins zam-plugins iempluginsuite
```

Present:

* LV2
* LADSPA
* Calf
* LSP Plugins
* x42 Plugins
* Zam Plugins
* IEM Plugin Suite

The `pro-audio` group from Arch/CachyOS was also installed:

```fish
sudo pacman -S --needed (pacman -Sgq pro-audio)
```

This brought in practically the entire pro-audio stack available in the repositories.

---

## 8. Sampler

Installed:

```fish
sudo pacman -S --needed \
  linuxsampler qsampler samplv1-lv2 samplv1-standalone
```

---

## 9. SuperCollider

Installed:

```fish
sudo pacman -S --needed supercollider sc3-plugins
```

Version:

```text
SuperCollider 3.14.1
```

Verified with:

```fish
sclang -v
scsynth -v
```

---

## 10. Live coding

Installed:

```fish
sudo pacman -S --needed \
  haskell-tidal sonic-pi foxdot overtone faust \
  puredata supercollider sc3-plugins csound csound-plugins \
  hydra orca chuck
```

### Sonic Pi

Installed from AUR:

```text
sonic-pi 5.0.0-1
```

Completed the build with `makepkg`. Sonic Pi installed correctly.

### FoxDot

Version:

```text
foxdot 0.8.12-3
```

Installed via AUR. FoxDot installed correctly.

### Overtone

Upstream repository:

```text
~/Dev/overtone
```

Installed:

```fish
sudo pacman -S --needed clojure leiningen jdk21-openjdk
sudo archlinux-java set java-21-openjdk
```

Dependencies:

```fish
clojure -P
```

Test:

```fish
clojure -M -e '(require (quote overtone.live))'
```

Boot succeeded with:

```text
scsynth
PipeWire JACK
48 kHz
block size 64
SuperCollider 3.14.1
Overtone 0.16.3331
```

---

## 11. Languages

Installed:

```text
rustup
go
go-tools
lua
luarocks
ruby
ruby-bundler
sbcl
racket
guile
zig
nim
perl
php
nodejs
npm
```

Java:

```text
jdk21-openjdk
```

Java 21 set as default:

```fish
sudo archlinux-java set java-21-openjdk
```

---

## 12. LSP / Developer tooling

Installed:

```fish
sudo pacman -S --needed \
  clang-tools-extra \
  rust-analyzer \
  gopls \
  lua-language-server \
  python-lsp-server \
  marksman
```

Present:

* clangd / clang tools
* rust-analyzer
* gopls
* lua-language-server
* python-lsp-server
* marksman

---

## 13. VS Code / VSCodium

Installed from AUR:

```fish
shelly install aur visual-studio-code-bin vscodium-bin
```

Available commands:

```text
code
codium
```

---

## 14. PlugData

Installed from AUR:

```fish
shelly install aur plugdata-bin
```

Pure Data is already installed.

---

## 15. REAPER

Installed:

```fish
sudo pacman -S --needed reaper
```

Version:

```text
REAPER 7.79
```

Support present:

```text
lua
luarocks
liblo
portaudio
jack-example-tools
```

---

## 16. Spatial Audio

Present:

* ambix-lv2
* SSR
* sound-gambit
* IEM Plugin Suite
* IEM standalone
* IEM VST3

Search performed:

```fish
pacman -Sgq pro-audio | grep -Ei 'ambi|spat|spatial|surround|hoa|iem|ssr'
```

---

## 17. Virtualization / KVM / libvirt

Installed:

```fish
sudo pacman -S --needed \
  qemu-desktop \
  libvirt \
  virt-manager \
  virt-viewer \
  edk2-ovmf \
  dnsmasq \
  iptables-nft \
  swtpm \
  spice \
  spice-gtk \
  spice-protocol \
  virtiofsd
```

Service:

```fish
sudo systemctl enable --now libvirtd
```

Groups:

```fish
sudo usermod -aG libvirt,kvm $USER
```

KVM/libvirt are working.

---

## 18. libvirt network

NAT network:

```text
default
```

Bridge:

```text
virbr0
```

Gateway:

```text
192.168.122.1
```

Subnet:

```text
192.168.122.0/24
```

DHCP range:

```text
192.168.122.2 - 192.168.122.254
```

Network made persistent:

```fish
sudo virsh net-start default
sudo virsh net-autostart default
```

Final state:

```text
Name:       default
Active:     yes
Persistent: yes
Autostart:  yes
Bridge:     virbr0
```

---

## 19. Windows 10 VM (Win10VM)

VM created with Virt-Manager.

Configuration:

```text
Name:       Win10VM
OS:         Windows 10 Pro 64-bit
RAM:        8 GiB
CPU:        4 vCPU
Disk:       100 GB
Firmware:   UEFI / OVMF
Disk bus:   SATA
Network:    libvirt default
NIC:        e1000e
Display:    SPICE
```

Windows 10 22H2 installed.

During installation:

* No product key
* Windows 10 Pro
* Custom installation
* Local configuration
* Initially offline network

---

## 20. VM Disk Configuration (SATA)

The VirtIO disk did not appear during the initial Windows installation. 

`virtio-win.iso` was mounted to provide the drivers, but the disk did not become easily available. Therefore, the disk bus was set to:

```text
SATA
```

The VM works correctly with this configuration.

**Note:** Do not change the disk bus without a specific reason.

---

## 21. VM Network & Firewall Configuration

The VM network initially failed to obtain a DHCP lease and resolve DNS due to UFW blocking routed traffic and bridge-specific ports.

The issue was resolved by configuring UFW to allow routed traffic and explicitly permitting DHCP and DNS on the `virbr0` interface.

Applied UFW rules:

```fish
sudo ufw default allow routed
sudo ufw route allow in on virbr0
sudo ufw allow in on virbr0 to any port 67 proto udp
sudo ufw allow in on virbr0 to any port 53 proto udp
sudo ufw allow in on virbr0 to any port 53 proto tcp
sudo ufw reload
```

Final network state:

```text
DHCP              ✅
IP 192.168.122.x  ✅
Gateway           ✅
Internet          ✅
DNS               ✅
```

Configuration flow:

```text
Windows
   ↓
e1000e
   ↓
virbr0
   ↓
192.168.122.1
   ↓
libvirt NAT
   ↓
UFW forwarding
   ↓
Internet
```

The VM was restarted after configuration and the network was made persistent.

---

## 22. Useful commands for the VM

List VMs:

```fish
virsh list --all
```

Network status:

```fish
sudo virsh net-info default
```

DHCP leases:

```fish
sudo virsh net-dhcp-leases default
```

Interface:

```fish
sudo virsh domiflist Win10VM
```

Addresses:

```fish
sudo virsh domifaddr Win10VM
```

UFW:

```fish
sudo ufw status verbose
```

UFW rules:

```fish
sudo ufw status numbered
```

Forwarding:

```fish
sudo iptables -L FORWARD -n -v --line-numbers
```

---

## 23. Pending VM Configurations

To be done eventually:

* [ ] USB passthrough
* [ ] Shared folder Linux → Windows
* [ ] Shared folder Windows → Linux
* [ ] Audio PipeWire → Windows
* [ ] Video/graphics optimization
* [ ] VirtIO guest tools where useful
* [ ] VM snapshots
* [ ] VM backup
* [ ] Specific USB devices passthrough
* [ ] Max/MSP configuration

---

## 24. Configuration philosophy

The machine was set up as a Linux workstation oriented towards:

* Audio
* DSP
* Live coding
* Programming
* C/C++
* Python
* MIDI
* OSC
* SuperCollider
* JACK/PipeWire
* REAPER
* Spatial audio
* Virtualization
* Windows for specific software

The general stack choice is:

```text
CachyOS
   +
Hyprland
   +
Fish
   +
PipeWire
   +
JACK compatibility
   +
Low latency 48k / quantum 64
   +
Full pro-audio stack
   +
KVM/libvirt
   +
Windows 10 VM
```

---

## 25. Fish shell note

The commands in this document are designed for:

```text
Fish shell
```

Avoid automatically converting them to Bash.

In particular, do not use Bash heredocs like:

```bash
cat <<EOF
...
EOF
```

when working directly on the machine.

---

## 26. Overall state

### Linux System

* [x] CachyOS
* [x] Hyprland
* [x] Fish
* [x] Toolbox
* [x] C/C++
* [x] Python/DSP
* [x] LSP
* [x] Rust
* [x] Go
* [x] Lua
* [x] Ruby
* [x] Lisp/Scheme/Racket
* [x] Zig
* [x] Nim
* [x] Perl
* [x] PHP
* [x] Node.js

### Audio

* [x] PipeWire
* [x] WirePlumber
* [x] ALSA
* [x] JACK compatibility
* [x] Low latency quantum 64
* [x] Carla
* [x] qpwgraph
* [x] LV2
* [x] LADSPA
* [x] LSP
* [x] Calf
* [x] x42
* [x] Zam
* [x] IEM
* [x] SuperCollider
* [x] SC3 Plugins
* [x] Sonic Pi
* [x] FoxDot
* [x] Overtone
* [x] Tidal
* [x] Faust
* [x] Pure Data
* [x] PlugData
* [x] CSound
* [x] REAPER
* [x] LinuxSampler
* [x] SamplV1
* [x] Spatial audio stack

### Network & Virtualization

* [x] SSH
* [x] libvirt NAT
* [x] UFW
* [x] VM Internet access
* [x] QEMU
* [x] KVM
* [x] libvirt
* [x] Virt-Manager
* [x] OVMF/UEFI
* [x] SPICE
* [x] Win10VM
* [ ] USB passthrough
* [ ] Shared folders
* [ ] Audio VM
* [ ] Snapshots
* [ ] Backup VM

---

# End of history

**Current state: Linux machine ready as an audio/dev workstation and Windows VM operational with Internet.**
