# CachyOS / ThinkPad X13 — Storico configurazione 2026

> Storico operativo della configurazione fatta finora.
> Sistema: CachyOS rolling + Hyprland
> Shell: Fish
> Hardware: ThinkPad X13, Ryzen 5 PRO 4650U, 32 GiB RAM, Radeon Vega/Renoir iGPU.

---

## 1. Toolbox

Installato:

```fish
sudo pacman -S --needed \
  base-devel git curl wget unzip zip tar rsync tree file which \
  man-db man-pages less nano neovim htop btop mc zellij tmux \
  fzf ripgrep fd bat eza ncdu jq fastfetch procs dust duf openssh lazygit
```

Tool principali:

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

## 2. Toolchain C/C++

Installato:

```fish
sudo pacman -S --needed \
  gcc clang llvm cmake ninja gdb lldb make meson ccache pkgconf \
  autoconf automake libtool python-pip python-virtualenv
```

Versioni principali verificate:

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

Installato:

```fish
sudo pacman -S --needed \
  python-numpy python-scipy python-matplotlib python-pandas \
  python-numba python-sympy python-soundfile python-librosa \
  python-pyaudio python-mido python-pyqt6
```

Venv DSP:

```fish
python -m venv --system-site-packages ~/Dev/venvs/dsp
source ~/Dev/venvs/dsp/bin/activate
pip install sounddevice jupyterlab python-osc
```

Venv:

```text
~/Dev/venvs/dsp
```

`python-osc` è installato nel venv.

---

# 4. Audio / PipeWire

Installato:

```fish
sudo pacman -S --needed \
  alsa-card-profiles alsa-firmware alsa-lib alsa-plugins \
  alsa-topology-conf alsa-ucm-conf alsa-utils \
  gst-plugin-pipewire libpipewire libsrtp libwireplumber \
  pavucontrol pipewire pipewire-alsa pipewire-audio \
  pipewire-jack pipewire-pulse realtime-privileges \
  vlc-plugin-alsa vlc-plugin-jack wireplumber
```

Servizi verificati:

```fish
systemctl --user status pipewire
systemctl --user status wireplumber
```

PipeWire e WirePlumber funzionanti.

L'utente è stato aggiunto al gruppo `realtime`.

Verifiche:

```fish
ulimit -r
ulimit -l
wpctl status
pactl info
```

Risultato:

```text
ulimit -r = 98
ulimit -l = unlimited
```

---

# 5. Low latency

Configurazione testata e funzionante:

```text
Sample rate: 48 kHz
Quantum:     64
```

Impostazione usata:

```fish
pw-metadata -n settings 0 clock.quantum 64
```

A 48 kHz / quantum 64:

```text
circa 1.33 ms per buffer
```

La modalità a quantum 64 è stata provata realmente ed è risultata stabile.

---

# 6. JACK / Pro Audio

Installato:

```fish
sudo pacman -S --needed \
  carla qpwgraph jack-example-tools jack_delay jaaa \
  jack_utils a2jmidid mididings
```

Verificati:

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

PipeWire JACK funzionante.

MIDI virtuale verificato con:

```fish
aconnect -l
aseqdump -l
```

Non è presente hardware MIDI fisico.

---

# 7. Plugin audio

Installato:

```fish
sudo pacman -S --needed \
  lv2 ladspa calf lsp-plugins x42-plugins zam-plugins iempluginsuite
```

Presenti:

* LV2
* LADSPA
* Calf
* LSP Plugins
* x42 Plugins
* Zam Plugins
* IEM Plugin Suite

È stato inoltre installato il gruppo `pro-audio` di Arch/CachyOS:

```fish
sudo pacman -S --needed (pacman -Sgq pro-audio)
```

Questo ha portato dentro praticamente l'intero stack pro-audio disponibile nei repository.

---

# 8. Sampler

Installato:

