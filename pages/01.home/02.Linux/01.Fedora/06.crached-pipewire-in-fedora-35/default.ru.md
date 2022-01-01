---
title: 'Crached pipewire in Fedora 35'
date: '31-12-2021 20:34'
taxonomy:
    category:
        - docs
horizontal: false
shortcodes: true
---

## First check wireplumber service

`systemctl --user status wireplumber`

## Then check wireplumber packets and remove old versions

`sudo dnf list wireplumber --installed`