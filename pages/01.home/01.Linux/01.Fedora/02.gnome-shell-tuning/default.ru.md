---
title: 'Gnome-shell tuning'
taxonomy:
    category:
        - docs
horizontal: false
shortcodes: true
---

### Installing the required packages

```
packs=(
  gawk                    # The GNU version of the AWK text processing utility
  git                     # Fast Version Control System  
  glib2-devel             # The glib2-devel package includes the header files for the GLib library.
  gnome-extensions-app    # GNOME Extensions is an application for configuring and removing GNOME Shell extensions.
  gnome-menus             # A menu system for the GNOME project
  gnome-themes-extra
  gnome-tweaks
  GraphicsMagick          # An ImageMagick fork, offering faster image generation and better quality
  gtk2-engines
  gtk-murrine-engine
  gtk-update-icon-cache
  ImageMagick             # An X application for displaying and manipulating images
  inkscape
  libgnome-devel
  libgnomekbd-devel
  libgnome-keyring-devel
  libgnomeui-devel
  libpng-devel
  libX11-devel
  libXcursor-devel
  python37
  python3-pip
  xorg-x11-apps
  xorg-x11-utils
)

sudo dnf install ${packs[@]}

```

### Installing gnome-shell themes from packages extensions

```
packs=(
  gnome-shell-extension-background-logo # Background logo extension for GNOME Shell
  gnome-shell-extension-common          # Files common to GNOME Shell Extensions
  gnome-shell-extension-dash-to-dock    # Dock for the Gnome Shell by micxgx@gmail.com
  gnome-shell-extension-drive-menu      # Drive status menu for GNOME Shell
  gnome-shell-extension-freon           # GNOME Shell extension to display system temperature, voltage, and fan speed
  gnome-shell-extension-openweather     # Display weather information from many locations in the world
  gnome-shell-extension-user-theme      # Support for custom themes in GNOME Shell
  flat-remix-gtk3-theme                 # Flat Remix GTK theme is a pretty simple GTK window theme inspired on material design following a modern design using "flat" colors with high contrasts and sharp borders.
  flat-remix-theme                      # Flat Remix GTK theme is a pretty simple GTK window theme inspired on material design following a modern design using "flat" colors with high contrasts and sharp borders.
)

sudo dnf install ${packs[@]}
```

### Installing gnome-shell icons from external packages extensions

```
sudo dnf copr enable peterwu/rendezvous
sudo dnf install bibata-cursor-theme
```
# SYSCTL.CONF

## Tuning TCP stack

### Sysctl parameters that allow you to increase the size of the ARP cache

`Installation using parameters in a file, for example:`

```bash
#!/bin/env bash

  if [[ ! -f /etc/sysctl.d/99-main-sysctl.conf ]]; then
    cat <<EOF | sudo tee /etc/sysctl.d/99-main-sysctl.conf >/dev/null
# gc_thresh1 — the minimum number of entries that should be in the ARP cache.
# If the number of entries is less than this value, the garbage collector will not clear the ARP cache.
# (def. 128)
net.ipv4.neigh.default.gc_thresh1 = 1024
# gc_thresh2 - soft limit on the number of entries in the ARP cache.
# If the number of entries reaches this value, the garbage collector starts within 5 seconds.
# (def. 512)
net.ipv4.neigh.default.gc_thresh2 = 2048
# gc_thresh3 - hard limit on the number of entries in the ARP cache.
# If the number of records reaches this value, the garbage collector starts immediately.
# (def. 1024)
net.ipv4.neigh.default.gc_thresh3 = 4096
EOF
  fi

  # Return the values of variables to the state saved in files.
  sudo sysctl --system

```

`or or using the sysctl utility:`

```bash
#!/bin/env bash

  # gc_thresh1 — the minimum number of entries that should be in the ARP cache.
  # If the number of entries is less than this value, the garbage collector will not clear the ARP cache.
  # (def. 128)
  sudo sysctl -w net.ipv4.neigh.default.gc_thresh1=1024

  # gc_thresh2 - soft limit on the number of entries in the ARP cache.
  # If the number of entries reaches this value, the garbage collector starts within 5 seconds.
  # (def. 512)
  sudo sysctl -w net.ipv4.neigh.default.gc_thresh2=2048

  # gc_thresh3 - hard limit on the number of entries in the ARP cache.
  # If the number of records reaches this value, the garbage collector starts immediately.
  # (def. 1024)
  sudo sysctl -w net.ipv4.neigh.default.gc_thresh3=4096

  # Return the values of variables to the state saved in files and delete your changes.
  # sudo sysctl --system

```
