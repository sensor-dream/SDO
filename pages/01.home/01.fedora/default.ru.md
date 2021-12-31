---
title: FEDORA
---

# Gnome-shell tuning

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

# OS VERSION ID

    FILE_OS_RELEASE="/etc/os-release"
    OS_VERSION_ID="$(grep VERSION_ID ${FILE_OS_RELEASE} | awk -F= '{ print $2 }')"
    or
    OS_VERSION_ID="$(rpm -E %fedora)"
    echo "${OS_VERSION_ID}

# nginx configure output

`nginx -V`

    nginx version: nginx/1.18.0
    built by gcc 10.0.1 20200328 (Red Hat 10.0.1-0.11) (GCC)
    built with OpenSSL 1.1.1d FIPS  10 Sep 2019 (running with OpenSSL 1.1.1g FIPS  21 Apr 2020)
    TLS SNI support enabled
    configure arguments:
      --prefix=/usr/share/nginx
      --sbin-path=/usr/sbin/nginx
      --modules-path=/usr/lib64/nginx/modules
      --conf-path=/etc/nginx/nginx.conf
      --error-log-path=/var/log/nginx/error.log
      --http-log-path=/var/log/nginx/access.log
      --http-client-body-temp-path=/var/lib/nginx/tmp/client_body
      --http-proxy-temp-path=/var/lib/nginx/tmp/proxy
      --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi
      --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi
      --http-scgi-temp-path=/var/lib/nginx/tmp/scgi
      --pid-path=/run/nginx.pid
      --lock-path=/run/lock/subsys/nginx
      --user=nginx
      --group=nginx
      --with-file-aio
      --with-ipv6
      --with-http_ssl_module
      --with-http_v2_module
      --with-http_realip_module
      --with-stream_ssl_preread_module
      --with-http_addition_module
      --with-http_xslt_module=dynamic
      --with-http_image_filter_module=dynamic
      --with-http_sub_module
      --with-http_dav_module
      --with-http_flv_module
      --with-http_mp4_module
      --with-http_gunzip_module
      --with-http_gzip_static_module
      --with-http_random_index_module
      --with-http_secure_link_module
      --with-http_degradation_module
      --with-http_slice_module
      --with-http_stub_status_module
      --with-http_perl_module=dynamic
      --with-http_auth_request_module
      --with-mail=dynamic
      --with-mail_ssl_module
      --with-pcre
      --with-pcre-jit
      --with-stream=dynamic
      --with-stream_ssl_module
      --with-google_perftools_module
      --with-debug
      --with-cc-opt='-O2 -g -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fexceptions -fstack-protector-strong -grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -m64 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection'
      --with-ld-opt='-Wl,-z,relro -Wl,--as-needed -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E'

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
