# CachyOS — General Configuration & Setup History

> General configuration history for two CachyOS systems.
>
> This document intentionally contains **no personal names, usernames, hostnames, passwords, tokens, email addresses, account identifiers, or other credentials**.
>
> The software/configuration sections describe the reusable CachyOS workstation setup. Hardware-specific details are kept only in the machine profiles at the beginning.

---

## 0. Machine Profiles

### Computer A — Current System

- OS: CachyOS Rolling
- Kernel: CachyOS kernel 7.2.6-1
- Session: Wayland
- Desktop currently active: COSMIC
- Hyprland: installed and available
- Shell: Fish
- CPU: AMD Ryzen 7 5700U, 8 cores / 16 threads
- Memory: 12 GiB RAM + 12 GiB swap
- GPU: AMD Radeon Graphics / Lucienne
- Audio: AMD Renoir/Cezanne HDMI/DP + Ryzen HD Audio
- PipeWire/WirePlumber: active
- Realtime limits: `ulimit -r = 98`, `ulimit -l = unlimited`
- UFW: active
- KVM/libvirt: not installed/active on this system

### Computer B — Reference Configuration

- OS: CachyOS Rolling
- Desktop/compositor: Hyprland
- Shell: Fish
- CPU: AMD Ryzen 5 PRO 4650U
- Memory: 32 GiB RAM
- GPU: Radeon Vega/Renoir iGPU
- This system contains the original KVM/libvirt + Windows VM configuration described later.

Hardware profiles are reference information only. The remainder of this document describes the software stack and configuration in general.

---

## 1. Toolbox

Installed / used:

```fish
sudo pacman -S --needed \
  base-devel git curl wget unzip zip tar rsync tree file which \
  man-db man-pages less nano neovim htop btop mc zellij tmux \
  fzf ripgrep fd bat eza ncdu jq fastfetch procs dust duf openssh lazygit
```

Additional utilities present on the current system include:

- glances
- inxi
- 7zip
- btrfs-assistant
- btrfs-progs
- fsarchiver
- yay
- Alacritty
- Kitty

Main tools:

- Git
- Neovim
- htop / btop / glances
- Midnight Commander
- Zellij / tmux
- fzf / ripgrep / fd
- bat / eza
- lazygit
- OpenSSH
- fastfetch / inxi

---

## 2. Shell / Desktop

### Fish

Fish is the primary shell.

CachyOS Fish configuration is installed.

The commands in this document are designed for Fish unless explicitly marked otherwise.

### COSMIC

Computer A currently runs:

```text
COSMIC
Wayland
```

COSMIC components are installed, including the session, compositor, panel, launcher, settings, terminal, files, monitor and related desktop services.

### Hyprland

Hyprland is also installed and available:

```text
hyprland
hyprland-guiutils
hyprcursor
hyprgraphics
hyprlang
hyprpicker
hyprtoolkit
hyprutils
hyprwire
```

The original configuration used Hyprland as the primary compositor.

### Terminals

Installed terminals include:

- Alacritty
- Kitty
- COSMIC Terminal

---

## 3. C/C++ Toolchain

Installed:

```fish
sudo pacman -S --needed \
  gcc clang llvm cmake ninja gdb lldb make meson ccache pkgconf \
  autoconf automake libtool python-pip python-virtualenv
```

Current verified versions:

- GCC 16.2
- Clang/LLVM 22.1
- CMake 4.4
- Ninja 1.13
- GDB 17.2
- LLDB 22.1
- Python 3.14
- pip 26.2

Additional compiler/runtime components present include:

- Clang 21 compatibility packages
- LLVM compiler runtimes
- lib32 compiler/runtime libraries
- binutils
- bison
- flex
- pkgconf
- ccache

---

## 4. Python / DSP

Installed:

```fish
sudo pacman -S --needed \
  python-numpy python-scipy python-matplotlib python-pandas \
  python-numba python-sympy python-soundfile python-librosa \
  python-pyaudio python-mido python-pyqt6
```

The current system also contains useful DSP/scientific packages including:

- scikit-learn
- python-rtmidi
- python-pyliblo3
- python-audioread
- python-soxr
- python-resampy
- Python/PyQt5
- Python/PyQt6
- pandas
- NumPy
- SciPy
- Matplotlib
- Numba
- SymPy
- librosa

### DSP Venv

Original DSP environment:

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

No personal username or home-directory identity should be written into documentation.

---

## 5. Audio / PipeWire

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

Current system:

