# 🛡️ Informe de Laboratorio: HackLabs - A01 Broken Access Control (IDOR) (Easy)
**IP Objetivo:** 10.10.10.194 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Insecure Direct Object Reference (IDOR) que permite acceder a perfiles de otros usuarios sin autorización mediante la manipulación del parámetro `id`.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A01 – Broken Access Control (IDOR)` en `http://10.10.10.194`.
- Se identificó un formulario con un campo "ID de usuario" y un botón "Ver perfil".
- La funcionalidad carga el perfil del usuario correspondiente al `id` proporcionado.

## 💥 Fase 2: Explotación del IDOR
- **Vulnerabilidad:** La aplicación no verifica que el usuario autenticado tenga permiso para acceder al perfil solicitado. Simplemente devuelve los datos del `id` proporcionado.
- **Método 1 (Manual):** Se modificó el valor del campo "ID de usuario" de `1` a `2`, `3`, `4`, `5`, accediendo a los datos de otros usuarios, incluyendo campos sensibles como `credit_card`, `ssn`, `password_md5` y `password_plain`.
- **Método 2 (Burp Repeater):** Se interceptó la petición con Burp Suite, se envió a Repeater y se modificó el parámetro `id` para obtener los datos de otros usuarios.
- **Método 3 (Burp Intruder):** Se automatizó el ataque con Burp Intruder, configurando un payload de números del 1 al 10 para extraer los datos de todos los usuarios de forma masiva.

## 🧠 Lecciones Aprendidas
1.  **Control de Acceso Deficiente:** La aplicación confía ciegamente en el `id` proporcionado por el usuario sin verificar su identidad o permisos.
2.  **Datos Sensibles Expuestos:** Campos como `credit_card`, `ssn` y contraseñas en texto plano nunca deben ser accesibles sin una estricta verificación de autorización.
3.  **Automatización con Burp:** Herramientas como Burp Intruder permiten explotar este tipo de vulnerabilidades de forma masiva y eficiente.
4.  **Defensa:** Implementar controles de acceso a nivel de servidor que verifiquen que el usuario autenticado es el propietario del recurso o tiene permisos de administrador.

## 🏁 Conclusión
El laboratorio A01 demuestra un fallo crítico de control de acceso. La falta de verificación de permisos en el endpoint de perfiles permite a un atacante enumerar y extraer datos sensibles de todos los usuarios del sistema de forma trivial.