```fish
sudo pacman -S --needed \
  linuxsampler qsampler samplv1-lv2 samplv1-standalone
```

---

# 9. SuperCollider

Installato:

```fish
sudo pacman -S --needed supercollider sc3-plugins
```

Versione:

```text
SuperCollider 3.14.1
```

Verificato con:

```fish
sclang -v
scsynth -v
```

---

# 10. Live coding

Installato:

```fish
sudo pacman -S --needed \
  haskell-tidal sonic-pi foxdot overtone faust \
  puredata supercollider sc3-plugins csound csound-plugins \
  hydra orca chuck
```

## Sonic Pi

Installato da AUR:

```text
sonic-pi 5.0.0-1
```

Durante il build AUR ci sono stati problemi con download GitHub.

È stato necessario usare:

```fish
set -gx CURL_HTTP_VERSION 1.1
```

e completare il build con `makepkg`.

Sonic Pi installato correttamente.

---

## FoxDot

Versione:

```text
foxdot 0.8.12-3
```

Il download AUR iniziale dava:

```text
http chunk truncated
```

Download manuale:

```fish
set -gx CURL_HTTP_VERSION 1.1

curl --http1.1 -L --retry 10 --retry-delay 3 --retry-all-errors \
  -o foxdot-0.8.12.tar.gz \
  https://github.com/Qirky/foxdot/archive/refs/tags/v0.8.12.tar.gz
```

Poi:

```fish
makepkg -si
```

FoxDot installato.

---

## Overtone

Repository upstream:

```text
~/Dev/overtone
```

Installato:

```fish
sudo pacman -S --needed clojure leiningen jdk21-openjdk
sudo archlinux-java set java-21-openjdk
```

Dipendenze:

```fish
clojure -P
```

Test:

```fish
clojure -M -e '(require (quote overtone.live))'
```

Avvio riuscito con:

```text
scsynth
PipeWire JACK
48 kHz
block size 64
SuperCollider 3.14.1
Overtone 0.16.3331
```

---

# 11. Linguaggi

Installati:

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

Java 21 impostato come default:

```fish
sudo archlinux-java set java-21-openjdk
```

---

# 12. LSP / Developer tooling

Installato:

```fish
sudo pacman -S --needed \
  clang-tools-extra \
  rust-analyzer \
  gopls \
  lua-language-server \
  python-lsp-server \
  marksman
```

Presenti:

* clangd / clang tools
* rust-analyzer
* gopls
* lua-language-server
* python-lsp-server
* marksman

---

# 13. VS Code / VSCodium

Installato da AUR:

```fish
shelly install aur visual-studio-code-bin vscodium-bin
```

Comandi disponibili:

```text
code
codium
```

---

# 14. PlugData

Installato da AUR:

```fish
shelly install aur plugdata-bin
```

Pure Data è già installato.

---

# 15. REAPER

Installato:

```fish
sudo pacman -S --needed reaper
```

Versione:

```text
REAPER 7.79
```

Supporto presente:

```text
lua
luarocks
liblo
portaudio
jack-example-tools
```

---

# 16. Spatial Audio

Presenti:

* ambix-lv2
* SSR
* sound-gambit
* IEM Plugin Suite
* IEM standalone
* IEM VST3

Ricerca effettuata:

```fish
pacman -Sgq pro-audio | grep -Ei 'ambi|spat|spatial|surround|hoa|iem|ssr'
```

---

# 17. Tailscale

Installato:

```fish
sudo pacman -S --needed tailscale
```

Servizio:

```fish
sudo systemctl enable --now tailscaled
```

Autenticazione:

```fish
sudo tailscale up
```

Tailscale funzionante.

Macchine rilevate, tra le altre:

```text
bagalur      100.120.232.126
```

SSH configurato in:

```text
~/.ssh/config
```

con:

```text
Host bagalur
    HostName 100.120.232.126
    User bagalur
```

SSH verso `bagalur` funziona.

