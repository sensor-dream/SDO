---
title: 'OS VERSION ID '
date: '31-12-2021 15:48'
taxonomy:
    category:
        - docs
horizontal: false
shortcodes: true
---

```
FILE_OS_RELEASE="/etc/os-release"
OS_VERSION_ID="$(grep VERSION_ID ${FILE_OS_RELEASE} | awk -F= '{ print $2 }')"

or

OS_VERSION_ID="$(rpm -E %fedora)"
echo "${OS_VERSION_ID}
```