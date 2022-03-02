---
title: 'Восстановление рабочего стола Gnome после выхода из режима ожидания'
author: 'Pavel M. Teslenko'
date: '02-03-2022 19:31'
---

	sudo gedit  /lib/systemd/system-sleep/broken-desktop-fix

and insert

	#!/bin/bash
 
	case "" in
  		post)
    	DISPLAY=:0.0 ; export DISPLAY
    	STR="$(users)"
    	echo ${STR}
    	IFS=' ' read -ra NAMES <<< ${STR}
 
     	for i in "${NAMES[@]}"; do
        	su $i -c 'dbus-send --type=method_call --dest=org.gnome.Shell /org/gnome/Shell org.gnome.Shell.Eval "string:global.reexec_self()"'
    done;;esac
  
	sudo chmod +x /lib/systemd/system-sleep/broken-desktop-fix
  
!!! Вы можете вручную перезагружать рабочее окружение после выхода из режима ожидания. Для этого воспользуйтесь комбинацией клавиш ALT+F2 и в появившемся окне введите r и нажмите Enter
