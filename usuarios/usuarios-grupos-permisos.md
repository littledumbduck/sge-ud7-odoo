# Usuarios, grupos y permisos: Panadería Pancracio

Odoo 19 · Base de datos sin datos de demostración · Módulos: Ventas, Facturación/Contabilidad, Inventario, Contactos.

La plantilla aplica el **principio de mínimo privilegio**. Cada persona tiene solo los grupos que necesita para su puesto. Ningún usuario de negocio tiene acceso a Ajustes ni a Contabilidad completa.

> Por seguridad, este documento no contiene contraseñas. Solo se indica el correo de acceso de cada usuario.

## 1. Plantilla de usuarios

| Usuario | Correo de acceso | Puesto | Tipo | Grupos / privilegios por módulo | Justificación |
|---|---|---|---|---|---|
| Alberto Larios Espina | alberto@pancracio.com | Responsable de ventas | Interno | **Ventas:** Administrador · **Facturación/Contabilidad:** sin acceso · **Inventario:** sin acceso · **Ajustes:** sin acceso | Supervisa a los comerciales, así que necesita ver y gestionar **todos** los presupuestos y pedidos. No factura ni cobra porque eso es tarea de Administración. |
| María Gómez Ruiz | maria@pancracio.com | Comercial | Interno | **Ventas:** Usuario: solo mostrar documentos propios · **Panadería Pancracio:** Comercial junior (grupo propio) · **Facturación/Contabilidad:** sin acceso · **Inventario:** sin acceso | Solo prepara presupuestos y pedidos de sus propios clientes. No ve el menú de Contabilidad y **no puede borrar clientes**. |
| Carmen Navarro Díaz | carmen@pancracio.com | Administración / Facturación | Interno | **Facturación/Contabilidad:** Facturación · **Panadería Pancracio:** Facturación de pedidos (grupo propio) · **Ventas:** sin acceso · **Inventario:** sin acceso | Emite facturas desde los pedidos, rectificativas y registra pagos. No gestiona ventas: no ve el menú Ventas ni puede crear presupuestos. |
| Antonio Ruiz Moreno | antonio@pancracio.com | Encargado de obrador / almacén | Interno | **Inventario:** Usuario · **Ventas:** sin acceso · **Facturación/Contabilidad:** sin acceso | Valida las órdenes de entrega y controla el stock del obrador. Dar Inventario a un usuario de ventas rompería el mínimo privilegio, por eso es un puesto aparte. |
| Cafetería El Buen Desayuno S.L. | pedidos@elbuendesayuno.example | Cliente (empresa) | Portal | **Rol:** Portal | Cliente B2B que consulta desde el portal web sus presupuestos, pedidos y facturas, y puede aceptar o firmar presupuestos. No entra en el backend de Odoo. |

El usuario `Administrator` (admin) solo se usa para la configuración técnica de la base de datos y no forma parte de la plantilla de la empresa.

## 2. Tipos de usuario

- **Interno** (`Role / User`): accede al backend de Odoo (las aplicaciones). Ocupa licencia. Sus permisos dependen de los grupos asignados. Alberto, María, Carmen y Antonio son internos porque trabajan en la empresa.
- **Portal** (`Role / Portal`): usuario externo (cliente o proveedor) que solo accede a `/my` en el sitio web. Ve **únicamente sus propios documentos** (presupuestos, pedidos, facturas). Se ha creado para la Cafetería El Buen Desayuno S.L., que es un cliente recurrente.
- **Público**: visitante anónimo sin iniciar sesión, por ejemplo de la web o la tienda. No se crea ningún usuario de este tipo porque Odoo usa internamente el usuario *Public user*.

## 3. Grupos propios

Se han creado dos grupos con el nombre de la empresa.

### 3.1. «Panadería Pancracio / Comercial junior»

Se creó con el modo desarrollador activado en **Ajustes → Usuarios y compañías → Grupos**.

- **Nombre:** Comercial junior
- **Privilegio (sección):** Panadería Pancracio, dentro de la categoría Ventas
- **Nombre completo:** Panadería Pancracio / Comercial junior
- **Usuarios asignados:** María Gómez Ruiz
- **Descripción:** comercial junior. Ve solo sus propias ventas, puede consultar, crear y editar clientes pero no borrarlos, y consulta el catálogo en solo lectura.

