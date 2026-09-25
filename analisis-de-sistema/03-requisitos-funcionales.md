# 03 — Requisitos funcionales

> **Historia de usuario:** expresa la necesidad desde el punto de vista del usuario.
> **Requisito funcional:** expresa lo que el sistema debe hacer para satisfacer esa necesidad.

## Requisitos funcionales

| ID | Requisito funcional | Módulo |
|---|---|---|
| RF01 | El sistema debe permitir buscar productos mediante criterios de búsqueda. | Catálogo |
| RF02 | El sistema debe permitir consultar la información y disponibilidad de los productos. | Catálogo |
| RF03 | El sistema debe permitir registrar y actualizar productos en la plataforma. | Catálogo |
| RF04 | El sistema debe permitir agregar, modificar y eliminar productos del carrito de compra. | Carrito |
| RF05 | El sistema debe permitir generar un pedido a partir de los productos del carrito. | Pedidos |
| RF06 | El sistema debe permitir consultar los pedidos realizados y su estado. | Pedidos |
| RF07 | El sistema debe permitir registrar, actualizar y desactivar sellers de la plataforma. | Sellers |
| RF08 | El sistema debe permitir consultar el detalle de un pedido realizado. | Pedidos |
| RF09 | El sistema debe permitir registrar usuarios e iniciar sesión con credenciales. | Usuarios |
| RF10 | El sistema debe asignar permisos según el rol del usuario (cliente, seller, administrador). | Usuarios |
| RF11 | El sistema debe permitir procesar el pago del pedido mediante la pasarela de pago externa. | Pedidos / Pagos |
| RF12 | El sistema debe permitir registrar y seleccionar direcciones de entrega. | Usuarios / Pedidos |
| RF13 | El sistema debe enviar la información del pedido al servicio de envío y mostrar su seguimiento. | Pedidos |
| RF14 | El sistema debe solicitar la emisión del comprobante electrónico al servicio de facturación. | Pedidos |
| RF15 | El sistema debe permitir al seller consultar los pedidos que contienen sus productos. | Sellers / Pedidos |
| RF16 | El sistema debe permitir actualizar el stock de los productos y sincronizarlo con el ERP. | Catálogo |
| RF17 | El sistema debe permitir registrar, actualizar y desactivar categorías de productos. | Catálogo |
| RF18 | El sistema debe permitir filtrar productos por tipo de mascota, marca, categoría y rango de precio. | Catálogo |

## Relación entre historias de usuario y requisitos funcionales

| Historia de usuario | Requisitos funcionales relacionados |
|---|---|
| HU01 Buscar y consultar productos | RF01, RF02 |
| HU02 Gestionar productos | RF03 |
| HU03 Gestionar carrito | RF04 |
| HU04 Realizar pedido | RF05, RF08 |
| HU05 Gestionar sellers | RF07 |
| HU06 Consultar pedidos | RF06, RF08, RF13 |
| HU07 Registrarse e iniciar sesión | RF09, RF10 |
| HU08 Pagar en línea | RF11 |
| HU09 Registrar direcciones | RF12 |
| HU10 Recibir comprobante | RF14 |
| HU11 Consultar ventas del seller | RF15 |
| HU12 Actualizar stock | RF16 |
| HU13 Gestionar categorías | RF17 |
| HU14 Filtrar productos | RF18 |
