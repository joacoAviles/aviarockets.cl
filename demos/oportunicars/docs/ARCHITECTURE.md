# Arquitectura y modelo

## Implementación

React + Vite, Lucide para iconos, CSS responsive. Sin backend. `src/domain.js` concentra datos ficticios, reglas y cálculos; `src/main.jsx` contiene las vistas y formularios; `src/integrations.js` registra el roadmap. `transition(state, role, action, payload)` valida permiso y precondiciones, clona el estado y registra el evento. Una mutación rechazada no modifica el estado original.

Persistencia por navegador bajo `oportunicars-demo-v1`; sin sincronización entre usuarios o dispositivos. El selector de perfil no inicia sesión. Las fuentes de Google Fonts son el único recurso externo de presentación; no se envían datos del escenario. Las conexiones de negocio están ausentes.

## Entidades implementadas

| Entidad | Clave / relación | Contenido |
|---|---|---|
| Vehicle | id DEMO, no patente real | marca, modelo, versión, año, km, modalidad, estado, fechas, precio y compra/liquidación |
| Cost | id, vehicle | concepto, proveedor ficticio, monto, vencimiento, estado; settlement distingue liquidación de gasto |
| WorkOrder | id, vehicle, cost | tarea, proveedor, ejecución; al completar todas libera preparación |
| Lead | id, vehicle | nombre ficticio, canal, etapa y próxima acción |
| Reservation | id, vehicle, lead | anticipo, precio congelado, vencimiento, estado |
| Financing | id, vehicle | monto, plazo, tasa supuesta y estado de revisión |
| Receivable | id, vehicle | obligación, importe y cobros confirmados |
| BankMovement | id, reference, matched | cartola ficticia y relación uno a uno con documento |
| RecurringCost | id | contrato mensual, proveedor, monto y marca de estacionamiento |
| DTE draft | id, vehicle | monto y clasificación tributaria pendiente |
| Commission | id, vehicle | liquidación demo después de cobro completo |
| PostSale | id, vehicle | caso abierto posterior a venta |
| AuditEvent | id, actor | fecha real de acción, comando y payload; bitácora no inmutable |

Los proveedores están normalizados sólo por nombre en esta demo; producción usaría claves propias. Una retoma se modela como modalidad de adquisición, sin compensación automática contra otra venta. El precio de mercado es una mediana ficticia fija, con tres comparables ilustrativos derivados. No existe un motor de tasación entrenado ni acceso a datos reales.

## Estados y controles

Oportunidad → autorización CEO → ingreso Admin → Preparación → publicación con checklist → Publicado → reserva única → Reservado → venta con requisitos → Vendido.

Gasto pendiente → aprobación CFO → registro de pago simulado → conciliación CFO → Pagado. El costo afecta contribución desde su registro; el pago sólo cambia tesorería. Gastos posteriores a venta se bloquean en este alcance; ajustes contables y notas de crédito requerirían un flujo adicional.

Reserva → anticipo por cobrar + movimiento ficticio → conciliación → venta → saldo por cobrar + movimiento ficticio. Financiamiento no reemplaza saldo por cobrar: la transferencia de la financiera debe quedar confirmada; la demo agrupa el saldo para una conciliación exacta. No implementa fraccionamiento, devoluciones ni match muchos-a-muchos.

Consignación: se pacta una liquidación fija al ingreso. Diferencia entre precio de cierre y liquidación representa ingreso gerencial de intermediación antes de gastos; ajustes del contrato al precio requieren un proceso futuro. No se contabiliza el valor del vehículo como capital propio.

## Evolución productiva propuesta

Separar frontend/API/servicios de dominio; PostgreSQL con IDs, restricciones y transacciones; RBAC servidor y permisos por sucursal; autenticación y MFA; almacenamiento cifrado de documentos; bitácora inmutable; outbox y webhooks idempotentes. Proveedores, contratos, bancos, movimientos, asientos y documentos tendrían claves normalizadas. DTE requiere proveedor habilitado o certificación aplicable y validación de tratamiento por operación. Incluir cierre por período, reconciliación de auxiliares, correcciones, anulaciones, partidas divididas y conciliación muchos-a-muchos.

Nada de lo anterior se presenta como implementado en la demo.
