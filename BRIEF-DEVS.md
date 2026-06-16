# Nativa — Brief para desarrollo

> Documento de arranque para los programadores. Convierte el prototipo (deck + web estática) en el sitio real con backend, pagos, suscripciones y automatizaciones.
>
> Referencias: web prototipo (`index.html`), costos (`Costos Nativa.xlsx` › hoja Precios).

## 1. Qué es Nativa

Delivery premium de frutas y verduras por **suscripción mensual**, zona norte de Buenos Aires (Nordelta, Villanueva, Tigre, Maschwitz, Escobar, Pilar). El cliente elige un cajón, se suscribe y recibe una entrega semanal curada que rota con la estación. Lanzamiento objetivo: **julio 2026**.

## 2. Alcance del MVP — dos flujos

1. **Flujo cliente** — web pública: descubre, elige, paga y administra su suscripción.
2. **Flujo operación (Aldana)** — panel interno: ve pedidos, arma la lista de compras consolidada, cobra y concilia.

Todo lo que hoy en el prototipo es manual o "a pedido" tiene que quedar automatizado.

## 3. Flujo cliente

### 3.1 Descubrimiento
- Entrada principal por Instagram. El perfil/bio debe llevar a la **web** y al **WhatsApp Business**.
- La web tiene que poder cerrar la compra sin fricción desde el celular.

### 3.2 Elegir el cajón
- Planes base: **Mini** (1–2 personas), **Family** (3–4, el más elegido), **Harvest** (5+), más versiones **Fruit** (solo fruta, para oficinas).
- Mostrar el contenido del cajón (base fija + estacional que rota).
- **Extras / productos sueltos (+10%):** a cualquier cajón se le suman productos sueltos (palta, huevos, frutas extra, berenjena, etc.) con un 10% sobre el precio Nativa del producto.
- **Cajón a medida (+15%):** el cliente arma el cajón producto por producto; recargo del 15% por producto, mínimo equivalente a Mini.
- Regla de precios horneada (ver §6).

### 3.3 Suscripción
- La suscripción es **mensual** (4 entregas/mes), por adelantado, cancela cuando quiere.
- El mix siempre está presente y rota con la estación.
- **Cajón sorpresa premium cada 3 meses** para todos los suscriptores.
- **A definir:** ¿la suscripción es lo mismo que "iniciar sesión"? Definir el modelo de cuenta del cliente (registro, login, gestión de la suscripción, pausas, alergias/exclusiones).

### 3.4 Checkout
- Al agregar al carrito, mostrar el **plazo de entrega según la zona** (día fijo por zona).
- **Medios de pago:**
  - Transferencia o efectivo → **con descuento**.
  - Tarjeta de crédito y Mercado Pago (link) → **un poco más caro** (absorbe el costo de la pasarela).
- Al elegir el medio de pago, enviar **automáticamente el link de pago** (Mercado Pago) o los **datos de transferencia (CBU/alias)**.
- Cobro mensual recurrente (suscripción).

### 3.5 Captación y fidelización
- **Pop-up "Suscribite"** al entrar a la web: captura de email para ofertas y descuentos.
- **Mail de bienvenida** automático con **cupón de primera compra**.
- El primer contacto por **Instagram** (DM) también debe disparar un mensaje automático de bienvenida (referencia: GoFruit).
- **Códigos de descuento por influencer** (ej. `Aldi10`): el sistema aplica el descuento y además registra cuántos clientes trajo ese código para medir **rentabilidad por influencer** (CAC, conversión, recurrencia).

## 4. Flujo operación (Aldana)

### 4.1 Pedidos
- Panel con **pedidos nuevos + suscripciones activas/remanentes** (no solo las nuevas), para armar la operación completa de la semana.
- Aviso de cada suscripción nueva al panel y al teléfono.

### 4.2 Lista de compras consolidada
- A partir de todos los pedidos de la semana, calcular el **total a comprar por producto**, expresado en la unidad de compra real: **cajones / bultos / unidades** (no solo kg).
- Usa el rinde por bulto (ya modelado en el deck: `precio bulto ÷ rinde`).

### 4.3 Cobro y conciliación
- Ver quién está al día con la suscripción.
- **Conciliación bancaria**: cruzar avisos de pago contra los movimientos de la cuenta.

### 4.4 Precios de costo
- **Linkear los precios de verdulería con COTO / JUMBO** (feed o scraping) para mantener costos y márgenes actualizados sin cargarlos a mano.

## 5. Integraciones
- **WhatsApp Business + bot**: coordina el primer cajón y responde sin intervención manual.
- **Instagram**: DM automático de bienvenida.
- **Mercado Pago**: links de pago y suscripción recurrente.
- **Transferencia**: generación de datos CBU/alias y matcheo con la conciliación.
- **Email** (transaccional): bienvenida, cupones, comprobantes.
- **COTO / JUMBO**: fuente de precios de referencia.

## 6. Reglas de negocio (precios)
- Precio del cajón = costo unitario × markup, redondeado.
- **Recargo cajón a medida: +15%** sobre el precio del cajón estándar.
- **Recargo productos sueltos: +10%** sobre el precio Nativa del producto.
- **Descuento** por transferencia/efectivo; **recargo** por tarjeta/Mercado Pago.
- Mínimo del cajón a medida: equivalente a Mini.
- Cupones de descuento por código, con atribución a influencer.

## 7. Modelo de datos (entidades base)
- **Producto**: nombre, unidad, costo (d1), valor verdulería (d2), bulto, rinde, precio bulto (pb).
- **Cajón / Plan**: nombre, descripción, items (producto + cantidad), tamaño.
- **Cliente**: datos, zona, alergias/exclusiones, cuenta.
- **Suscripción**: plan, estado, ciclo de cobro, pausas, extras fijos.
- **Pedido semanal**: cliente, cajón, extras/sueltos, a medida, zona, fecha de entrega, estado de pago.
- **Cupón**: código, descuento, influencer, métricas de uso.
- **Pago**: medio, monto, estado, conciliación.

## 8. No funcionales
- **Responsive** (celular y compu) — prioridad celular.
- **Dominio**: **`nativamarket.com.ar`** (registrado). Falta apuntar el DNS a GitHub Pages.
- Hosting/infra a definir con el equipo de dev.

## 9. Fases sugeridas
1. **MVP:** catálogo + suscripción + checkout (transferencia/MP con link automático) + panel de pedidos + lista de compras consolidada + responsive + dominio.
2. **V2:** cajón a medida y sueltos online, cupones con atribución de influencer, pop-up + mail de bienvenida, bot de WhatsApp, DM automático de IG.
3. **V3:** conciliación bancaria automática, precios linkeados a COTO/JUMBO, analytics de rentabilidad por influencer.

## 10. Preguntas abiertas
- ¿Suscripción = login? Definir el modelo de cuenta.
- ¿Cuántas coliflores (y unidades sin rinde claro) trae cada bulto del mayorista?
- ¿TikTok además de Instagram para influencers/recetas?
