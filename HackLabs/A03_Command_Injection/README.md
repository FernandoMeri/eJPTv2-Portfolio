# 💣 Informe de Laboratorio: HackLabs - A03 Command Injection (Easy)
**IP Objetivo:** 10.10.10.180 | **Fecha:** 23/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de Inyección de Comandos (Command Injection) en el campo de "ping" de HackLabs, obtener ejecución remota de comandos (RCE) y acceder a la flag de root.

## 🔍 Fase 1: Enumeración y Descubrimiento
- **Herramienta:** `nmap -sV -p 21,22,80,445 <IP_HackLabs>`
- **Servicio objetivo:** HTTP (puerto 80). Se accedió al laboratorio `A03 - Command Injection` desde el navegador web.
- **Funcionalidad:** El laboratorio presenta un campo de texto para introducir una dirección IP y un botón "Ping" que ejecuta el comando `ping` en el servidor.

## 💥 Fase 2: Explotación de Command Injection
- **Vulnerabilidad:** Inyección de comandos en el campo de "ping". El servidor concatena la entrada del usuario directamente en un comando del sistema sin la sanitización adecuada.
- **Payload:** Se utilizó el carácter separador `;` para ejecutar comandos adicionales al `ping`.

### Pasos de Explotación:
1.  **Prueba de concepto (PoC):** Se introdujo el payload `10.10.10.5; whoami`. El servidor devolvió `root` junto a un error de `ping: not found`, confirmando la vulnerabilidad y que el script se ejecutaba con los máximos privilegios.
2.  **Captura de la flag:** Dado que el script se ejecutaba como `root`, se accedió directamente a la flag final con el payload `10.10.10.5; cat /root/root.txt`.

## 🚩 Fase 3: Captura de la Flag
- **Ubicación:** `/root/root.txt`
- **Payload utilizado:** `10.10.10.5; cat /root/root.txt`
- **Contenido de la flag:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Impacto de Command Injection:** Una simple concatenación no sanitizada en un comando del sistema puede llevar al compromiso total del servidor en cuestión de segundos, eludiendo toda la cadena de ataque tradicional.
2.  **Privilegios del servicio:** Ejecutar scripts o servicios web con privilegios de `root` magnifica exponencialmente el impacto de cualquier vulnerabilidad, como se ha demostrado en este laboratorio.
3.  **Validación de Entradas:** Es crucial sanitizar y validar cualquier entrada del usuario que pueda llegar a ser interpretada por el sistema operativo.

## 🏁 Conclusión
El laboratorio A03 de Command Injection de HackLabs ilustra de forma clara y contundente la gravedad de esta vulnerabilidad. Se obtuvo acceso de `root` y la flag del sistema de forma directa, sin necesidad de exploits complejos ni escalada de privilegios posterior.
