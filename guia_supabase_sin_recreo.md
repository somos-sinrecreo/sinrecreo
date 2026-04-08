# SIN RECREO · guía rápida para dejar la tienda andando con Supabase

Te dejo una base pensada para este flujo:

- **GitHub Pages** = frontend público
- **Supabase** = catálogo, stock, imágenes y admin
- **Mercado Pago** = link directo por talle al principio
- **WhatsApp / transferencia** = canal alternativo y cierre manual cuando haga falta

## 1) Crear el proyecto en Supabase

1. Entrá a Supabase y creá un proyecto nuevo.
2. Esperá a que termine de aprovisionarse.
3. Copiá estos dos datos:
   - `Project URL`
   - `anon public key`

No pongas nunca la `service_role` en GitHub Pages.

## 2) Crear la base

1. Abrí **SQL Editor**.
2. Pegá completo el archivo `supabase_schema_sin_recreo.sql`.
3. Ejecutalo.

Eso te crea:

- tabla `profiles`
- tabla `products`
- tabla `product_images`
- tabla `product_variants`
- bucket público `product-images`
- políticas RLS para lectura pública y edición solo de admins

## 3) Crear tu usuario admin

1. Abrí `admin_catalogo_supabase.html`.
2. Pegá tu `SUPABASE_URL` y tu `SUPABASE_ANON_KEY` dentro del bloque `CONFIG`.
3. Subí el archivo a GitHub Pages o abrilo localmente.
4. Registrate o iniciá sesión desde Supabase Auth si ya tenés usuario.
5. En Supabase, corré esta consulta cambiando tu mail:

```sql
update public.profiles
set is_admin = true
where email = 'TUEMAIL@DOMINIO.COM';
```

A partir de ahí ya entrás como admin.

## 4) Conectar el storefront

1. Abrí `index_supabase_sin_recreo.html`.
2. Buscá `STORE_CONFIG`.
3. Pegá:
   - `supabaseUrl`
   - `supabaseAnonKey`

Con eso el catálogo público deja de depender del array hardcodeado y empieza a leer desde Supabase.

## 5) Cargar productos

Desde `admin_catalogo_supabase.html` podés:

- crear producto
- cargar cápsula, título, descripción y precio
- subir hasta **3 imágenes**
- poner **stock por talle**
- cargar **un link de Mercado Pago por talle**

### Recomendación de carga
- `slug`: corto y limpio, por ejemplo `voto-femenino`
- `sku`: algo interno, por ejemplo `SR-MA-001`
- `sort_date`: usalo para ordenar drops
- `featured = true`: solo para las prendas protagonistas
- `stock = 0`: el talle no aparece al público

## 6) Mercado Pago sin complicarte la vida

Para empezar, el camino más simple es este:

- en cada talle cargás un **link directo de MP**
- cuando el cliente elige talle, va directo a ese link

Eso te deja vender rápido sin montar backend de pagos todavía.

## 7) Transferencia

La tienda ya está pensada para que mantengas también:

- alias / CBU visibles
- WhatsApp prearmado
- descuento por transferencia

Así no dependés de un solo medio de pago.

## 8) Qué subir a GitHub

Como mínimo:

- `index_supabase_sin_recreo.html` → renombralo como `index.html`
- `admin_catalogo_supabase.html` → podés dejarlo como `admin.html`
- opcionalmente esta guía y el SQL en una carpeta interna del repo

## 9) Mi recomendación de implementación real

### Fase 1
- catálogo en Supabase
- imágenes en bucket
- stock por talle
- links directos de MP
- transferencia por WhatsApp

### Fase 2
- backend propio para crear preferencias de Mercado Pago
- webhook de pago aprobado
- descuento automático de stock
- registro de pedidos

### Fase 3
- panel de pedidos
- estados: pendiente / pago aprobado / enviado / entregado
- código de seguimiento

## 10) Punto delicado

No pude verificar en vivo la documentación más reciente de Supabase ni de Mercado Pago porque acá no tengo navegación web habilitada. La estructura que te dejé sigue el patrón correcto y te acelera muchísimo, pero cuando vayas a cerrar el backend de MP conviene revisar la doc actual de ese paso puntual.

## Archivos del paquete

- `index_supabase_sin_recreo.html`
- `admin_catalogo_supabase.html`
- `supabase_schema_sin_recreo.sql`
