# 🕸️ Informe de Pentesting: doc (HackMyVM)
**IP:** 10.10.10.7 | **Fecha:** 19/04/2026 | **Objetivo:** Preparación eJPTv2

## 🔍 Fase 1: Enumeración
- **Escaneo Nmap:** `nmap -sV -p- -T4 10.10.10.7`
- **Puertos abiertos:** 80/tcp (nginx 1.18.0)
- **Dominio virtual:** `doc.hmv` (añadido a `/etc/hosts`)

## 💥 Fase 2: Explotación de Inyección SQL (SQLi)
- **Vulnerabilidad:** Time-Based Blind SQL Injection en el parámetro `username` del formulario de login.
- **Herramienta:** `sqlmap`.
- **Comando:** `sqlmap -r login_request.txt -D doc -T users --dump --batch`
- **Resultado:** Volcado completo de la tabla `users`, incluyendo hashes de contraseñas.
- **Crackeo:** `sqlmap` crackeó automáticamente los hashes (MD5) usando su diccionario por defecto.

### 🔑 Credenciales obtenidas
| Usuario | Contraseña | Tipo |
|:---|:---|:---|
| `adminyo` | `admin123` | Administrador |
| `jsmith` | `jsmith123` | Usuario |
| `hacker` | `hacker123` | Administrador |

## 🚀 Fase 3: Post-Explotación (Intentos de RCE)
- **Acceso al panel de administración:** Login exitoso con `adminyo:admin123`.
- **Vector de ataque:** Subida de avatar con código PHP incrustado (archivo políglota).
- **Ruta de uploads:** `/uploads/1776630420_avatar.jpg` (obtenida mediante SQLi).
- **Resultado:** El servidor no ejecuta código PHP en archivos con extensión `.jpg`. La función `--os-shell` de `sqlmap` falló por restricciones de permisos del usuario de la BD (`dbuser`).

## 🧠 Lecciones Aprendidas
1. **Time-Based Blind SQLi:** Técnica sigilosa y efectiva cuando no hay errores visibles.
2. **Automatización con `sqlmap`:** Acelera enormemente la explotación y el crackeo de hashes.
3. **Restricciones de RCE:** Los permisos del sistema de ficheros y la configuración del servidor web pueden frustrar la ejecución de código incluso con acceso administrativo a la aplicación.
4. **Documentación de vectores fallidos:** Es tan importante como documentar los exitosos.

## 🏁 Conclusión
La máquina `doc` fue comprometida a nivel de aplicación web, obteniendo control total del panel de administración y credenciales de todos los usuarios. Sin embargo, la ejecución remota de código (RCE) no fue posible debido a las estrictas restricciones de permisos y configuración del servidor. Se recomienda para futuras auditorías investigar vulnerabilidades en la versión específica de nginx o en el código fuente de la aplicación.