#### Grupos heredados

| Grupo heredado | Motivo |
|---|---|
| Ventas / Usuario: solo mostrar documentos propios | Base mínima para crear presupuestos y pedidos. Las reglas de registro de Odoo limitan la vista a los documentos donde el usuario es el comercial. Este grupo hereda a su vez `Role / User` (usuario interno). |

#### Derechos de acceso (sobre modelos)

| Nombre | Modelo | Lectura (L) | Escritura (E) | Creación (C) | Eliminación (B) |
|---|---|:-:|:-:|:-:|:-:|
| pancracio_comercial_junior_res_partner | Contacto (`res.partner`) | ✅ | ✅ | ✅ | ❌ |
| pancracio_comercial_junior_product_template | Producto (`product.template`) | ✅ | ❌ | ❌ | ❌ |

Los derechos de acceso en Odoo son **acumulativos**: un usuario tiene un permiso si *cualquiera* de sus grupos se lo concede. Por eso María **no** tiene el grupo `Contactos / Creación` (que da permiso de borrar contactos). Así el permiso de eliminación de clientes no le llega por ninguna vía.

### 3.2. «Panadería Pancracio / Facturación de pedidos»

- **Nombre:** Facturación de pedidos
- **Privilegio (sección):** Panadería Pancracio, dentro de la categoría Ventas
- **Usuarios asignados:** Carmen Navarro Díaz
- **Motivo:** el rol estándar de Facturación no puede usar el botón «Crear factura» de un pedido de venta, porque Odoo solo lo permite a usuarios de Ventas. Este grupo le da justo lo necesario para facturar pedidos sin darle acceso a Ventas.

#### Grupos heredados

| Grupo heredado | Motivo |
|---|---|
| Contabilidad / Facturación | Base para crear y publicar facturas y registrar pagos. |

#### Derechos de acceso (sobre modelos)

| Modelo | Lectura (L) | Escritura (E) | Creación (C) | Eliminación (B) |
|---|:-:|:-:|:-:|:-:|
| Asistente de factura anticipada (`sale.advance.payment.inv`) | ✅ | ✅ | ✅ | ❌ |
| Línea de pedido de venta (`sale.order.line`) | ✅ | ✅ | ❌ | ❌ |

## 4. Permisos efectivos comprobados (L/E/C/B)

Resultado de cruzar los derechos de acceso de todos los grupos de cada usuario:

| Modelo | Alberto (Resp. ventas) | María (Comercial) | Carmen (Facturación) | Antonio (Almacén) |
|---|:-:|:-:|:-:|:-:|
| Contactos (`res.partner`) | LEC | **LEC** (sin B) | L | L |
| Pedidos de venta (`sale.order`) | LECB | LEC (solo propios) | LE* | L |
| Facturas (`account.move`) | L | L | LECB | – |
| Pagos (`account.payment`) | – | – | LECB | – |
| Productos (`product.template`) | L | L | L | L |

\* Carmen tiene lectura/escritura técnica sobre pedidos porque Facturación necesita actualizar su estado de facturación. No puede crearlos y no tiene el menú Ventas.

## 5. Demostración (vídeo)

1. **María (comercial)**: solo ve las apps Ventas y Contactos (y Conversaciones). En Ventas → Pedidos ve únicamente sus presupuestos. **No** aparece el menú Facturación/Contabilidad. Al intentar *Acción → Eliminar* sobre un cliente, Odoo muestra un error de acceso.
2. **Alberto (responsable de ventas)**: en Ventas ve los presupuestos de todos los comerciales, incluidos los de María, y tiene el menú Configuración de Ventas. No tiene acceso a Facturación.
3. **Carmen (facturación)**: ve Facturación, puede crear y confirmar facturas y **registrar pagos**. **No** ve la app Ventas.
4. **Antonio (almacén)**: ve Inventario y puede validar entregas. **No** ve Ventas ni Facturación.
5. **Portal (Cafetería El Buen Desayuno)**: al iniciar sesión entra en `/my` y solo ve sus presupuestos, pedidos y facturas.