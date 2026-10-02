# Decisiones arquitectónicas (ADR)

Un **ADR (Architecture Decision Record)** documenta una decisión importante tomada durante el diseño de la arquitectura, junto con el driver que la motiva y su justificación.

| ID | Decisión arquitectónica | Driver relacionado | Justificación | Resultado |
|---|---|---|---|---|
| ADR-001 | Monolito modular | DA01 – Escalabilidad; DA06 – Mantenibilidad | Organizar las funcionalidades en módulos independientes dentro de una misma aplicación desplegable. | Módulos de Catálogo, Carrito, Pedidos, Pagos y Usuarios. |
| ADR-002 | Clean Architecture | DA06 – Mantenibilidad | Separar las reglas del negocio de los detalles tecnológicos. | Capas de Dominio, Aplicación, Infraestructura y Presentación. |
| ADR-003 | Estrategia de caché | DA02 – Rendimiento | Reducir consultas repetitivas a la fuente de datos. | Caché para información de consulta frecuente (catálogo, categorías). |
| ADR-004 | Integración de pagos mediante interfaces y adaptadores | DA04 – Pago externo | Desacoplar los casos de uso del proveedor de pagos. | Contrato de pagos y adaptador para la pasarela externa. |
| ADR-005 | Integraciones externas mediante puertos y adaptadores | DA07 – Integraciones externas | Evitar que un cambio de proveedor de envío, facturación o ERP afecte la lógica de negocio. | Un contrato (interfaz) por servicio externo y un adaptador concreto en infraestructura. |

## Detalle de cada decisión

### ADR-001 — Monolito modular
- **Contexto:** se espera crecimiento de usuarios en campañas (DA01) y el sistema debe poder evolucionar sin romper otros módulos (DA06).
- **Alternativas evaluadas:** monolito tradicional, microservicios.
- **Decisión:** monolito modular, una sola unidad de despliegue con módulos de negocio bien separados.
- **Consecuencias:** despliegue y operación simples; si un módulo crece mucho, puede extraerse a un servicio más adelante. Requiere disciplina para que los módulos no accedan a datos de otros directamente.

### ADR-002 — Clean Architecture
- **Contexto:** el negocio debe poder cambiar de tecnología (base de datos, framework, proveedores) sin reescribir sus reglas (DA06).
- **Alternativas evaluadas:** capas tradicionales, MVC, hexagonal.
- **Decisión:** Clean Architecture, con dependencias que siempre apuntan hacia el dominio.
- **Consecuencias:** reglas de negocio aisladas y fáciles de probar; mayor cantidad de interfaces y archivos.

### ADR-003 — Estrategia de caché
- **Contexto:** alta concurrencia durante campañas (DA02).
- **Alternativas evaluadas:** consultar siempre la base de datos, réplicas de lectura.
- **Decisión:** caché para datos de lectura frecuente y bajo cambio.
- **Consecuencias:** menor tiempo de respuesta; hay que definir cuándo se invalida la caché para no mostrar stock o precios desactualizados.

### ADR-004 — Pagos mediante interfaces y adaptadores
- **Contexto:** el pago depende de una pasarela externa (DA04).
- **Alternativas evaluadas:** llamar a la pasarela directamente desde el caso de uso.
- **Decisión:** el caso de uso depende de un contrato `ProcesadorPagos`; la pasarela concreta se implementa como adaptador.
- **Consecuencias:** se puede cambiar de pasarela o simularla en pruebas sin tocar la lógica de compra.

### ADR-005 — Integraciones externas mediante puertos y adaptadores
- **Contexto:** envío, facturación electrónica y ERP son servicios de terceros (DA07).
- **Alternativas evaluadas:** integración directa con cada proveedor.
- **Decisión:** definir un contrato por cada servicio externo y un adaptador en infraestructura.
- **Consecuencias:** cambiar de proveedor afecta solo su adaptador; agrega una capa de abstracción por integración.