# 06 · Drivers arquitectónicos

## Factores que determinan la arquitectura

| Driver | Prioridad | Origen | Decisión | Consecuencia |
| --- | --- | --- | --- | --- |
| D01 · Evitar sobreventa | Crítica | Problema, RF04, RNF11 | Transacciones PostgreSQL y bloqueo por salida | Mayor contención en una salida muy demandada; exige pruebas concurrentes |
| D02 · Vincular reserva y cobro | Crítica | RF05, RF12 | Estados separados, idempotencia y conciliación | Más estados explícitos, pero sin confirmaciones falsas |
| D03 · Cubrir operación turística completa | Alta | RF01–RF13 | Módulos de catálogo, inventario, reservas, pagos, guías, atención y vuelos | Disciplina de límites dentro del monolito |
| D04 · Minimizar costo del MVP | Alta | RNF08 | Servicios gestionados y despliegue simple | Cuotas y suspensión obligan a una ruta pagada |
| D05 · Demanda estacional | Alta | RNF02, RNF03 | CDN, caché pública, réplicas stateless y trabajos con límite | La base sigue siendo cuello de botella a proteger |
| D06 · Fallos externos | Alta | RNF09 | Timeouts, circuit breaker y compensaciones | Necesidad de estados inciertos y atención operativa |
| D07 · Seguridad y confianza | Crítica | RNF04 | TLS, WAF, JWT verificado y autorización por agencia | El backend conserva la responsabilidad de acceso |
| D08 · Mantenibilidad | Alta | RNF07 | Un monolito modular con capas y ADR | Liberación coordinada y pruebas de integración |
| D09 · Acceso móvil | Media | RNF05, RNF06 | PWA responsiva y contratos API reutilizables | Borradores offline sujetos a revalidación |
| D10 · Emisión aérea | Alta | RF13 | Adaptador de un consolidador, cotización con vencimiento y emisión asíncrona | Inventario externo y compensaciones comerciales |
| D11 · Trazabilidad | Alta | RNF10, RNF12 | Outbox, auditoría, correlación y OpenAPI | Almacenamiento y retención de registros controlados |

## Priorización

La consistencia de cupos y la corrección financiera prevalecen sobre una mejora de latencia. Ante incertidumbre se conserva el estado pendiente y se informa al usuario. Las llamadas remotas se ejecutan fuera de transacciones de inventario para evitar bloqueos prolongados.

## Decisión rectora

**Un único backend modular, con datos transaccionales compartidos y procesos de ejecución diferenciados.** API y workers usan la misma versión del artefacto. Se escala mediante réplicas y capacidad de infraestructura antes de considerar separación de servicios.

No se incorporan Lambda, Vercel y Render como runtimes simultáneos obligatorios. Render es la base de cómputo propuesta; Lambda y Vercel son alternativas con condiciones propias. Cloudflare aporta protección y distribución sin desplazar reglas de negocio al borde.

## Criterios para reconsiderar decisiones

Reevaluar el stack cuando el costo observado, la latencia regional, el presupuesto de conexiones o una dependencia comercial impidan cumplir objetivos. Extraer módulos solo con evidencia de necesidad de escalado o independencia de entrega; no por anticipación.