- PipeWire active
- WirePlumber active
- ALSA available
- PipeWire JACK compatibility available
- PulseAudio compatibility available
- Pavucontrol installed
- GStreamer PipeWire integration installed

Verification:

```fish
systemctl --user status pipewire
systemctl --user status wireplumber
wpctl status
pactl info
```

---

## 6. Low Latency

Target configuration:

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

The quantum 64 configuration was tested as stable on the reference configuration.

Realtime limits:

```text
ulimit -r = 98
ulimit -l = unlimited
```

The `realtime` group is present.

---

## 7. JACK / Pro Audio

Core tools:

```fish
sudo pacman -S --needed \
  carla qpwgraph jack-example-tools jack_delay jaaa \
  jack_utils a2jmidid mididings
```

Additional JACK/MIDI tools currently present include:

- QjackCtl
- jack_mixer
- jackmeter
- jackminimix
- jacktrip
- japa
- jconvolver
- jack-stdio
- zita-ajbridge
- zita-alsa-pcmi
- zita-convolver
- zita-resampler
- a2jmidid
- aconnect
- aseqdump
- qmidiarp
- qmidictl
- qmidinet
- qmidiroute
- kmidimon
- QSynth

PipeWire JACK is the preferred compatibility layer.

Virtual MIDI was verified on the reference configuration with:

```fish
aconnect -l
aseqdump -l
```

---

## 8. Audio Plugins

The system contains a substantially larger audio plugin collection than the original configuration.

Core formats:

- LV2
- LADSPA
- VST
- VST3
- CLAP
- DSSI

Core collections:

- Calf
- LSP Plugins
- x42 Plugins
- Zam Plugins
- IEM Plugin Suite
- Distrho Ports
- DPF Plugins
- Dragonfly Reverb
- Zita tools/plugins

Additional plugin/instrument collections currently present include:

- ADLPlug
- AIDA-X
- AMSynth
- ArtyFX
- Aeolus
- Cardinal
- Dexed
- DrumGizmo
- Drumkv1
- Fabla
- Geonkick
- GMSynth
- Hydrogen
- JS80P
- Yoshimi
- ZynAddSubFX
- padthv1
- infamousplugins
- blop
- bshapr
- bslizr
- caps
- fomp
- g2reverb
- guitarix
- freeverb3
- EQ10Q
- ebumeter
- ecasound
- amb-plugins
- ambix
- jconvolver

The `pro-audio` group from Arch/CachyOS was installed on the original configuration:

```fish
sudo pacman -S --needed (pacman -Sgq pro-audio)
```

This provides a broad repository-level pro-audio stack.

---

## 9. Synths / Samplers / Instruments

Installed/currently present:

- LinuxSampler
- QSampler
- SamplV1
- Drumkv1
- Padthv1
- Yoshimi
- ZynAddSubFX
- Dexed
- JS80P
- AMSynth
- Aeolus
- FluidSynth
- QSynth
- Cardinal
- Geonkick
- Hydrogen
- Bespoke Synth

Original sampler configuration:

```fish
sudo pacman -S --needed \
  linuxsampler qsampler samplv1-lv2 samplv1-standalone
```

---

## 10. SuperCollider

Installed:

```fish
sudo pacman -S --needed supercollider sc3-plugins
```

Original verified version:

```text
SuperCollider 3.14.1
```

Verification:

```fish
sclang -v
scsynth -v
```

---

## 11. Live Coding / Computer Music

Installed/currently present:

```text
haskell-tidal
sonic-pi
foxdot
overtone
faust
puredata
supercollider
sc3-plugins
csound
csound-plugins
hydra
orca
chuck
```

Additional music/composition software present on the current system includes:

- Ardour
- Audacity
- LilyPond
- Furnace
- Bespoke Synth
- Hydrogen
- Guitarix
- Freewheeling

### Sonic Pi

Original installation:

```text
sonic-pi 5.0.0-1
```

### FoxDot

Original installation:

```text
foxdot 0.8.12-3
```

### Overtone

Original upstream repository:

```text
Dev/overtone
```

Dependencies:

```fish
sudo pacman -S --needed clojure leiningen jdk21-openjdk
sudo archlinux-java set java-21-openjdk
```

Boot configuration verified with:

```text
scsynth
PipeWire JACK
48 kHz
block size 64
SuperCollider 3.14.1
Overtone 0.16.3331
```

---

## 12. Programming Languages

