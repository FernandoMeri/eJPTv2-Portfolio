# 🔗 Informe de Laboratorio: HackLabs - Open Redirect (Easy)
**IP Objetivo:** 10.10.10.115 | **Fecha:** 24/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de Open Redirect presente en el parámetro de destino del portal, permitiendo a un atacante redirigir a los usuarios a sitios externos maliciosos.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Open Redirect` en `http://10.10.10.115/open_redirect`.
- Se identificó un campo "URL de destino" y un botón "Ir a Destino".
- La descripción del laboratorio indicaba: *"El parámetro de destino no está validado"*.

## 💥 Fase 2: Explotación
- **Vulnerabilidad:** El parámetro `url` no tiene ninguna validación ni whitelist, permitiendo la redirección a cualquier dominio externo.
- **Prueba de Concepto (PoC):**
    - Se introdujo la URL `https://www.google.com` en el campo de destino.
    - El servidor redirigió correctamente a Google, confirmando la falta de restricciones.
    - Se introdujo `https://www.wikipedia.org` para demostrar la redirección a otro dominio arbitrario.

### Evidencia Técnica (petición HTTP):
curl -v "http://10.10.10.115/open_redirect?url=https://www.google.com" 2>&1 | grep -E "Location:|GET"
> GET /open_redirect?url=https://www.google.com HTTP/1.1
< Location: https://www.google.com

La cabecera `Location:` en la respuesta del servidor confirma la redirección no validada.

## 🧠 Lecciones Aprendidas
1.  **Validación de URLs de Redirección:** Es fundamental validar cualquier parámetro de redirección contra una lista blanca (whitelist) de dominios permitidos o usar redirecciones relativas.
2.  **Confianza del Usuario:** Un atacante puede aprovechar esta vulnerabilidad para crear enlaces de phishing convincentes, ya que la URL base pertenece a un sitio de confianza.
3.  **Impacto en la Reputación:** Un portal legítimo puede ser utilizado como puente para redirigir a los usuarios a sitios de descargas maliciosas o formularios de robo de credenciales, dañando la reputación del sitio original.

## 🏁 Conclusión
El laboratorio de Open Redirect demuestra una vulnerabilidad de seguridad web común pero peligrosa. La ausencia de validación en el parámetro `url` permite a un atacante redirigir a los usuarios a cualquier sitio externo, facilitando ataques de phishing y otros engaños.
