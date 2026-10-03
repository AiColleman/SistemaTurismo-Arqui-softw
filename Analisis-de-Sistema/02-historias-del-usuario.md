# 02 · Historias de usuario

Las historias descomponen el alcance del PDF. Sus criterios concretan el comportamiento propuesto y no son evidencia de funcionalidades ya implementadas.

## HU01 · Identidad

**Como** turista, **quiero** registrarme e ingresar por correo, Google o Facebook, **para** gestionar mis reservas.

**Trazabilidad:** RF01.

**Criterios de aceptación:** El inicio válido crea una sesión; credenciales inválidas se rechazan; cambiar el rol desde el cliente no altera permisos.

## HU02 · Publicación

**Como** operador, **quiero** publicar un tour con fotos, itinerario, punto de encuentro, duración y precios, **para** ofrecer información completa.

**Trazabilidad:** RF02.

**Criterios de aceptación:** No se publica sin campos obligatorios; solo el propietario modifica; cambios conservan auditoría.

## HU03 · Búsqueda

**Como** turista, **quiero** filtrar por destino, categoría, fechas, precio y duración, **para** encontrar una oferta disponible.

**Trazabilidad:** RF03.

**Criterios de aceptación:** Los filtros combinan correctamente; hay paginación; disponibilidad se revalida al reservar.

## HU04 · Reserva

**Como** turista, **quiero** seleccionar salida y pasajeros y obtener un hold temporal, **para** completar el pago sin perder el cupo.

**Trazabilidad:** RF04.

**Criterios de aceptación:** El hold tiene vencimiento; dos solicitudes concurrentes no exceden capacidad; un reintento no crea otra reserva.

## HU05 · Pago

**Como** turista, **quiero** pagar con un medio habilitado de Culqi, **para** recibir confirmación y voucher.

**Trazabilidad:** RF05.

**Criterios de aceptación:** El servidor determina importe; la aprobación se verifica; un resultado incierto queda en conciliación; no hay confirmación por callback del navegador.

## HU06 · Calendario

**Como** operador, **quiero** abrir o cerrar fechas y crear salidas privadas, **para** controlar la operación.

**Trazabilidad:** RF06.

**Criterios de aceptación:** Cerrar bloquea nuevas ventas; no elimina reservas existentes; no se reduce capacidad por debajo de cupos comprometidos.

## HU07 · Avisos

**Como** turista, **quiero** recibir confirmaciones, recordatorios y cambios, **para** organizar mi viaje.

**Trazabilidad:** RF07.

**Criterios de aceptación:** El aviso nace después del commit; se respeta consentimiento; fallos se reintentan sin duplicar efectos de negocio.

## HU08 · Panel

**Como** operador, **quiero** consultar ventas, ocupación, precios y guías, **para** tomar decisiones.

**Trazabilidad:** RF08.

**Criterios de aceptación:** Los reportes filtran agencia y fechas; diferencian cobro, devolución y reserva; indicadores declaran su hora de actualización.

## HU09 · Guías

**Como** operador, **quiero** asignar un guía disponible, **para** evitar conflictos de horario.

**Trazabilidad:** RF09.

**Criterios de aceptación:** La asignación considera duración y margen de traslado; una asignación solapada se rechaza incluso con dos operadores simultáneos.

## HU10 · Reseñas

**Como** turista, **quiero** calificar un tour realizado, **para** compartir mi experiencia.

**Trazabilidad:** RF10.

**Criterios de aceptación:** Solo una reserva elegible y propia permite reseña; hay moderación; no se acepta una calificación fuera de escala.

## HU11 · Mapas

**Como** turista, **quiero** visualizar atractivos, puntos y rutas, **para** ubicar la experiencia.

**Trazabilidad:** RF11.

**Criterios de aceptación:** Los filtros cambian elementos del mapa; existe alternativa textual; se muestra atribución del proveedor.

## HU12 · Atención

**Como** turista, **quiero** escribir al operador y solicitar cancelación o devolución, **para** resolver cambios de mi reserva.

**Trazabilidad:** RF12.

**Criterios de aceptación:** Solo participantes leen el hilo; la solicitud registra motivo y estado; una devolución repetida no devuelve más del importe cobrado.

## HU13 · Vuelos

**Como** pasajero, **quiero** cotizar, pagar y obtener un boleto aéreo, **para** completar mi compra.

**Trazabilidad:** RF13.

**Criterios de aceptación:** La tarifa se revalida antes de pagar; se conserva localizador; emisión fallida tras cobro genera conciliación y compensación, nunca un ticket ficticio.

## Historias operativas complementarias

| ID | Historia | Aceptación |
| --- | --- | --- |
| HU14 | Como guía, quiero consultar mis salidas y cambios para preparar el servicio | Solo ve salidas asignadas; horarios y punto de encuentro son consistentes; se limita la información personal |
| HU15 | Como administrador, quiero auditar operaciones para investigar incidencias | Puede buscar por reserva, actor y correlación; la auditoría no expone tokens ni tarjetas |
| HU16 | Como turista con conectividad intermitente, quiero guardar un borrador | El borrador no bloquea cupos ni cobra; al reconectar se actualizan precio y disponibilidad y se solicita confirmación |

## Priorización del prototipo

Primero HU01–HU06 y un canal de HU07; después panel, guías, reseñas, mapas y atención. HU13 se diseña desde el inicio y se valida con simulador hasta disponer de convenio. La priorización no elimina requisitos del diseño completo.
