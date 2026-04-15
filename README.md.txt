# 🛡️ Portafolio de Laboratorios - Preparación eJPTv2

![Status](https://img.shields.io/badge/eJPTv2-En%20Progreso-blue)
![SO](https://img.shields.io/badge/Plataformas-Windows%20%7C%20Linux-orange)
![Herramientas](https://img.shields.io/badge/Herramientas-Metasploit%20%7C%20Nmap%20%7C%20John%20%7C%20Hydra-red)

## 📋 Sobre este repositorio

Repositorio personal que documenta mi preparación práctica para la certificación **eLearnSecurity Junior Penetration Tester (eJPTv2)** . El objetivo es demostrar competencias prácticas en pentesting de redes, explotación de vulnerabilidades y post-explotación.

**Entorno de laboratorio:**
- **VirtualBox** con **Kali Linux** como máquina atacante.
- Red local configurada en modo **Adaptador Puente**.
- Máquinas vulnerables descargadas de VulnHub y comunidades de hacking ético.

## 🗂️ Máquinas Completadas

| # | Máquina | Sistema Operativo | Técnicas Clave | Informe |
|:---:|:---|:---|:---|:---:|
| 1 | **Blue** | Windows 7 | EternalBlue (MS17-010), Hashdump, John the Ripper | [📄 Ver](./Windows-Blue/README.md) |
| 2 | **Metasploitable 2** | Linux | vsftpd, UnrealIRCd, Samba, Escalada SUID (nmap) | [📄 Ver](./Linux-Metasploitable2/README.md) |
| 3 | **DC-1** | Linux (Web) | *Próximamente* | ⏳ |

## 🧠 Habilidades Adquiridas / Practicadas

- **Enumeración:** Escaneo completo de puertos (`-p-`), detección de versiones (`-sV`), scripts de Nmap.
- **Explotación:** Uso de Metasploit Framework (`search`, `use`, `set`, `run`), configuración de payloads (`reverse_tcp`).
- **Post-Explotación:** Volcado de hashes NTLM (`hashdump`), crackeo con `John the Ripper`.
- **Fuerza Bruta:** Ataques de diccionario con `Hydra` contra servicios SSH/FTP.
- **Escalada de Privilegios (Linux):** Explotación de binarios **SUID** mal configurados.
- **Pivoting:** *Próximamente (Máquina Ripper)*.

## 🎯 Objetivo Final

Llegar al examen del eJPTv2 con una metodología sólida y un portafolio que demuestre experiencia práctica real, más allá de los conceptos teóricos.

---
*Repositorio creado con fines educativos. Las máquinas documentadas son entornos de laboratorio controlados.*