Gitea è già presente su `bagalur`.

### Nota MagicDNS

Tailscale segnala un problema con l'integrazione `systemd-resolved` / NetworkManager.

Non è stato corretto perché non serve: si utilizzano direttamente gli IP Tailscale.

---

# 18. Virtualizzazione / KVM / libvirt

Installato:

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

Servizio:

```fish
sudo systemctl enable --now libvirtd
```

Gruppi:

```fish
sudo usermod -aG libvirt,kvm $USER
```

KVM/libvirt funzionanti.

---

# 19. Rete libvirt

Rete NAT:

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

DHCP:

```text
192.168.122.2 - 192.168.122.254
```

Rete resa persistente:

```fish
sudo virsh net-start default
sudo virsh net-autostart default
```

Stato finale:

```text
Name:       default
Active:     yes
Persistent: yes
Autostart:  yes
Bridge:     virbr0
```

---

# 20. Windows 10 VM — WinNino

VM creata con Virt-Manager.

Configurazione:

```text
Nome:       WinNino
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

Windows 10 22H2 italiano installato.

Durante l'installazione:

* niente product key
* Windows 10 Pro
* installazione custom
* configurazione locale
* rete inizialmente offline

---

# 21. Perché il disco è SATA

Il disco VirtIO non compariva durante l'installazione.

È stato montato `virtio-win.iso` per provare a fornire i driver, ma il disco non diventava disponibile in modo semplice.

È stato quindi scelto:

```text
SATA
```

La VM funziona correttamente così.

**Non cambiare il bus del disco senza motivo.**

---

# 22. Problema rete WinNino — DHCP

All'inizio Windows riceveva un indirizzo:

```text
169.254.x.x
```

quindi non riceveva DHCP.

Il test `tcpdump` mostrava che Windows inviava:

```text
0.0.0.0:68 → 255.255.255.255:67
```

ma non riceveva DHCP Offer.

È stata aggiunta temporaneamente:

```fish
sudo iptables -I INPUT 1 -i virbr0 -p udp --dport 67 -j ACCEPT
```

Dopo questa modifica Windows ha ottenuto:

```text
192.168.122.236/24
```

Lease verificato:

```fish
sudo virsh net-dhcp-leases default
```

Risultato:

```text
MAC:
52:54:00:9a:3f:b8

IP:
192.168.122.236/24

Hostname:
DESKTOP-C2BGA3U
```

---

# 23. Problema rete WinNino — Forwarding

Dopo il DHCP:

```text
ping 192.168.122.1  → OK
ping 1.1.1.1        → OK
ping google.com     → FAIL
```

Il DNS di libvirt era in ascolto:

```text
192.168.122.1:53
```

verificato con:

```fish
sudo ss -lunpt | grep ':53'
```

Il `tcpdump` mostrava richieste DNS della VM:

```text
192.168.122.236 → 8.8.8.8:53
192.168.122.236 → 1.1.1.1:53
```

ma nessuna risposta tornava.

Il problema era il forwarding bloccato da UFW.

Configurazione iniziale UFW:

```text
Status: active
Default:
deny incoming
allow outgoing
deny routed
```

---

# 24. Test temporaneo del forwarding

Per confermare il problema sono state aggiunte temporaneamente:

```fish
sudo iptables -I FORWARD 1 \
  -i virbr0 \
  -s 192.168.122.0/24 \
  -j ACCEPT

sudo iptables -I FORWARD 2 \
  -o virbr0 \
  -d 192.168.122.0/24 \
  -m conntrack \
  --ctstate ESTABLISHED,RELATED \
  -j ACCEPT
