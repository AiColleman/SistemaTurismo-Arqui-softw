# 03 · Requisitos funcionales


## Catálogo de requisitos

Se conserva la numeración del PDF para facilitar su evaluación y trazabilidad.

| Código | Función | Comportamiento requerido | Módulo responsable |
| --- | --- | --- | --- |
| RF01 | Identidad y roles | Correo, Google y Facebook mediante OAuth; turista, operador/agencia, guía y administrador. | Identidad |
| RF02 | Catálogo | Alta y administración de tours y paquetes con fotografías, itinerario, encuentro, duración, precio y cupos por salida. | Catálogo |
| RF03 | Búsqueda | Filtros por destino, categoría, rango de fechas, precio y duración; disponibilidad actualizada. | Catálogo / Inventario |
| RF04 | Reserva | Fecha y pasajeros; bloqueo temporal de cupos durante el pago. | Reservas / Inventario |
| RF05 | Cobro y confirmación | Pasarela para medios habilitados; aprobación verificable, confirmación automática y voucher. | Pagos |
| RF06 | Disponibilidad | Calendario de salidas, cambios de capacidad, cierre manual y salidas privadas. | Inventario |
| RF07 | Notificaciones | Confirmación, recordatorios, cambios y cancelaciones mediante push, email y/o SMS. | Notificaciones |
| RF08 | Administración | Panel de tours, precios, guías, ventas y ocupación. | Administración / Reportes |
| RF09 | Asignación de guías | Agenda y asignación sin solapamiento horario. | Guías |
| RF10 | Reseñas | Calificaciones posteriores al tour visibles en su ficha. | Reseñas |
| RF11 | Mapa | Atractivos, encuentros y rutas; filtros por zona y categoría. | Geografía |
| RF12 | Mensajería y devoluciones | Conversación turista-operador; solicitud explícita de cancelación y reembolso. | Atención / Pagos |
| RF13 | Pasajes aéreos | Búsqueda, cotización y venta mediante un consolidador/GDS; emisión electrónica tras pago. | Vuelos |

## Reglas de negocio propuestas

| Código | Regla | Aplicación |
| --- | --- | --- |
| RN01 | Cupos confirmados + holds activos no superan la capacidad | Transacción de base de datos y bloqueo de la salida |
| RN02 | El hold tiene expiración explícita | Duración inicial propuesta: 10 minutos, configurable y no fijada por el PDF |
| RN03 | Precio e importe se calculan en servidor | Guardar tarifa, moneda e impuestos aplicables como snapshot de compra |
| RN04 | Confirmación requiere pago aprobado verificado y cupo vigente o readquirido | Un cobro tardío sin disponibilidad se compensa |
| RN05 | Cancelar y devolver son procesos relacionados con estados separados | No declarar devolución completada antes de verificación del proveedor |
| RN06 | La guía no puede tener intervalos de trabajo solapados | Restricción de rango temporal o serialización por guía |
| RN07 | Solo reservas completadas permiten reseñar | Una reseña por reserva elegible |
| RN08 | Las escrituras son idempotentes cuando hay riesgo de reintento | Clave por actor y operación; payload diferente con misma clave produce conflicto |
| RN09 | El inventario aéreo pertenece al consolidador | Revalidar tarifa y respetar su límite de emisión |
| RN10 | Toda modificación financiera o de capacidad es auditable | Actor, entidad, antes/después permitido y motivo |

## Aclaraciones de alcance

- **Medios de pago:** el PDF menciona tarjeta, Yape/Plin y transferencia. La implementación activa únicamente los medios contratados y comprobados en Culqi; Plin o una transferencia directa no se presuponen como cargos nativos de la API.
- **Voucher:** acredita la reserva o el pago dentro del sistema. No equivale automáticamente a boleta/factura tributaria. La emisión fiscal requeriría una integración y requisitos propios.
- **Tiempo real:** el catálogo puede mostrar una lectura reciente. La disponibilidad definitiva se decide mediante transacción al crear el hold.
- **Salidas privadas:** requieren autorización o enlace restringido y reglas de capacidad; no deben quedar visibles accidentalmente en búsquedas públicas.
- **Vuelos:** un PNR/localizador no equivale a un boleto emitido. Solo se entrega un e-ticket confirmado por el proveedor.