Installed/currently present:

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
ghc
clojure
```

Java:

```text
jdk17-openjdk
jdk21-openjdk
jre8-openjdk
```

Java 21 is the configured/default development JDK on the reference setup:

```fish
sudo archlinux-java set java-21-openjdk
```

The current system also has multiple Java runtimes available.

---

## 13. LSP / Developer Tooling

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

Main developer tooling:

- clangd / Clang tools
- rust-analyzer
- gopls
- Lua language server
- Python LSP server
- marksman
- Git
- lazygit
- CMake
- Ninja
- Meson
- GDB
- LLDB

---

## 14. Editors / IDEs

AUR packages currently present:

```text
visual-studio-code-bin
vscodium-bin
plugdata-bin
```

Available commands:

```text
code
codium
```

PlugData is installed on top of Pure Data.

---

## 15. REAPER / DAWs

Original configuration:

```fish
sudo pacman -S --needed reaper
```

Original verified version:

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

Additional DAWs/audio applications currently present:

- Ardour
- Audacity
- REAPER

---

## 16. Spatial Audio

Present:

- ambix
- ambix-lv2
- SSR-related components where available
- sound-gambit
- IEM Plugin Suite
- IEM standalone
- IEM VST3
- amb-plugins
- ambdec
- jconvolver
- Zita audio tools

Search used on the reference configuration:

```fish
pacman -Sgq pro-audio | grep -Ei 'ambi|spat|spatial|surround|hoa|iem|ssr'
```

---

## 17. Windows / Wine Integration

The current system contains:

```text
yabridge
```

This provides a bridge for integrating Windows VST/VST3 plugins into Linux audio workflows.

The exact Windows plugin inventory is intentionally not documented here.

---

## 18. Virtualization / KVM / libvirt

The reference configuration contains a complete KVM/libvirt stack:

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

KVM/libvirt worked on the reference configuration.

**Current system status:** KVM/libvirt is not installed/active in the captured configuration.

---

## 19. libvirt Network

Reference configuration:

```text
Name:       default
Active:     yes
Persistent: yes
Autostart:  yes
Bridge:     virbr0
Gateway:    192.168.122.1
Subnet:     192.168.122.0/24
DHCP range: 192.168.122.2 - 192.168.122.254
```

Network commands:

```fish
sudo virsh net-start default
sudo virsh net-autostart default
```

---

## 20. Windows 10 VM

Reference VM:

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

Windows 10 22H2 was installed.

The VM used SATA rather than VirtIO because the VirtIO disk was not immediately available during installation.

**Note:** Do not change the disk bus without a specific reason.

---

## 21. VM Network / Firewall

Reference configuration required UFW forwarding and bridge-specific rules.

Applied:

```fish
sudo ufw default allow routed
sudo ufw route allow in on virbr0
sudo ufw allow in on virbr0 to any port 67 proto udp
sudo ufw allow in on virbr0 to any port 53 proto udp
sudo ufw allow in on virbr0 to any port 53 proto tcp
sudo ufw reload
```

Final reference state:

```text
DHCP       working
Gateway    working
Internet   working
DNS        working
```

Current system firewall:

```text
UFW active
Default incoming: deny
Default outgoing: allow
Routed traffic: disabled
```

---

## 22. VM Useful Commands

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

## 23. Current System Utilities / Desktop Applications

Current installation additionally includes several general-purpose applications not present in the original document:

### Desktop / system

- COSMIC
- Hyprland
- Alacritty
- Kitty
- LightDM
- Btrfs Assistant
- FSArchiver
- ddcutil
- brightnessctl
- BlueZ
- Flatpak

### Browsers / communication

- Firefox
- Chromium
- Element

### Media / graphics

- VLC components
- Celluloid
- ImageMagick
- Ghostscript

### Development / assistant software

- Claude Desktop
- Visual Studio Code
- VSCodium

### Package management

- pacman
- yay
- CachyOS Package Installer
- CachyOS Kernel Manager

---

## 24. CachyOS Configuration

CachyOS-specific components currently present include:

```text
cachyos-settings
cachyos-fish-config
cachyos-alacritty-config
cachyos-ananicy-rules
cachyos-kernel-manager
cachyos-packageinstaller
cachyos-hooks
cachyos-snapper-support
cachyos-mirrorlist
cachyos-v3-mirrorlist
cachyos-v4-mirrorlist
cachyos-wallpapers
```

The active kernel on the current system is:

```text
linux-cachyos 7.2.6-1
```

An LTS CachyOS kernel is also installed:

```text
linux-cachyos-lts 6.18.52-1
```

---

## 25. Storage / Filesystem Tools

Current system includes:

```text
btrfs-progs
btrfs-assistant
fsarchiver
f2fs-tools
exfatprogs
dosfstools
parted
cryptsetup
dmraid
fuse3
```

CachyOS Snapper support is installed.

---

## 26. Pending VM Configurations

Reference configuration still listed these as future work:

- [ ] USB passthrough
- [ ] Shared folder Linux → Windows
- [ ] Shared folder Windows → Linux
- [ ] Audio PipeWire → Windows
- [ ] Video/graphics optimization
- [ ] VirtIO guest tools where useful
- [ ] VM snapshots
- [ ] VM backup
- [ ] Specific USB device passthrough
- [ ] Max/MSP configuration

These items apply to the reference virtualization setup and are not claimed as active on the current system.

---

## 27. Configuration Philosophy

The general CachyOS workstation stack is oriented towards:

- Audio
- DSP
- Live coding
- Programming
- C/C++
- Python
- MIDI
- OSC
- SuperCollider
- JACK/PipeWire
- REAPER
- Ardour
- Audacity
- Spatial audio
- Software instruments
- Plugin development/use
- Linux/Windows audio integration
- Optional virtualization

General architecture:

```text
CachyOS
   +
