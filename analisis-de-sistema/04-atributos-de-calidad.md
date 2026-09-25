# 04 — Atributos de calidad

Los atributos de calidad indican **cómo** debe funcionar el sistema, más allá de **qué** hace.

**Escenario de análisis:** durante una campaña comercial (ej. Cyber Wow, Black Friday, Día de la Mascota), el marketplace podría recibir una gran cantidad de usuarios consultando productos y realizando compras simultáneamente.

| ID | Atributo | Escenario de calidad | Medida orientativa |
|---|---|---|---|
| AC01 | **Rendimiento** | Las consultas de productos y operaciones del carrito deben responder rápidamente incluso cuando exista una alta cantidad de usuarios concurrentes. | Búsquedas y carrito en menos de 2 s en el 95 % de las solicitudes. |
| AC02 | **Disponibilidad** | El sistema debe permanecer disponible durante la campaña comercial y permitir que los usuarios realicen sus operaciones. | Disponibilidad de 99.5 % mensual; si la pasarela falla, el pedido queda "pendiente de pago" y no se pierde. |
| AC03 | **Escalabilidad** | El sistema debe poder soportar un incremento de usuarios y solicitudes sin afectar significativamente su funcionamiento. | Soportar al menos 5 veces el tráfico normal agregando instancias del backend. |
| AC04 | **Seguridad** | Los datos de los usuarios, cuentas y operaciones de compra deben estar protegidos frente a accesos no autorizados. | HTTPS en todo el sitio, contraseñas con hash, autorización por rol y ningún dato de tarjeta almacenado en el sistema. |
| AC05 | **Mantenibilidad** | El sistema debe estar organizado de manera que permita realizar cambios y correcciones sin afectar innecesariamente otras funcionalidades. | Módulos separados por responsabilidad; un cambio en Carrito no requiere modificar Sellers. |
| AC06 | **Usabilidad** | Un cliente nuevo debe poder encontrar un producto y completar una compra sin ayuda, desde computadora o celular. | Compra completada en 5 pasos o menos; interfaz responsive. |
