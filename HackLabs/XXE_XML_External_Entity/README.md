# 📁 Informe de Laboratorio: HackLabs - XXE – XML External Entity (Easy)
**IP Objetivo:** 10.10.10.178 | **Fecha:** 29/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de XML External Entity (XXE) que permite a un atacante leer archivos del sistema mediante la inyección de una entidad externa maliciosa en un documento XML procesado por el servidor.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `XXE – XML External Entity` en `http://10.10.10.178/xxe`.
- Se identificó un formulario de creación de tickets de soporte que envía los datos en formato XML al endpoint `/xxe/api`.

## 💥 Fase 2: Explotación del XXE
- **Vulnerabilidad:** El parser XML del servidor no deshabilita el procesamiento de entidades externas (DTD).
- **Herramienta:** Burp Suite (Repeater).
- **Payload:** Se definió una entidad externa `xxe` que apunta a `file:///etc/passwd` y se referenció en el campo `name` del ticket.
- **Resultado:** El servidor devolvió el contenido completo del archivo `/etc/passwd` en el campo `name` del ticket creado, confirmando la vulnerabilidad.

## 🧠 Lecciones Aprendidas
1.  **Configuración del Parser XML:** Es fundamental deshabilitar el procesamiento de entidades externas (DTD) en los parsers XML de las aplicaciones.
2.  **Impacto del XXE:** Esta vulnerabilidad permite leer archivos del sistema, realizar peticiones SSRF, y en algunos casos, ejecutar código remoto.
3.  **Burp Suite para XXE:** Interceptar y modificar peticiones XML con Burp Repeater es una técnica esencial para explotar este tipo de fallos.

## 🏁 Conclusión
El laboratorio de XXE demuestra cómo una mala configuración de un parser XML permite a un atacante leer archivos sensibles del servidor. Se logró extraer el archivo `/etc/passwd` completo, lo que confirma la gravedad de la vulnerabilidad.