Fish
   +
Wayland
   +
COSMIC and/or Hyprland
   +
PipeWire
   +
JACK compatibility
   +
Low latency 48 kHz / quantum 64
   +
Full pro-audio stack
   +
LV2 / LADSPA / VST / VST3 / CLAP / DSSI
   +
C/C++ / Python / Rust / Go / Lua / JVM / other languages
   +
Optional Wine plugin bridge
   +
Optional KVM/libvirt
   +
Optional Windows VM
```

---

## 28. Privacy / Documentation Rules

This document is intentionally generic.

Do **not** add:

- real usernames
- hostnames
- real names
- email addresses
- passwords
- password-manager contents
- API keys
- tokens
- SSH private keys
- account IDs
- personal home-directory names
- personal file paths that expose identity

Use generic placeholders when a command requires a user-specific value:

```text
$USER
$HOME
~/...
```

Do not replace those placeholders with a real account name.

---

## 29. Overall State

### Linux System

- [x] CachyOS
- [x] Fish
- [x] COSMIC
- [x] Hyprland
- [x] Toolbox
- [x] C/C++
- [x] Python/DSP
- [x] LSP
- [x] Rust tooling
- [x] Go
- [x] Lua
- [x] Ruby
- [x] Lisp/Scheme/Racket
- [x] Zig
- [x] Nim
- [x] Perl
- [x] PHP
- [x] Node.js
- [x] Java
- [x] Haskell/Tidal
- [x] Clojure

### Audio

- [x] PipeWire
- [x] WirePlumber
- [x] ALSA
- [x] JACK compatibility
- [x] Low latency configuration
- [x] Carla
- [x] qpwgraph
- [x] QjackCtl
- [x] LV2
- [x] LADSPA
- [x] VST
- [x] VST3
- [x] CLAP
- [x] DSSI
- [x] LSP
- [x] Calf
- [x] x42
- [x] Zam
- [x] IEM
- [x] SuperCollider
- [x] SC3 Plugins
- [x] Sonic Pi
- [x] FoxDot
- [x] Overtone
- [x] Tidal
- [x] Faust
- [x] Pure Data
- [x] PlugData
- [x] CSound
- [x] REAPER
- [x] Ardour
- [x] Audacity
- [x] LinuxSampler
- [x] SamplV1
- [x] Spatial audio stack
- [x] Yabridge
- [x] Multiple software instruments/effects

### Development

- [x] GCC
- [x] Clang/LLVM
- [x] CMake
- [x] Ninja
- [x] Meson
- [x] GDB
- [x] LLDB
- [x] ccache
- [x] Git
- [x] lazygit
- [x] Python
- [x] Rust tooling
- [x] Go tooling
- [x] Java
- [x] Clojure
- [x] Haskell
- [x] VS Code
- [x] VSCodium

### Network / Security / System

- [x] SSH
- [x] UFW
- [x] BlueZ
- [x] Flatpak
- [x] Btrfs tools
- [x] Snapper support
- [x] CachyOS kernel
- [x] CachyOS configuration tools

### Virtualization

- [x] KVM/libvirt on reference configuration
- [x] QEMU on reference configuration
- [x] Virt-Manager on reference configuration
- [x] OVMF/UEFI on reference configuration
- [x] SPICE on reference configuration
- [x] Win10VM on reference configuration
- [ ] KVM/libvirt on current system
- [ ] USB passthrough
- [ ] Shared folders
- [ ] VM audio
- [ ] VM snapshots
- [ ] VM backup

---

# End of General Configuration History
