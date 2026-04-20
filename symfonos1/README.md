# 🎻 Informe de Pentesting: symfonos1 (VulnHub)
**IP:** 10.10.10.8 | **Fecha:** 20/04/2026 | **Objetivo:** Preparación eJPTv2

## 🔍 Fase 1: Enumeración
- **Escaneo Nmap:** `nmap -sV -p- -T4 10.10.10.8`
- **Puertos abiertos:** 22 (SSH), 25 (SMTP), 80 (HTTP), 139/445 (Samba).
- **Dominio virtual:** `symfonos.localdomain` (descubierto en el banner SMTP).

## 📁 Fase 2: Enumeración de Samba (SMB)
- **Recurso anónimo:** Se encontró el recurso compartido `anonymous` accesible sin credenciales.
- **Archivo hallado:** `attention.txt`, que contenía posibles contraseñas (`epidioko`, `qwerty`, `baseball`) y el nombre de usuario `Zeus`.

## 🕸️ Fase 3: Enumeración Web
- **Directorio oculto:** Se descubrió el directorio `/h3l105/` mediante fuerza bruta dirigida.
- **CMS:** WordPress 5.2.2 desactualizado.
- **Usuario:** `admin` (visible en los posts).

## 💥 Fase 4: Explotación (LFI y Acceso Inicial)
- **Vulnerabilidad:** Local File Inclusion (LFI) en el plugin `mail-masta`.
- **Vector:** `/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd`
- **Log Poisoning:** Los intentos de envenenar logs (SMTP, Apache, Auth) fueron infructuosos.
- **Acceso SSH:** Se leyó la clave privada SSH del usuario `helios` mediante el LFI (`/home/helios/.ssh/id_rsa`). Se accedió al sistema como `helios` usando la clave privada.

## 🚀 Fase 5: Escalada de Privilegios (PATH Hijacking)
- **Binario SUID:** Se encontró `/opt/statuscheck` con permisos SUID root.
- **Análisis:** El binario ejecuta `curl` sin la ruta completa (`strings /opt/statuscheck | grep curl`).
- **Explotación:**
    1.  Se creó un script malicioso `/tmp/curl` con el contenido `#!/bin/bash; /bin/bash -p`.
    2.  Se modificó la variable `PATH` para priorizar `/tmp` (`export PATH=/tmp:$PATH`).
    3.  Al ejecutar `/opt/statuscheck`, se obtuvo una shell con privilegios `root`.
- **Resultado:** Acceso total al sistema.

## 🚩 Fase 6: Flag Final
- **Ubicación:** `/root/proof.txt`
- **Contenido:** Arte ASCII con el mensaje "Congrats on rooting symfonos:1!" y contacto del creador (@zayotic).

## 🧠 Lecciones Aprendidas
1.  **Enumeración SMB:** Los recursos compartidos anónimos son una mina de oro para obtener pistas iniciales.
2.  **LFI en Plugins:** Plugins de WordPress desactualizados son un vector clásico de LFI.
3.  **PATH Hijacking:** Binarios SUID que llaman a comandos sin ruta absoluta son altamente vulnerables. La opción `-p` en Bash es crucial para mantener los privilegios elevados.
4.  **Persistencia:** No rendirse ante los "rabbit holes" y mantener una metodología clara es clave para el éxito.

## 🏁 Conclusión
`symfonos1` es una máquina excelente para practicar la enumeración de servicios, la explotación de vulnerabilidades web (LFI) y una escalada de privilegios clásica pero efectiva (PATH Hijacking). Se logró comprometer completamente el sistema y obtener la flag de root.
