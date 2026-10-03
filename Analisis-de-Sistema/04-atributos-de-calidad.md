# 04 · Atributos de calidad

Los RNF del PDF se transforman en criterios comprobables. Las metas son objetivos de diseño, no resultados de pruebas ni garantías de proveedores.

| Código | Atributo | Meta del PDF | Verificación propuesta | Condición |
| --- | --- | --- | --- | --- |
| RNF01 | Disponibilidad | ≥ 99 % mensual | Medir sintéticamente el acceso al servicio y la API; en 30 días, presupuesto aproximado de indisponibilidad: 7 h 12 min. | Los niveles gratuitos no acreditan SLA comercial. |
| RNF02 | Rendimiento | API p95 < 300 ms; catálogo < 2 s | Evaluar endpoints internos sin espera de Culqi/GDS; medir por separado extremo a extremo. Proponer carga moderada inicial de 50 usuarios activos. | Los 50 usuarios son un perfil de validación propuesto, no dato del PDF. |
| RNF03 | Escalabilidad | Réplicas backend stateless | Aumentar réplicas con el mismo artefacto y mantener sesiones/datos fuera del proceso. | Limitar conexiones y concurrencia de workers antes de saturar PostgreSQL. |
| RNF04 | Seguridad | TLS 1.2+, JWT corto, WAF y proveedor de pagos | Verificar token, audiencia y permisos en cada operación; evitar tarjetas en backend. | La protección perimetral no reemplaza controles de aplicación. |
| RNF05 | Usabilidad | Responsive, WCAG 2.1 AA y borradores offline | Validar teclado, foco, contraste, formularios y recuperación al reconectar. | Un borrador offline no reserva ni mantiene un precio garantizado. |
| RNF06 | Portabilidad | Proyección móvil multiplataforma | Reutilizar contratos REST, reglas y modelos de dominio en clientes futuros. | Flutter no comparte automáticamente el código React/TypeScript. |
| RNF07 | Mantenibilidad | Módulos y documentación C4 | Mantener límites de módulo, contratos y decisiones versionadas. | Los eventos internos no implican microservicios. |
| RNF08 | Costo | Free tiers o bajo costo en MVP | Presupuestar hosting, datos, tráfico, cola, email, SMS y comisión por cobro. | El paso a producción necesita revisar cuotas y contratos. |
| RNF09 | Resiliencia | Degradación controlada | Si falla mapa o avisos, reservar sigue operativo; si falla inventario, cerrar ventas. | Nunca mostrar una compra exitosa para ocultar una caída del proveedor. |
| RNF10 | Observabilidad | Logs, métricas, errores y alertas | Correlacionar reserva, intento de pago, webhook y emisión. | Eliminar secretos y datos personales innecesarios de telemetría. |
| RNF11 | Consistencia | Cero overbooking | Con capacidad 10 y 100 solicitudes simultáneas de un cupo, aceptar como máximo 10 holds activos. | Verificar también expiración, cancelación y duplicados. |
| RNF12 | Interoperabilidad | REST y OpenAPI | Publicar contrato versionado con autenticación, errores, paginación e idempotencia. | Las futuras OTAs son puntos de extensión, no integraciones existentes. |

## Escenarios arquitectónicos prioritarios

| Escenario | Estímulo y entorno | Respuesta esperada | Medida |
| --- | --- | --- | --- |
| EC01 · Venta concurrente | Campaña con múltiples compras para la misma salida | Serializar el inventario y rechazar solicitudes sin capacidad | Cero cupos negativos y cero reservas excedentes |
| EC02 · Pago ambiguo | Culqi no responde después de enviar el cargo | Persistir estado incierto y conciliar antes de otro intento | Ningún reintento ciego de cobro |
| EC03 · Cola no disponible | El commit de compra sucede mientras Redis cae | Conservar evento en outbox y republicarlo al recuperar | Sin pérdida de evento confirmado |
| EC04 · Aviso fallido | Email o SMS rechaza una entrega | Reintentar con límite y dejar tarea operativa | La reserva conserva su estado |
| EC05 · Caída del backend | Catálogo estático sigue en CDN | Mostrar contenido público y suspender nuevas transacciones | Mensaje de estado claro; sin confirmaciones falsas |
| EC06 · Pérdida de conectividad | El turista prepara una reserva en móvil | Guardar borrador local y revalidar al reconectar | Sin holds ni cobros offline |

## Política de medición

Separar latencia interna, latencia de proveedores y carga visual. Medir p95 sobre muestras representativas, declarar región, volumen y duración del ensayo. Para catálogo, fijar un perfil reproducible de red móvil y tamaño de imágenes; «3G/4G típica» debe concretarse antes de aceptar el requisito.

La disponibilidad del flujo de compra depende de autenticación, API, base de datos y pagos. Un SLA aislado no garantiza el SLO global. Registrar resultados reales en cada liberación.
