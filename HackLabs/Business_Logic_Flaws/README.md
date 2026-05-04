# 🛡️ Informe de Laboratorio: HackLabs - Business Logic Flaws 
**IP Objetivo:** 10.10.10.148 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de lógica de negocio que permite a un atacante manipular el precio de un producto en el lado del cliente y comprarlo por un valor muy inferior al real.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Business Logic Flaws` en `http://10.10.10.148`.
- Se identificó un catálogo de productos con precios y un carrito de compras.
- Se inspeccionó el formulario de "Add to Cart" del producto HackLabs Pro License ($99.99).
- **Hallazgo clave:** El precio se envía como un campo oculto (`<input type="hidden" name="price" value="9999">`) en céntimos.

## 💥 Fase 2: Explotación del Fallo de Lógica
- **Vulnerabilidad:** El servidor confía ciegamente en el precio enviado desde el cliente sin validarlo en el backend.
- **Herramienta:** Burp Suite (Intercept).
- **Ataque:**
    1.  Se interceptó la petición `POST /shop/cart/add` con Burp Suite.
    2.  Se modificó el parámetro `price` de `9999` (99.99$) a `1` (0.01$).
    3.  La petición se envió al servidor, que aceptó el precio manipulado.
- **Resultado:** El producto fue añadido al carrito por $0.01. Se completó la compra con éxito.

## 🚩 Flag capturada
- **Flag:** `HL{bu51n355_l0g1c_0wn3d}`

## 🧠 Lecciones Aprendidas
1.  **Validación en el servidor:** Nunca se debe confiar en los datos enviados desde el cliente. Toda validación de precios debe realizarse en el backend.
2.  **Campos ocultos modificables:** Los campos `hidden` en formularios HTML son fácilmente manipulables por un atacante.
3.  **Impacto en el negocio:** Este tipo de fallos puede causar pérdidas económicas directas y daños a la reputación.

## 🏁 Conclusión
El laboratorio demuestra cómo un fallo de lógica de negocio, combinado con la confianza ciega en datos del cliente, permite a un atacante comprar productos por un precio irrisorio. La validación en el servidor es fundamental para prevenir este tipo de ataques.
