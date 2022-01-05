---
title: 'Disable PackageKit daemon'
date: '04-01-2022 18:38'
---

>>> This is per-user:  
`dconf write /org/gnome/software/download-updates false`

and 

```
#!/bin/env bash

local pkc="/etc/PackageKit/PackageKit.conf"
sudo sed -i -e 's/^[#]BackendShutdownTimeout.*$/BackendShutdownTimeout=5/mig; s/^[#]ShutdownTimeout.*$/ShutdownTimeout=30/mig' "${pkc}"

```