# 🕵️ Informe de Laboratorio: HackLabs - A09 Security Logging & Monitoring Failures (Easy)
**IP Objetivo:** 10.10.10.143 | **Fecha:** 29/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de fallo en el registro y monitoreo de seguridad que permite a un atacante realizar actividades maliciosas sin dejar rastro alguno en los logs del sistema.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A09 – Security Logging & Monitoring Failures` en `http://10.10.10.143`.
- Se identificó un formulario de login y la indicación de que el archivo de log (`/var/log/hacklabs/access.log`) estaba vacío o no existía.

## 💥 Fase 2: Explotación del Fallo de Logging
- **Vulnerabilidad:** El sistema no registra ningún evento de seguridad, incluyendo intentos de inicio de sesión fallidos.
- **Prueba de Concepto (PoC):**
    1.  Se realizó un ataque de fuerza bruta con `hydra` utilizando el diccionario `fasttrack.txt` (262 contraseñas) contra el usuario `admin`.
    2.  El ataque se completó sin generar ninguna entrada en el archivo de log.
    3.  Se verificó la ausencia total de registros accediendo directamente al archivo de log, que devolvió un error 404.
- **Impacto en Seguridad:** Un ataque de fuerza bruta real, que podría involucrar miles de intentos, pasaría completamente desapercibido para los administradores del sistema, permitiendo al atacante probar contraseñas de forma indefinida sin riesgo de ser detectado.

## 🧠 Lecciones Aprendidas
1.  **Registro de Eventos de Seguridad:** Es fundamental registrar todos los eventos relevantes para la seguridad, incluyendo inicios de sesión exitosos y fallidos, con información como timestamp, IP de origen y User-Agent.
2.  **Monitorización Activa:** No basta con generar logs; deben ser monitorizados activamente para detectar patrones de ataque.
3.  **Integridad de los Logs:** Los archivos de registro deben estar protegidos contra accesos no autorizados y asegurar su integridad.

## 🏁 Conclusión
El laboratorio A09 demuestra cómo la falta de un sistema de logging y monitoreo adecuado permite a un atacante realizar actividades maliciosas sin ser detectado, eliminando completamente la capacidad de respuesta ante incidentes.
