# 🛡️ Informe de Laboratorio: HackLabs - A05 Security Misconfiguration (Easy)
**IP Objetivo:** 10.10.10.125 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar múltiples fallos de configuración de seguridad en el servidor HackLabs, incluyendo exposición de paneles de administración, repositorios Git, trazas de error y APIs internas sin autenticación.

## 🔍 Fase 1: Enumeración Metodológica
- **Fuzzing de directorios:** Se utilizó `gobuster` con extensiones `php,html,txt,git,json` para descubrir endpoints expuestos.
- **Escaneo de vulnerabilidades:** Se utilizó `nikto` para detectar cabeceras inseguras y archivos sensibles.

### Resultados de `gobuster`:
/admin (Status: 200)
/console (Status: 400)
/files (Status: 200)
/login (Status: 200)
/logout (Status: 302)
/profile (Status: 200)
/upload (Status: 200)

## 💥 Fase 2: Hallazgos de Seguridad y Explotación

### 1. Panel de Administración Expuesto (`/admin`)
- **Vulnerabilidad:** Acceso sin autenticación.
- **Datos expuestos:** Lista completa de usuarios, contraseñas en texto plano (`password1`, `Password1`, etc.), hashes MD5, roles de usuario.

### 2. Repositorio Git Expuesto (`/.git/config`)
- **Vulnerabilidad:** El directorio `.git` es accesible públicamente.
- **Información obtenida:** URL del repositorio interno (`http://internal-git.hacklabs.local/hacklabs.git`).

### 3. Stack Trace del Servidor (`/debug/error`)
- **Vulnerabilidad:** Modo debug de Werkzeug expuesto.
- **Información obtenida:**
    - Versiones de software: Flask 2.3.0, Python 3.11, SQLite 3.39.
    - Ruta del código fuente: `/var/www/hacklabs/app.py`.
    - Clave secreta `SECRET` expuesta.

### 4. API de Usuarios sin Autenticación (`/api/users`)
- **Vulnerabilidad:** Endpoint de API accesible sin credenciales.
- **Datos expuestos:** Listado JSON de todos los usuarios, emails y roles.

### 5. Cabeceras de Seguridad Ausentes
- **Vulnerabilidades detectadas por `nikto`:**
    - Falta cabecera `X-Frame-Options` (posible clickjacking).
    - Falta cabecera `X-Content-Type-Options` (posible MIME sniffing).
    - Cookie `is_admin` sin flag `httponly` (posible robo de cookie vía XSS).
    - Archivo `#wp-config.php#` encontrado (posible backup de configuración).

## 🧠 Lecciones Aprendidas
1.  **Metodología de Enumeración:** El fuzzing de directorios y el escaneo con `nikto` son pasos fundamentales para descubrir este tipo de fallos.
2.  **Defensa en Profundidad:** La seguridad no se trata de un solo mecanismo, sino de capas. La ausencia de autenticación, la exposición de información y la falta de cabeceras de seguridad son fallos que se acumulan.
3.  **Impacto Acumulativo:** Un fallo individual puede ser subsanable, pero la combinación de varios fallos de configuración puede comprometer completamente un sistema.

## 🏁 Conclusión
El laboratorio A05 expone una serie de malas configuraciones que, combinadas, permiten a un atacante obtener un control total sobre la aplicación y sus datos. La metodología aplicada demuestra la importancia de una fase de enumeración exhaustiva para descubrir todos los vectores de ataque posibles.
