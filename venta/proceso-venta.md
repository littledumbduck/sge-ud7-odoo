# Proceso de venta completo: Panadería Pancracio

Cada paso lo ejecutó el usuario que corresponde a su rol, no el administrador.

**Cliente:** Cafetería El Buen Desayuno S.L.

## Paso a paso

| # | Paso | Usuario (rol) | Documento | Estado final |
|---|---|---|---|---|
| 1 | Crear presupuesto con 3 líneas | María Gómez (Comercial) | S00001 | Presupuesto enviado |
| 2 | Confirmar el presupuesto | Alberto Larios (Responsable de ventas) | S00001 | Pedido de venta |
| 3 | Validar la entrega (cantidad hecha = pedida) | Antonio Ruiz (Encargado de obrador) | WH/OUT/00001 | Hecho |
| 4 | Crear la factura, publicarla y registrar el pago | Carmen Navarro (Administración) | INV/2026/00001 · pago PBNK1/2026/00001 | Publicado · Pagado |
| 5 | Comprobar el estado final del pedido | — | S00001 | Totalmente facturado |

## 1. Presupuesto

| Producto | Cantidad | Descuento | Importe |
|---|--:|--:|--:|
| Pan de masa madre (barra 500 g) | 20 | 10 % | 57,60 € |
| Croissant de mantequilla | 30 | — | 54,00 € |
| Tarta de chocolate (8 raciones) | 2 | — | 48,00 € |

- Condiciones de pago: 30 días.
- Validez y vencimiento: 12/12/2026.
- No hay servidor de correo configurado, así que el presupuesto no se envió por email. Se generó el PDF (`presupuesto.pdf`) y se marcó como enviado.

## 2. Confirmación

Alberto confirma el presupuesto S00001 y pasa a pedido de venta. Se genera automáticamente la orden de entrega.

## 3. Entrega

Antonio valida `WH/OUT/00001` con cantidad hecha igual a la pedida. El stock del pan de masa madre baja de 50 a 30 unidades. Odoo preguntó por el envío de un SMS al cliente y se eligió no enviarlo.

Antonio es un usuario de Inventario. Se creó para esta tarea porque ni María ni Alberto ni Carmen tienen permiso de Inventario, y dárselo a ventas rompería el principio de mínimo privilegio.

## 4. Facturación y pago

Carmen crea la factura desde el pedido, la revisa y la publica (`factura.pdf`). Registra el pago en el diario Banco con fecha 12/11/2026. La factura queda como **Pagado**.

| Concepto | Importe |
|---|--:|
| Base imponible | 159,60 € |
| IVA 4 % | 2,30 € |
| IVA 10 % | 10,20 € |
| **Total** | **172,10 €** |

## 5. Estado final

El pedido S00001 aparece como **Totalmente facturado**.

## Documentos

- `venta/presupuesto.pdf`: impreso desde Odoo.
- `venta/factura.pdf`: impresa desde Odoo.
