# Oportunicars · demo profesional

Aplicación de demostración para la operación de una automotora chilena, desde la adquisición hasta el cobro y la postventa. **Todos los registros visibles son ficticios. No maneja credenciales, no emite DTE y no realiza pagos ni publicaciones reales.**

El proyecto vive en [`demos/oportunicars`](demos/oportunicars). La rama `demo/oportunicars` mantiene el trabajo aislado. La rama `gh-pages` contiene únicamente el resultado compilado de la demo. No se modifica el sitio comercial aviarockets.cl ni su DNS.

Se creó este repositorio en `joacoAviles/aviarockets.cl`, cuenta disponible, después de que `Oportuniti/aviarockets.cl` no pudiera resolverse y el usuario autorizara crearlo. No se presupone pertenencia a una organización Oportuniti.

## Ejecución

Node.js 22.12+ o 24 LTS recomendado.

```sh
cd demos/oportunicars
npm ci
npm test
npm run dev
npm run build
```

La demo compilada es estática y funciona en un subdirectorio. `npm run preview` permite comprobar `dist`. No requiere `.env`, API keys, bases de datos ni servicios externos.

## Qué incluye

- Dashboard por perfil, capital propio, contribución esperada y realizada, antigüedad y costo diario del stock.
- Oportunidades nuevas: propio, consignación o retoma; autorización CEO e ingreso operativo Admin.
- Inventario filtrable y ficha por vehículo: toma, preparación, gastos, documentación, publicación y cierre.
- Órdenes de trabajo, proveedores, gastos, aprobación, pago simulado y conciliación.
- Arriendo de oficina, estacionamientos y otros costos recurrentes; contribución por unidad separada de gastos generales.
- CRM con creación de leads, avance de contacto/visita, reservas únicas y vigencia.
- Financiamiento ilustrativo, cuota con tasa supuesta y revisión CFO. No oferta crediticia.
- Venta condicionada, retiro de publicaciones, generación de saldo por cobrar y liquidación al consignante.
- Borradores DTE sin validez tributaria, comisiones liberables al cobrar, casos de postventa.
- Conciliación uno a uno con coincidencia de referencia y monto, sin duplicados; diferencias sin respaldo permanecen pendientes.
- Radar de precios, mediana ficticia, costo de esperar 30 días y recomendación transparente de compra/venta.
- Exportación CSV, bitácora, reinicio del escenario y persistencia exclusivamente local.
- Roadmap con 15 proveedores/capacidades, fuentes y requisitos; ninguna conexión se presenta como activa.

## Base operacional del Excel

Se inspeccionó `FLUJO OPORTUNICARS.xlsx` recuperado desde el adjunto de la conversación original: **130 hojas, 13.225 celdas con contenido y 2.315 fórmulas**, incluyendo fórmulas matriciales y valores guardados. El archivo no se publica. El análisis no recalculó el Excel en Microsoft Excel y no certifica sus resultados históricos.

El consolidado relaciona registros por patente con 128 fichas; `MES 0` reúne ingresos, egresos, centros de costo, origen de fondos y contrapartes. Las fichas contienen modalidad/estado mezclados en “Ubicación”, toma, gastos, preparación, fotos, publicación y precios. Se separaron modalidad, estado y posición física en el diseño objetivo; la demo usa Casa matriz y costos de estacionamiento, sin plano de plazas.

Ver [análisis y decisiones](demos/oportunicars/docs/EXCEL-MODEL.md), [arquitectura y modelo](demos/oportunicars/docs/ARCHITECTURE.md), [integraciones](demos/oportunicars/docs/INTEGRATIONS.md) y [recorrido de validación](demos/oportunicars/docs/DEMO-GUIDE.md).

## Perfiles y responsabilidades

| Perfil | Acciones permitidas | Control |
|---|---|---|
| Admin | Crear oportunidad y lead, ingresar vehículo autorizado, registrar gastos, completar OT/documentos, publicar, reservar, cerrar venta, postventa | No aprueba sus gastos, no confirma bancos, no cambia precios |
| CEO | Autorizar adquisición y ajustar precio; vistas de rentabilidad y mercado | No opera cobros/pagos; no altera precio de reserva |
| CFO | Aprobar gastos, registrar pago simulado, conciliar, revisar financiamiento, crear borrador DTE y liberar comisión | No publica ni ejecuta venta |

Las tres vistas permiten lectura de la información demo y priorizan trabajo distinto. `can()` y `transition()` comprueban permisos para cada mutación, además de deshabilitar controles. **Es RBAC de simulación en cliente, no seguridad productiva**: el selector cambia de perfil sin autenticación. Datos en `localStorage` son editables por el visitante; auditoría local no es inmutable.

## Cálculos y límites

El porcentaje de margen original es retorno sobre costo: `(precio − toma − gastos) / (toma + gastos)`. La demo conserva esa lectura y agrega contribución: `precio − toma/liquidación − gastos directos − parking − capital − comisión`. El margen sobre venta divide esa contribución por el precio.

Consignación excluye inversión propia y costo de capital. La liquidación al propietario se descuenta una sola vez; al vender genera una obligación, no un gasto duplicado. Comisión comercial y remuneración por intermediación son conceptos separados. Costos e ingresos son importes gerenciales brutos demo: el tratamiento de IVA y tipo DTE se deja pendiente de validación contable. La contribución no equivale a utilidad neta ni a flujo de caja.

El escenario se fija al **05-10-2026**; no cambia con el reloj. Aparcamiento por días y capital a tasa anual simple (12% supuesta); la antigüedad de vendidos se congela a la venta. Los contratos mensuales de parking muestran capacidad y no se restan una segunda vez. El estado gerencial es ilustrativo: no sustituye cierre contable, inventario valorizado, libro mayor o estados financieros certificados.

## Referencias funcionales

De [GoAuto](https://www.goauto.cl/) se tomó como referencia pública inventario, CRM, publicación y alertas de rentabilidad; no se copió su aplicación ni se verificaron sus conexiones privadas. [Nubox](https://www.nubox.com/software-contabilidad) informa centralización y propuestas de conciliación, y [Chipax](https://www.chipax.com/) describe seguimiento financiero. La demo agrega controles de aprobación y detalle de costos identificados en el Excel. [Bsale](https://docs.bsale.dev/) y [Defontana](https://apiintegraciones.defontana.com/inicio) se consideran posibles conectores, sujetos a validación comercial y técnica.

## Publicación y verificación

Preview prevista: https://joacoaviles.github.io/aviarockets.cl/ . GitHub Pages sirve el contenido estático de `gh-pages`; el nombre del repositorio no configura un dominio propio.

Pruebas de dominio: `npm test` (9 casos con múltiples comprobaciones). Compilación: `npm run build`. Se comprueba navegación de las nueve pantallas en escritorio y móvil, perfiles, cierre de venta, conciliación, creación de gastos y persistencia. Ver detalles en la guía. No se validan conexiones reales, DTE tributarios o pagos porque están expresamente fuera de esta demo.
