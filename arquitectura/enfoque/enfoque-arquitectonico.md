# Enfoque arquitectónico: Clean Architecture

## 1. Ficha del enfoque

| Elemento | Descripción aplicada al Marketplace |
|---|---|
| Patrón / enfoque arquitectónico | Clean Architecture (Arquitectura Limpia). |
| Objetivo | Separar responsabilidades y controlar las dependencias hacia el dominio. |
| ¿Qué problema resuelve? | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas, como bases de datos, API y servicios de pago. |
| Capas definidas | Presentación, Aplicación, Dominio e Infraestructura. |
| Driver que responde | DA06 – Mantenibilidad (ADR-002); también apoya DA04 y DA07 mediante contratos y adaptadores. |
| Beneficios | Facilita el mantenimiento y las pruebas unitarias. Permite cambiar implementaciones técnicas sin modificar innecesariamente las reglas del negocio. Mejora la organización y separación de responsabilidades del código. |

## 2. Regla de dependencia

Las dependencias del código **siempre apuntan hacia el centro**:

```
Presentación  ─┐
               ├──►  Aplicación  ──►  Dominio
Infraestructura┘                       ▲
       └──── implementa los contratos ─┘
```

1. El **dominio** no importa nada de las capas externas (ni Angular, ni HttpClient, ni librerías de pago).
2. Los **casos de uso** solo conocen entidades y contratos del dominio.
3. Los **adaptadores** de infraestructura implementan los contratos del dominio; son intercambiables.
4. Cambiar de tecnología o proveedor significa cambiar el adaptador registrado, no el dominio.

## 3. Organización de responsabilidades por capa

| Carpeta | Capa | ¿Qué contiene? | Ejemplos en el Marketplace |
|---|---|---|---|
| `dominio/` | Domain | Entidades, objetos de valor, reglas de negocio y contratos (puertos). | Entidades: `Producto`, `Carrito`, `Pedido`, `Seller`. Reglas: cálculo de subtotal, IGV y comisión. Contratos: `RepositorioProductos`, `RepositorioPedidos`, `ProcesadorPagos`, `ServicioEnvio`, `ServicioFacturacion`, `SincronizadorERP`. |
| `aplicacion/` | Application | Casos de uso que coordinan el dominio y DTO cuando sean necesarios. | `ConsultarCatalogoCasoUso`, `AgregarAlCarritoCasoUso`, `RegistrarCompraCasoUso`, `RegistrarProductoSellerCasoUso`. |
| `presentacion/` | Presentación / adaptadores de interfaz | Pantallas, componentes visuales y estado de la vista. | `CatalogoComponent`, `CarritoComponent`, `PagoComponent`, `PanelSellerComponent`, `EstadoCarrito`. |
| `infraestructura/` | Infraestructura / frameworks y drivers | Implementaciones concretas de los contratos para APIs, base de datos y servicios externos. | `RepositorioProductosHttp`, `RepositorioPedidosHttp`, `ProcesadorPagosPasarela`, `ServicioEnvioCourier`, `ServicioFacturacionSunat`, `SincronizadorERPHttp`. |
| `app.config.ts` | Composición | Único lugar que decide qué adaptador cumple cada contrato y lo inyecta. | Registrar `ProcesadorPagosPasarela` para `ProcesadorPagos` (o uno simulado para pruebas). |

## 4. Ejemplo de flujo: registrar una compra

1. `PagoComponent` (presentación) invoca a `RegistrarCompraCasoUso` (aplicación).
2. El caso de uso usa las entidades `Carrito` y `Pedido` (dominio) para validar stock y calcular el total.
3. Pide cobrar mediante el contrato `ProcesadorPagos` y guardar mediante `RepositorioPedidos`, sin saber qué tecnología hay detrás.
4. `ProcesadorPagosPasarela` y `RepositorioPedidosHttp` (infraestructura) hacen la comunicación real con la pasarela y la API REST.

Si mañana se cambia de pasarela de pago, solo se crea un nuevo adaptador y se registra en `app.config.ts`; el caso de uso y el dominio no cambian.

## 5. Diagrama del enfoque

```mermaid
flowchart LR

    Usuario(["Usuario<br/>(Cliente / Seller)"])

    subgraph PRES["PRESENTACIÓN — presentacion/"]
        CatComp["CatalogoComponent"]
        CarComp["CarritoComponent"]
        PagComp["PagoComponent"]
        Estado["EstadoCarrito"]
    end

    subgraph APP["APLICACIÓN — aplicacion/ (casos de uso)"]
        UC1["ConsultarCatalogoCasoUso"]
        UC2["AgregarAlCarritoCasoUso"]
        UC3["RegistrarCompraCasoUso"]
    end

    subgraph DOM["DOMINIO — dominio/ (núcleo, sin frameworks)"]
        subgraph ENT["Entidades y reglas"]
            Producto["Producto"]
            Carrito["Carrito"]
            Pedido["Pedido"]
            Precios["reglas de precios<br/>IGV · comisión"]
        end
        subgraph CON["Contratos (puertos)"]
            IRP["«interface»<br/>RepositorioProductos"]
            IRPed["«interface»<br/>RepositorioPedidos"]
            IPago["«interface»<br/>ProcesadorPagos"]
            IEnv["«interface»<br/>ServicioEnvio"]
            IFac["«interface»<br/>ServicioFacturacion"]
        end
    end

    subgraph INFRA["INFRAESTRUCTURA — infraestructura/ (adaptadores)"]
        ARP["RepositorioProductosHttp"]
        ARPed["RepositorioPedidosHttp"]
        APago["ProcesadorPagosPasarela"]
        AEnv["ServicioEnvioCourier"]
        AFac["ServicioFacturacionSunat"]
    end

    Config["app.config.ts<br/>(composición: elige qué adaptador<br/>cumple cada contrato)"]

    subgraph EXT["SISTEMAS EXTERNOS"]
        API["API REST<br/>Marketplace Backend"]
        Pasarela["Pasarela de pago"]
        Envio["Servicio de envío"]
        Fact["Facturación electrónica"]
    end

    Usuario --> PRES
    CatComp --> UC1
    CarComp --> UC2
    PagComp --> UC3
    CarComp -.-> Estado

    UC1 -.-> Producto
    UC1 -.-> IRP
    UC2 -.-> Carrito
    UC3 -.-> Pedido
    UC3 -.-> IRPed
    UC3 -.-> IPago
    UC3 -.-> IEnv
    UC3 -.-> IFac

    ARP -.->|implementa| IRP
    ARPed -.->|implementa| IRPed
    APago -.->|implementa| IPago
    AEnv -.->|implementa| IEnv
    AFac -.->|implementa| IFac

    ARP --> API
    ARPed --> API
    APago --> Pasarela
    AEnv --> Envio
    AFac --> Fact

    Config -.->|registra| INFRA

    style DOM fill:#5c4a0e,stroke:#fff,stroke-width:2px,color:#fff
    style APP fill:#1f4d2e,stroke:#fff,stroke-width:2px,color:#fff
    style PRES fill:#1e3a5f,stroke:#fff,stroke-width:2px,color:#fff
    style INFRA fill:#4a1f4a,stroke:#fff,stroke-width:2px,color:#fff
    style EXT fill:#333,stroke:#fff,stroke-dasharray:5 5,color:#fff
```

**Leyenda:** flecha continua = llamada en tiempo de ejecución; flecha punteada = dependencia de código (siempre apunta hacia el dominio); "implementa" = el adaptador cumple el contrato definido en el dominio (inversión de dependencias).