```

Dopo questo:

```text
ping google.com → OK
```

Problema confermato.

---

# 25. Fix permanente UFW

È stato impostato:

```fish
sudo ufw default allow routed
```

Forwarding da `virbr0`:

```fish
sudo ufw route allow in on virbr0
```

DHCP:

```fish
sudo ufw allow in on virbr0 to any port 67 proto udp
```

DNS UDP:

```fish
sudo ufw allow in on virbr0 to any port 53 proto udp
```

DNS TCP:

```fish
sudo ufw allow in on virbr0 to any port 53 proto tcp
```

Ricarica:

```fish
sudo ufw reload
```

Regole finali UFW:

```text
ALLOW FWD  Anywhere on virbr0
67/udp     ALLOW IN on virbr0
53/udp     ALLOW IN on virbr0
53/tcp     ALLOW IN on virbr0
```

Le regole `iptables` temporanee sono state rimosse:

```fish
sudo iptables -D FORWARD 2
sudo iptables -D FORWARD 1
```

---

# 26. Stato finale WinNino

La rete ora funziona:

```text
DHCP              ✅
IP 192.168.122.x  ✅
Gateway           ✅
Internet          ✅
DNS               ✅
google.com        ✅
```

Configurazione:

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

La VM è stata riavviata dopo la configurazione e la rete è stata resa persistente.

---

# 27. Comandi utili per la VM

Lista VM:

```fish
virsh list --all
```

Stato rete:

```fish
sudo virsh net-info default
```

Lease DHCP:

```fish
sudo virsh net-dhcp-leases default
```

Interfaccia:

```fish
sudo virsh domiflist WinNino
```

Indirizzi:

```fish
sudo virsh domifaddr WinNino
```

UFW:

```fish
sudo ufw status verbose
```

Regole UFW:

```fish
sudo ufw status numbered
```

Forwarding:

```fish
sudo iptables -L FORWARD -n -v --line-numbers
```

---

# 28. Cosa manca su WinNino

Da fare eventualmente:

* [ ] USB passthrough
* [ ] cartella condivisa Linux → Windows
* [ ] eventualmente Windows → Linux
* [ ] audio PipeWire → Windows
* [ ] ottimizzazione video/grafica
* [ ] VirtIO guest tools dove utili
* [ ] snapshot VM
* [ ] backup VM
* [ ] eventuali dispositivi USB specifici
* [ ] configurazione Max/MSP

## Max/MSP

L'installazione di Max/MSP viene lasciata all'utente.

---

# 29. Filosofia della configurazione

La macchina è stata impostata come workstation Linux orientata a:

* audio
* DSP
* live coding
* programmazione
* C/C++
* Python
* MIDI
* OSC
* SuperCollider
* JACK/PipeWire
* REAPER
* spatial audio
* virtualizzazione
* Windows per software specifici

La scelta generale è:

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
low latency 48k / quantum 64
   +
full pro-audio stack
   +
KVM/libvirt
   +
WinNino
```

---

# 30. Nota Fish

I comandi di questo documento sono pensati per:

```text
Fish shell
```

Evitare di convertirli automaticamente in Bash.

In particolare, non usare Bash heredoc tipo:

```bash
cat <<EOF
...
EOF
```

quando si lavora direttamente sulla macchina.

---

# 31. Stato complessivo

## Sistema Linux

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

## Audio

* [x] PipeWire
* [x] WirePlumber
* [x] ALSA
* [x] JACK compatibility
* [x] low latency quantum 64
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

## Network

* [x] Tailscale
* [x] SSH
* [x] Gitea access
* [x] libvirt NAT
* [x] UFW
* [x] WinNino Internet

## Virtualizzazione

* [x] QEMU
* [x] KVM
* [x] libvirt
* [x] Virt-Manager
* [x] OVMF/UEFI
* [x] SPICE
* [x] WinNino
* [ ] USB passthrough
* [ ] shared folders
* [ ] audio VM
* [ ] snapshots
* [ ] backup VM

---

# Fine storico

**Stato attuale: macchina Linux pronta come workstation audio/dev e WinNino operativo con Internet.**

Max/MSP verrà installato separatamente.
