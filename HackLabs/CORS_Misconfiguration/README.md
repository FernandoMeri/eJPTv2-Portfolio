🕸️ Informe de Laboratorio: HackLabs - CORS Misconfiguration (Easy)
IP Objetivo: 10.10.10.125 | Fecha: 28/04/2026 | Objetivo: Preparación eJPTv2

🎯 Objetivo
Demostrar una mala configuración de CORS (Cross-Origin Resource Sharing) que permite a un atacante robar datos sensibles de una API desde un dominio malicioso.

🔍 Fase 1: Enumeración
Se accedió al laboratorio CORS Misconfiguration en http://10.10.10.125/cors.
La descripción del laboratorio indicaba que la API refleja cualquier cabecera Origin y tiene Access-Control-Allow-Credentials: true.
💥 Fase 2: Explotación de la Mala Configuración CORS
Vulnerabilidad: El servidor refleja el valor de la cabecera Origin sin validar, y permite el envío de credenciales (Access-Control-Allow-Credentials: true).
Payload: Se realizó una petición GET a /cors/data desde un origen malicioso simulado (Origin: https://evil.com).
Resultado: El servidor devolvió los datos sensibles de la API, incluyendo la flag y la lista de usuarios.
Evidencia:
Cabeceras de respuesta: Access-Control-Allow-Origin: https://evil.com y Access-Control-Allow-Credentials: true.
Flag capturada: FLAG{cors_misconfigured_4cc3ss}.
🧠 Lecciones Aprendidas
Validación del Origin: Nunca se debe reflejar el valor de la cabecera Origin sin validarlo contra una lista blanca de dominios permitidos.
Credenciales y CORS: Si Access-Control-Allow-Credentials: true, el riesgo es máximo, ya que un atacante puede hacer peticiones autenticadas desde un dominio malicioso.
Impacto: Una mala configuración CORS puede permitir el robo de datos privados de usuarios, tokens de sesión y otra información sensible.
🏁 Conclusión
El laboratorio demuestra cómo una configuración CORS demasiado permisiva expone los datos de la API a un atacante externo. Se logró robar la flag y la lista de usuarios simplemente haciendo una petición desde un origen malicioso simulado.
