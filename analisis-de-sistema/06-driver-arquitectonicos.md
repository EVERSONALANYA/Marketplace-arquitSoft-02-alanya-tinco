# 06 — Drivers arquitectónicos

Los drivers son los requisitos, atributos de calidad y restricciones que **influyen de manera importante** en cómo se diseña la arquitectura.

> Pregunta guía: *¿Qué requisito o condición puede cambiar la forma en que diseñamos la arquitectura?*

## Evaluación de candidatos

| Fuente | Elemento | ¿Puede ser driver? | Justificación |
|---|---|---|---|
| Requisito funcional | RF11 Procesar pago | **Sí** | Obliga a diseñar una integración segura con un sistema externo y manejar sus fallos. |
| Requisito funcional | RF16 Sincronizar stock con ERP | **Sí** | Define cómo fluye el stock entre el marketplace y el ERP. |
| Requisito funcional | RF04 Gestionar carrito | No | Es una funcionalidad CRUD que no cambia la estructura general. |
| Requisito funcional | RF17 Gestionar categorías | No | Funcionalidad administrativa simple. |
| Atributo de calidad | AC01 Rendimiento | **Sí** | Puede requerir caché, índices y optimización de consultas. |
| Atributo de calidad | AC02 Disponibilidad | **Sí** | Condiciona el manejo de fallos de servicios externos y el despliegue. |
| Atributo de calidad | AC03 Escalabilidad | **Sí** | Requiere un backend sin estado que pueda replicarse. |
| Atributo de calidad | AC04 Seguridad | **Sí** | Condiciona autenticación, autorización y protección de datos. |
| Atributo de calidad | AC05 Mantenibilidad | **Sí** | Justifica la separación en capas y módulos, y el control de dependencias internas. |
| Atributo de calidad | AC06 Usabilidad | No | Afecta el diseño de la interfaz, no la estructura de la arquitectura. |
| Restricción | RC03 API REST | **Sí** | Define el estilo de comunicación frontend–backend. |
| Restricción | RC04 Pasarela de pago | **Sí** | Condiciona la integración externa de pagos. |
| Restricción | RC07 Facturación electrónica | **Sí** | Agrega una integración externa obligatoria. |
| Restricción | RC10 Moneda e idioma | No | No cambia la estructura del sistema. |

## Drivers arquitectónicos seleccionados

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
|---|---|---|---|
| DA01 | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | AC03 – Escalabilidad | Influye en la estrategia de escalamiento y despliegue: el backend debe ser sin estado para replicarse. |
| DA02 | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia. | AC01 – Rendimiento | Influye en la comunicación entre componentes, procesamiento y almacenamiento (índices, caché del catálogo). |
| DA03 | El sistema debe proteger los datos de usuarios y operaciones de compra. | AC04 – Seguridad / RC08, RC09 | Influye en autenticación, autorización por rol y protección de datos. |
| DA04 | El sistema debe integrarse con una pasarela de pago externa mediante una API. | RC04 – Pasarela de pago / RF11 | Condiciona la forma de comunicación e integración con servicios externos. |
| DA05 | El sistema debe utilizar una API REST para la comunicación entre frontend y backend. | RC03 – API REST | Limita las alternativas de comunicación entre las partes del sistema. |
| DA06 | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | AC05 – Mantenibilidad | Influye en la separación de responsabilidades, la modularidad y las dependencias internas. |
| DA07 | El sistema debe integrarse con servicios externos de envío, facturación y ERP. | RC05, RC06, RC07 / RF16 | Requiere aislar las integraciones para que un cambio de proveedor no afecte la lógica de negocio. |

> **Nota (Guía 03):** se incorporó DA06 – Mantenibilidad / evolución modular, que reemplaza al anterior driver de módulos independientes. El driver de integraciones externas pasó a ser DA07.

## Resumen: drivers y decisiones que responden

| Driver | Problema que plantea | Decisión que responde |
|---|---|---|
| DA01 – Escalabilidad | Aumentarán los usuarios en campañas comerciales. | Monolito modular con posibilidad de escalamiento horizontal. |
| DA02 – Rendimiento | Habrá alta concurrencia. | Incorporar caché y optimizar la comunicación y el procesamiento. |
| DA03 – Seguridad | Hay datos sensibles de usuarios y compras. | Autenticación y autorización por rol. |
| DA04 – Pago externo | Hay que comunicarse con una pasarela de pago. | Integración mediante API y adaptadores. |
| DA05 – API REST | Frontend y backend deben comunicarse mediante REST. | Separar interfaz y backend mediante una API REST. |
| DA06 – Mantenibilidad | Los cambios no deben afectar otros módulos. | Modularidad + Clean Architecture. |
| DA07 – Integraciones externas | Envío, facturación y ERP son proveedores externos que pueden cambiar. | Interfaces (puertos) y adaptadores por cada servicio externo. |