# 🛡️ Informe de Laboratorio: HackLabs - Clickjacking 
**IP Objetivo:** 10.10.10.117 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Clickjacking que permite a un atacante superponer un iframe invisible sobre un botón señuelo, engañando a la víctima para que realice acciones no deseadas (como una transferencia de fondos).

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Clickjacking` en `http://10.10.10.117`.
- Se identificó una página de transferencia (`/clickjacking/transfer`) sin cabeceras de protección anti-framing (`X-Frame-Options`, `frame-ancestors`).
- Se comprobó que la página puede ser incrustada en un iframe desde un dominio externo.

## 💥 Fase 2: Explotación del Clickjacking
- **Vulnerabilidad:** La página de transferencia no implementa ninguna protección contra iframing.
- **Ataque:**
    1.  Se creó una página HTML maliciosa con un botón señuelo ("RECLAMAR PREMIO").
    2.  Se incrustó la página de transferencia en un iframe con `opacity: 0.01` y `z-index: 2`, superponiéndola exactamente sobre el botón señuelo.
    3.  La víctima ve el botón "RECLAMAR PREMIO", pero al hacer clic, realmente interactúa con el iframe invisible y confirma la transferencia.
- **Resultado:** Transferencia de $500 confirmada sin el conocimiento de la víctima.

## 🚩 Flag capturada
- **Flag:** `HL{cl1ckj4ck1ng_0wn3d}`

## 🧠 Lecciones Aprendidas
1.  **Cabeceras Anti-Framing:** Implementar `X-Frame-Options: DENY` o `Content-Security-Policy: frame-ancestors 'none'` previene este ataque.
2.  **Impacto del Clickjacking:** Permite robar clics de la víctima para realizar acciones sensibles (transferencias, cambios de configuración, etc.).
3.  **Defensa en profundidad:** Combinar cabeceras HTTP con JavaScript frame-buster para mayor protección.

## 🏁 Conclusión
El laboratorio demuestra cómo la ausencia de cabeceras anti-framing permite a un atacante secuestrar los clics de una víctima mediante un iframe invisible, realizando acciones no autorizadas en su nombre.
