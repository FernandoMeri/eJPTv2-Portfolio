# 🔐 Informe de Laboratorio: HackLabs - A07 Authentication Failures (Easy)
**IP Objetivo:** 10.10.10.101 | **Fecha:** 23/04/2026 | **Objetivo:** Preparación eJPTv2

## 🔍 Fase 1: Enumeración
- Se identificó un formulario de login en `http://10.10.10.101/login` sin protección contra fuerza bruta (sin rate-limiting ni CAPTCHA).
- Parámetros del formulario: `username` y `password`.

## 💥 Fase 2: Fuerza Bruta y Descubrimiento de Credenciales
- **Herramienta:** `hydra`.
- **Primer intento:** `hydra -l admin -P fasttrack.txt ...` dio 4 falsos positivos debido a un error en el mensaje de fallo (`Invalid credentials` vs `Credenciales incorrectas`).
- **Corrección:** Se ajustó el mensaje de error, pero `fasttrack.txt` no contenía la contraseña.
- **Solución manual:** Se probaron credenciales por defecto. La combinación `admin:password1` permitió el acceso al panel, mostrando el rol de administrador.

## 🚀 Fase 3: Acceso al Sistema (SSH)
- **Técnica:** Reutilización de credenciales.
- **Comando:** `ssh admin@10.10.10.101`
- **Resultado:** Acceso exitoso al servidor como usuario `admin`.

## 👑 Fase 4: Escalada de Privilegios
- **Vector:** Permisos `sudo` mal configurados.
- **Comando:** `sudo -l` reveló `(ALL) NOPASSWD: ALL`.
- **Explotación:** `sudo su` otorgó una shell de root inmediata.
- **Resultado:** Acceso total al sistema (`uid=0(root)`).

## 🚩 Fase 5: Captura de la Flag
- **Ubicación:** `/root/root.txt`
- **Contenido:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Falsos positivos en Hydra:** El mensaje de error DEBE ser exacto. Un simple cambio de idioma puede invalidar el ataque.
2.  **Limitaciones de diccionarios pequeños:** `fasttrack.txt` no siempre contiene la contraseña. Las credenciales por defecto (`admin:password1`) siguen siendo un vector muy común.
3.  **Reutilización de credenciales:** Las credenciales web a menudo funcionan en SSH.

## 🏁 Conclusión
El laboratorio A07 demuestra la importancia de una política de contraseñas robusta y la protección contra ataques de fuerza bruta. Se logró comprometer completamente el sistema combinando fuerza bruta (con lecciones aprendidas), credenciales por defecto y escalada de privilegios.
