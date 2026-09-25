# 05 — Restricciones

Condiciones, reglas o limitaciones que deben respetarse durante el desarrollo. Pueden ser tecnológicas, organizacionales, legales o del proyecto.

## Restricciones base (guía)

| ID | Restricción | Tipo | Descripción |
|---|---|---|---|
| RC01 | Aplicación web | Tecnológica | El sistema debe desarrollarse como una aplicación accesible mediante un navegador web. |
| RC02 | Control de versiones | Proyecto | El código fuente debe gestionarse utilizando Git y mantenerse en un repositorio compartido (GitHub). |
| RC03 | API REST | Tecnológica | La comunicación entre el frontend y los servicios del sistema debe realizarse mediante una API REST. |
| RC04 | Pasarela de pago | Tecnológica | El sistema debe integrarse con una pasarela de pago externa para procesar las operaciones de pago. |
| RC05 | Servicio de envío | Tecnológica | El sistema debe integrarse con un servicio externo de envío para gestionar la información de entrega de pedidos. |

## Restricciones adicionales identificadas

| ID | Restricción | Tipo | Descripción |
|---|---|---|---|
| RC06 | Integración con ERP | Organizacional | La empresa ya cuenta con un ERP; la información de productos y stock debe sincronizarse con él y no reemplazarlo. |
| RC07 | Facturación electrónica | Legal | Los comprobantes de pago deben emitirse electrónicamente conforme a la normativa de la SUNAT. |
| RC08 | Protección de datos personales | Legal | El tratamiento de datos de clientes y sellers debe cumplir la Ley N.° 29733, Ley de Protección de Datos Personales del Perú. |
| RC09 | No almacenar datos de tarjeta | Legal / Seguridad | Los datos de tarjetas los procesa únicamente la pasarela de pago; el sistema solo guarda el identificador y el estado de la transacción. |
| RC10 | Moneda e idioma | Organizacional | Los precios se expresan en soles (PEN) y la interfaz se presenta en español. |
| RC11 | Diseño responsive | Tecnológica | La aplicación web debe adaptarse a dispositivos móviles, desde donde accede gran parte de los clientes. |
| RC12 | Documentación en Markdown | Proyecto | La documentación de análisis y arquitectura se redacta en Markdown y los diagramas en Mermaid o Draw.io. |
| RC13 | Tiempo académico | Proyecto | El proyecto debe desarrollarse dentro del semestre 2026-II, por lo que la arquitectura inicial debe ser simple de implementar. |
