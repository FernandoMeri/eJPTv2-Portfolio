# 🕸️ Informe de Laboratorio: HackLabs - A10 Server-Side Request Forgery (SSRF) (Easy)
**IP Objetivo:** 10.10.10.178 | **Fecha:** 29/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Server-Side Request Forgery (SSRF) en el endpoint `/ssrf?url=...` que permite a un atacante realizar peticiones HTTP a recursos internos del servidor que no deberían ser accesibles desde el exterior.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A10 – Server-Side Request Forgery (SSRF)` en `http://10.10.10.178`.
- Se identificó un formulario con un campo "URL destino" y un botón "Obtener".
- La descripción del laboratorio indicaba que el endpoint `/ssrf?url=...` realiza peticiones HTTP a cualquier URL sin validar ni filtrar.

## 💥 Fase 2: Explotación del SSRF
- **Vulnerabilidad:** El endpoint `/ssrf?url=...` no valida ni filtra la URL proporcionada, permitiendo a un atacante especificar recursos internos.
- **Pruebas de concepto (PoC):**
    - **Petición a `http://127.0.0.1:5000/api/users`:** El servidor respondió con `Connection refused`, confirmando que intentó acceder a un recurso en localhost.
    - **Petición a `http://169.254.169.254/latest/meta-data/` (AWS metadata):** El servidor respondió con `No route to host`, un error diferente que demuestra que intentó acceder a una IP externa de cloud.
- **Conclusión técnica:** La diferencia en los mensajes de error (`Connection refused` vs `No route to host`) confirma que el servidor está procesando las peticiones SSRF y que la validación de la URL es inexistente.

## 🧠 Lecciones Aprendidas
1.  **Validación de URLs:** Nunca se debe permitir que el usuario controle completamente la URL de una petición saliente sin una estricta validación y whitelist de dominios permitidos.
2.  **Defensa en Profundidad:** Los servicios internos deben estar protegidos por firewalls y autenticación, asumiendo que un SSRF podría permitir el acceso a la red interna.
3.  **Análisis de Errores:** La diferencia en los mensajes de error del servidor puede ser utilizada para mapear la red interna.

## 🏁 Conclusión
El laboratorio A10 demuestra una vulnerabilidad SSRF contundente. La falta de validación en el parámetro `url` permite a un atacante escanear la red interna del servidor y acceder a servicios no expuestos. Los errores diferenciados del servidor confirman la vulnerabilidad sin lugar a dudas.
