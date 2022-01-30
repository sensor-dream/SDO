---
title: 'Optimise Firefox'
author: 'Pavel M. Teslenko'
date: '30-01-2022 17:42'
---

>>>>
Open about:config

Search browser.sessionstore.interval (The interval (In ms) after which the session is saved. It loads the disk very much, a known problem)
set 1800000(30min, default 15000 - 15cek)

Search layers.acceleration.force-enabled (Enabling acceleration on the gpu - video card)
and set to true

Disabling the braking disk cache:
	Search
    	browser.cache.disk.enable
		browser.cache.disk.smart_size.enabled
		browser.cache.disk_cache_ssl
		browser.cache.offline.enable
	and set to all false (default all set to true)
