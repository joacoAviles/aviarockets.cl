# Guía de demostración y validación

## Recorrido de 8 minutos

1. Iniciar como **CEO**. El resumen muestra capital propio (sin consignados), contribución y antigüedad. En Inteligencia de mercado, RAV4 y Tucson activan alertas por precio y tiempo. Ajustar precio recalcula margen y bitácora.
2. Vehículos → Peugeot 2008 → autorizar adquisición como CEO. Cambiar a **Admin** e ingresar a inventario. También se puede crear una oportunidad nueva (propio, consignado o retoma) con datos ficticios.
3. Operación → completar orden de Swift como Admin. Ficha → Documentos → completar antecedentes y fotos; simular publicación. Sin checklist, la acción se rechaza.
4. Ficha → Costos → registrar gasto. Cambiar a **CFO**, Finanzas → aprobar → registrar pago. Conciliación → confirmar correspondencia. El pago no es una transferencia real.
5. Clientes y ventas → crear lead como Admin, marcar contacto, agendar visita y reservar un vehículo publicado. Reserva fija anticipo ficticio de $300.000 y vencimiento 08-10-2026; ambos son supuestos del recorrido. No se permiten reservas duplicadas.
6. Para el Sportage ya reservado: como CFO, aprobar financiamiento simulado y conciliar anticipo AR2. Como Admin, ficha → Venta → cerrar. Se genera un saldo por cobrar, movimiento ficticio y se retiran avisos simulados.
7. Como CFO, conciliar saldo para liberar comisión. Generar borrador DTE sin validez tributaria. Como Admin, abrir caso de postventa del vendido.
8. Revisar bitácora e integraciones. Exportar CSV o reiniciar la demo desde Control y auditoría. Reiniciar sólo afecta este navegador.

## Verificaciones automatizadas

`npm test` ejecuta 9 pruebas de dominio, con casos positivos y rechazos:

- Cálculo independiente de retorno, parking, capital, comisión y contribución.
- Denegación de mutaciones por perfil.
- Reserva, financiamiento, cobro, venta y comisión; duplicados rechazados.
- Consignación sin inversión propia ni duplicación de liquidación.
- Gasto positivo, aprobación, pago y conciliación.
- Checklist, orden de trabajo y autorización de adquisición.
- Reprecio recalcula margen; no cambia el precio de una reserva.
- DTE único y casos de postventa sólo para vendidos.
- Creación de oportunidades y avance CRM con auditoría.

`npm run build` verifica compilación de distribución. Verificación con navegador: las 9 pantallas bajo los 3 perfiles a 1440 px; las 9 pantallas a 390 px, sin desborde horizontal de la página; tablas y kanban tienen desplazamiento propio. Flujos ejercitados: conciliación, revisión de crédito, venta, búsqueda de vehículo, alta de gasto, persistencia y cierre con Escape. No se trata de una certificación integral de accesibilidad ni de pruebas de sistemas productivos.

## Límites explícitos

- Usuarios representados mediante selector, sin credenciales ni servidor.
- Facturación simulada y tratamiento tributario pendiente; sin folios, XML fiscal o firma.
- Sólo una cartola demo y conciliación exacta uno-a-uno; faltan splits, devoluciones y multi-moneda.
- Financiamiento usa supuestos; sin evaluación real de riesgo ni conexión bancaria.
- Arriendos/proveedores se presentan como registros de referencia; no hay CRUD completo de contratos ni programación real de pagos recurrentes.
- Retoma no liquida automáticamente una segunda venta; documentación es checklist agregado y no repositorio de documentos.
- Comparables y recomendaciones son ficticios, de reglas simples; no scraping, IA remota o tasación certificada.
- Inventario puede crecer desde nuevas oportunidades; los atributos secundarios nuevos usan valores demo explícitos en el formulario.
- Integraciones son roadmap. No se enviaron mensajes, publicaciones, pagos, documentos tributarios o datos a terceros.
