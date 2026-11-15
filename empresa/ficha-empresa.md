# Ficha de empresa: Panadería Pancracio

> Empresa ficticia creada para la tarea UD7. Todos los datos son inventados.

## Datos generales

| Campo | Valor |
|---|---|
| Nombre | Panadería Pancracio |
| Sector | Panadería y pastelería artesanal, con servicio de catering |
| NIF (ficticio) | ESA19283746 |
| Eslogan | Pan de masa madre · Pastelería · Catering |
| Dirección | Avenida del pan 12, 41001 Sevilla |
| Teléfono (ficticio) | 666 123 456 |
| Correo | info@pancracio.com |
| Web | http://www.pancracio.com |
| Logo | [logo.svg](logo.svg) · [logo.png](logo.png) |
| Colores corporativos | `#5e4766` (morado) y `#010101` (negro) |
| Almacén en Odoo | Obrador Pancracio (código `WH`) |

## Actividad

Elaboración y venta de pan de masa madre, bollería y tartas, y servicio de catering para eventos. Vende tanto a cafeterías y negocios como a clientes particulares.

## Configuración en Odoo

- Odoo 19.0 Community, base de datos `database` sin datos de demostración.
- Módulos: Ventas, Facturación/Contabilidad, Inventario, Contactos y localización española (`l10n_es`).
- Plan contable: España – PYMEs 2008. IVA de venta del 21 %, 10 % y 4 %.
- Descuentos en líneas de pedido activados en Ventas → Ajustes.

## Catálogo

| Ref. | Producto | Tipo | Precio | Coste | IVA venta |
|---|---|---|--:|--:|:-:|
| PAN-001 | Pan de masa madre (barra 500 g) | Bienes, almacenable | 3,20 € | 1,10 € | 4 % |
| BOL-001 | Croissant de mantequilla | Bienes | 1,80 € | 0,55 € | 10 % |
| TAR-001 | Tarta de chocolate (8 raciones) | Bienes | 24,00 € | 9,50 € | 10 % |
| SRV-001 | Servicio de catering para eventos (por persona) | Servicio | 15,00 € | 7,00 € | 10 % |

El pan de masa madre tiene stock inicial de 50 unidades en `WH/Stock`. El croissant y la tarta se elaboran en el día, por eso no llevan seguimiento de inventario.

## Clientes ficticios

| Cliente | Tipo | NIF | Dirección |
|---|---|---|---|
| Cafetería El Buen Desayuno S.L. | Empresa | ESB91753129 | Calle Feria 45, 41003 Sevilla |
| Lucía Fernández Ortega | Particular | ES12345678Z | Calle Betis 8, 2.º B, 41010 Sevilla |

Los correos de contacto usan el dominio reservado `.example`, así que no pertenecen a nadie real.
