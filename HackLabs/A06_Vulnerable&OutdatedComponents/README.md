# 🕸️ Informe de Laboratorio: HackLabs - A06 Outdated Components (Easy)
**IP Objetivo:** 10.10.10.125 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Cross-Site Scripting (XSS) causada por el uso de una versión desactualizada de jQuery (1.6.1) que inyecta HTML sin sanitizar en el DOM.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A06 – Vulnerable & Outdated Components` en `http://10.10.10.125`.
- La descripción indicaba que la página utiliza jQuery 1.6.1 (2011), con múltiples CVEs de XSS.
- El widget de búsqueda usa `$.html()` que inyecta HTML sin sanitizar en el DOM.

## 💥 Fase 2: Explotación del XSS
- **Vulnerabilidad:** jQuery 1.6.1 es vulnerable a múltiples CVEs de XSS. El método `$.html()` no sanitiza la entrada del usuario.
- **Payloads utilizados y resultados:**

| # | Payload | Vía | Resultado |
|:---|:---|:---|:---|
| 1 | `<img src=x onerror=alert(document.cookie)>` | Campo de búsqueda | ✅ Popup con la cookie de sesión |
| 2 | `<script>alert('XSS via jQuery 1.6.1')</script>` | Campo de búsqueda | ✅ Popup con el mensaje |
| 3 | `?q=<img src=x onerror=alert(1)>` | URL directa | ✅ Popup con `1` |

- **CVEs relevantes:** CVE-2011-4969, CVE-2015-9251, CVE-2019-11358.

## 🧠 Lecciones Aprendidas
1.  **Dependencias Desactualizadas:** El uso de librerías JavaScript obsoletas es una de las principales causas de vulnerabilidades XSS en aplicaciones modernas.
2.  **Sanitización del DOM:** Métodos como `$.html()` deben usarse con extrema precaución, sanitizando siempre la entrada del usuario.
3.  **Múltiples Vectores:** El XSS puede explotarse desde el campo de búsqueda y también directamente desde la URL, lo que facilita ataques de phishing.

## 🏁 Conclusión
El laboratorio demuestra cómo una librería jQuery sin actualizar puede exponer una aplicación a ataques XSS triviales. Se recomienda mantener todas las dependencias actualizadas y sanitizar cualquier entrada que se inyecte en el DOM.
