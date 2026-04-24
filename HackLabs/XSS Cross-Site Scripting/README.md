# 🕸️ Informe de Laboratorio: HackLabs - XSS Cross-Site Scripting (Easy)
**IP Objetivo:** 10.10.10.115 | **Fecha:** 24/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de Cross-Site Scripting (XSS) en sus tres variantes (Reflected, Stored y DOM-based) en el laboratorio HackLabs y evidenciar el impacto de la inyección de código HTML/JavaScript.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `XSS - Cross-Site Scripting`.
- Se identificaron tres pestañas: **Reflejado**, **Almacenado** y **DOM-based**.
- **Dificultad:** Easy.

## 💥 Fase 2: Explotación

### Reflejado (Reflected XSS)
- **Vulnerabilidad:** El parámetro de búsqueda refleja el input del usuario sin sanitizar en la respuesta HTTP.
- **Payload:** `<script>alert(1)</script>`
- **Resultado:** Se ejecutó una ventana emergente (popup) con el número `1`, confirmando la ejecución de JavaScript arbitrario.

### Almacenado (Stored XSS)
- **Vulnerabilidad:** El input del usuario se almacena en el servidor y se renderiza sin sanitizar al cargar la página.
- **Payload:** `<script>alert("Stored XSS")</script>`
- **Resultado:** Se ejecutó una ventana emergente con el texto `Stored XSS`. El script persiste tras recargar la página.

### DOM-based XSS
- **Vulnerabilidad:** El código JavaScript del lado del cliente (`renderName()`) inyecta el contenido del parámetro `name` de la URL directamente en el DOM sin sanitizar.
- **Payload:** `<h1 style="color:red;">HACKEADO DOM</h1>`
- **Resultado:** El texto "HACKEADO DOM" se renderizó en la página como un título de gran tamaño, confirmando la inyección de código HTML arbitrario. *(Nota: Los filtros de estilos de la aplicación mitigaron el color rojo, pero la estructura HTML se procesó correctamente).*

## 🧠 Lecciones Aprendidas
1.  **Tipos de XSS:** Es crucial entender las diferencias entre Reflected, Stored y DOM-based para identificar el vector de ataque correcto.
2.  **Payloads alternativos:** Cuando un payload no funciona (`<script>`), se deben probar otros vectores como `<img>`, `<svg>` o inyección HTML directa para demostrar la vulnerabilidad.
3.  **Mitigaciones parciales:** La aplicación pudo filtrar el atributo `style`, pero no la estructura HTML, lo que demuestra una defensa insuficiente.
4.  **Impacto:** El XSS permite robar cookies de sesión, redirigir a usuarios a sitios maliciosos o desfigurar la página web.

## 🏁 Conclusión
Se demostró con éxito la vulnerabilidad de Cross-Site Scripting en sus tres variantes. La falta de sanitización de la entrada del usuario permite a un atacante inyectar código arbitrario, comprometiendo la seguridad de los usuarios y la integridad de la aplicación. Se recomienda implementar una política de escapado de HTML y validación de entrada en todos los puntos de interacción con el usuario.
