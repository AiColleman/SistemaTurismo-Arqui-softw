# 05 · Restricciones y supuestos

## Restricciones del proyecto

| ID | Restricción | Efecto sobre el diseño |
| --- | --- | --- |
| R01 | Entrega académica: diseño y MVP acotado | Distinguir arquitectura objetivo de funcionalidades implementadas |
| R02 | Arquitectura monolítica modular solicitada | Un backend de negocio con módulos internos y liberación conjunta |
| R03 | Nube administrada, sin infraestructura física propia | Usar PaaS, base gestionada, CDN y almacenamiento externo |
| R04 | Costo inicial mínimo | Utilizar cuotas gratuitas en pruebas; presupuestar producción |
| R05 | Un único consolidador aéreo en MVP | Encapsular proveedor en un adaptador y posponer múltiples GDS |
| R06 | Culqi como pasarela seleccionada | Separar llaves de prueba/producción y validar medios habilitados |
| R07 | Conectividad móvil intermitente | PWA con borradores y revalidación; nunca cobro offline |
| R08 | Inventario consistente | PostgreSQL es fuente de verdad; Redis no decide disponibilidad final |
| R09 | Datos de pago sensibles fuera del sistema | Checkout/tokenización del proveedor y registros sanitizados |
| R10 | Autenticación OAuth y permisos por agencia | Proveedor de identidad más RBAC y alcance de recurso en backend |

## Reconciliación tecnológica

El PDF propone **Next.js, NestJS, PostgreSQL/PostGIS y Supabase**. La conversación anterior citó un repositorio Laravel/React/MariaDB. Se adopta aquí el stack del PDF para la arquitectura objetivo; una eventual migración de código requerirá inventario técnico, pruebas y plan de datos propios. Esta entrega no realiza esa migración.

La versión Next.js 14 mencionada en el PDF es una referencia de su propuesta. La construcción seleccionará versiones mantenidas y compatibles, fijadas mediante lockfile; no se declara vigente una versión histórica por aparecer en el documento.

## Límites económicos y operativos

- Render Free se reserva para demostración. El despliegue comercial necesita cómputo activo y workers adecuados.
- Vercel Hobby no se usa como base del comercio; Vercel comercial es una alternativa presupuestada.
- Supabase Free permite prototipar, pero sus límites y ausencia de SLA no respaldan por sí solos RNF01.
- El alcance WAF depende del plan Cloudflare. La API incorpora controles propios para complementar el borde.
- SMS, dominio, comisiones Culqi, convenios aéreos y excesos de consumo tienen costos potenciales.
- AWS Lambda es una alternativa futura y tiene servicios asociados; no se ofrece como despliegue gratuito garantizado.



## Supuestos por validar antes de construcción

| ID | Supuesto propuesto | Validación necesaria |
| --- | --- | --- |
| S01 | Operación multiagencia con aislamiento lógico | Confirmar si el producto comienza con una agencia o varias |
| S02 | Hold inicial de 10 minutos | Acordar plazo por medio de pago y tipo de servicio |
| S03 | PostgreSQL representa todos los cupos de tours comercializados | Definir sincronización de ventas por canales externos |
| S04 | Disponibilidad de API del consolidador | Validar sandbox, límites, emisión, cancelaciones y contrato |
| S05 | Medios Culqi contratados cubren el lanzamiento | Verificar activación por cuenta y flujo real de cada medio |
| S06 | Email disponible como canal mínimo | Configurar proveedor transaccional, dominio y límites |
| S07 | Política de cancelación aprobada | Definir plazos, penalidades y autoridad para devoluciones |

Una disponibilidad local no garantiza ausencia de sobreventa si otros canales consumen cupos sin sincronización. Ese riesgo debe resolverse contractualmente y mediante un flujo de actualización del inventario.
