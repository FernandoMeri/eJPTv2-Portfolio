# 🛡️ Informe de Laboratorio: HackLabs - CSRF – Cross-Site Request Forgery (Easy)
**IP Objetivo:** 10.10.10.148 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Cross-Site Request Forgery (CSRF) en el formulario de cambio de contraseña, que permite a un atacante forzar el cambio de contraseña de un usuario legítimo sin su consentimiento, utilizando tanto herramientas manuales como automatizadas.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `CSRF – Cross-Site Request Forgery` en `http://10.10.10.148/csrf/profile`.
- Se identificó un formulario de cambio de contraseña para el usuario `alice` (ID: 2, rol: user).
- Se identificó una herramienta de "Ataque CSRF — Auto-envío" que simula un ataque automatizado.
- **Hallazgo clave:** El formulario no incluye ningún token anti-CSRF ni verificación de origen.

## 💥 Fase 2: Explotación del CSRF
- **Vulnerabilidad:** El endpoint de cambio de contraseña no valida la autenticidad de la petición, permitiendo a un atacante forjar peticiones maliciosas desde cualquier origen.

### Método 1: Burp Suite (Repeater)
- **Herramienta:** Burp Suite.
- **Ataque:**
    1.  Se interceptó la petición de cambio de contraseña de `alice` con Burp Suite.
    2.  Se modificó la petición para cambiar la contraseña del usuario `admin` (ID: 1) a `hacked123`.
    3.  El servidor aceptó la petición sin verificar el origen.
- **Resultado:** `{"message": "Contraseña cambiada para user_id=1", "status": "ok"}`

### Método 2: Herramienta Auto-envío CSRF
- **Herramienta:** Funcionalidad integrada en el laboratorio.
- **Ataque:**
    1.  Se introdujo el ID de la víctima (`1`) y la nueva contraseña (`hacked456`).
    2.  Se lanzó el ataque CSRF automatizado.
    3.  El servidor procesó la petición sin verificar el origen.
- **Resultado:** `{"message": "Contraseña cambiada para user_id=1", "status": "ok"}`

## 🧠 Lecciones Aprendidas
1.  **Tokens Anti-CSRF:** Todo formulario que realice acciones sensibles debe incluir un token único y secreto para validar la autenticidad de la petición.
2.  **Automatización del ataque:** La existencia de herramientas de auto-envío demuestra lo fácil que es para un atacante explotar esta vulnerabilidad de forma masiva.
3.  **Doble verificación:** Se recomienda validar el header `Origin` o `Referer` y requerir la contraseña actual del usuario para cambios de credenciales.

## 🏁 Conclusión
El laboratorio demuestra cómo la ausencia de un token anti-CSRF permite a un atacante forjar peticiones maliciosas y cambiar la contraseña de un usuario legítimo sin su consentimiento. La vulnerabilidad fue explotada exitosamente tanto manualmente con Burp Suite como de forma automatizada.
