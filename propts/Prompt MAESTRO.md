Prompt maestro para Cursor (MVP E-commerce)
You are an expert frontend architect specialized in React, Material UI, TypeScript, React Router, and Zustand. 
Build a MOBILE-FIRST MVP frontend for an e-commerce strictly following the user flow below.

# USER FLOW (from diagram)
Screens & flow (mobile-first):
- Inicio (Home) → Listado de productos (con filtros, orden y búsqueda)
- Detalle de producto (carrusel de fotos, título, descripción, precio, cantidad, stock)
- Click en “Comprar” → si no logueado: Login → si logueado: Carrito
- Carrito (lista de items, cantidades, precio unitario, total, aviso de costo de envío)
- Formulario de envío (autocompletado si existe; si no, CP/dirección/ciudad/provincia/país)
  - Integrar mock de APIs públicas de autocompletado de direcciones
  - Calcular mock de costo de envío (Correo Argentino)
- Pago:
  - Opción Mercado Pago (mock de preferencia/intención de pago)
  - Opción Transferencia bancaria (generar link de WhatsApp con mensaje prellenado)
- Resultado de pago:
  - Pago exitoso → Pantalla de confirmación
  - Error de pago → Pantalla de error con CTA para reintentar
- Seguimiento de envío (tracking de pedido: estados mock)
- Ofertas exclusivas (sección/cards en Home)

# TECH & QUALITY REQUIREMENTS
- React 18 + Vite
- TypeScript estricto: no `any`, no `unknown`. Usa tipos exhaustivos y utilidades TS.
- React Router DOM v6+ (file-based routes opcional, pero define rutas claramente)
- Zustand para estado global (slices modulares)
- Material UI v5 con styled-components (de MUI) y Theme custom (palette, typography, spacing, breakpoints)
- ESLint + Prettier para TS/React (reglas estrictas; imports ordenados; no unused vars; exhaustive-deps)
- Testing: Jest + React Testing Library (render, interacciones, stores, hooks, guards, servicios)

# ARCHITECTURE & STRUCTURE
Create this file/folder structure:

/src
  /app
    App.tsx
    routes.tsx
    providers.tsx          # ThemeProvider, CssBaseline, RouterProvider, QueryClientProvider (si lo usas)
  /theme
    index.ts               # createTheme + breakpoints (xs mobile-first), palette, typography
    components.ts          # overrides de MUI si aplica
  /pages
    /home
      HomePage.tsx
    /products
      ProductsListPage.tsx
      ProductDetailPage.tsx
    /cart
      CartPage.tsx
    /auth
      LoginPage.tsx        # form email/pass, Google login, registro
    /checkout
      ShippingPage.tsx     # autocompletado + cálculo envío (mock)
      PaymentPage.tsx      # MercadoPago | Transferencia -> WhatsApp link
      PaymentSuccessPage.tsx
      PaymentErrorPage.tsx
    /orders
      TrackingPage.tsx
  /components
    /common                # Header, BottomNav (mobile), Drawer/Filters, SearchBar, ProductCard, PriceTag, QuantitySelector
    /product               # GalleryCarousel, StockBadge
    /forms                 # TextField controlled, Select, AddressForm
  /store
    /slices
      authSlice.ts
      cartSlice.ts
      productsSlice.ts
      uiSlice.ts
    index.ts
    types.ts               # AppState, AppActions
  /services                # *** MOCKS that return Promises ***
    products.service.ts
    auth.service.ts
    shipping.service.ts
    payment.service.ts
    orders.service.ts
    types.ts               # Product, User, Address, CartItem, Order, PaymentIntent, Shipment, etc.
    __mocks__              # objetos mock separados por dominio
  /utils
    formatCurrency.ts
    whatsappLink.ts
    validators.ts
    test-utils.tsx         # wrapper para RTL (Theme + Router + Store)
  /hooks
    useAuthGuard.ts        # redirect if !auth
    useCartTotals.ts
  /guards
    RequireAuth.tsx        # protected routes
  /tests
    app.test.tsx
    routes.test.tsx
    store/
    pages/
    components/
  /assets
    /images (placeholders)

# ROUTING
Define routes aligned to flow:
- "/": HomePage (Ofertas exclusivas + destacados)
- "/products": ProductsListPage (filtros: categorías, precio, estado usado/nuevo; sort: precio +/- , descripción A-Z; search por descripción)
- "/products/:id": ProductDetailPage
- "/cart": CartPage (RequireAuth)
- "/auth/login": LoginPage
- "/checkout/shipping": ShippingPage (RequireAuth)
- "/checkout/payment": PaymentPage (RequireAuth)
- "/checkout/success": PaymentSuccessPage
- "/checkout/error": PaymentErrorPage
- "/orders/tracking/:orderId": TrackingPage (RequireAuth)

Use <RequireAuth> for protected routes; if user is not logged in, redirect to /auth/login and preserve intended path.

# UI/UX & DESIGN
- MOBILE FIRST: Diseñar primero para xs (<=600px). Luego responsive para sm/md/lg utilizando theme.breakpoints.
- Moderno, prolijo, distintivo: usa MUI components con styled() para personalizaciones puntuales (no CSS crudo).
- Header minimal + Bottom Navigation (mobile) con enlaces Home/Productos/Carrito/Perfil.
- Filtros/Orden/Search accesibles desde un Drawer (mobile) y en sidebar/topbar (desktop).
- Cards de producto con imagen (ratio fijo), nombre truncado, precio, badges (usado/nuevo, stock).
- GalleryCarousel en detalle de producto (swipe en mobile).
- Formularios accesibles (labels, aria, helperText, estados de error).

# STATE MANAGEMENT (Zustand)
Create modular slices with selectors and persist where needed:
- authSlice:
  - state: user: User | null, token?: string
  - actions: loginWithEmail, loginWithGoogle, registerWithEmail, logout
- productsSlice:
  - state: products: Product[], filters, sort, search, status: 'idle'|'loading'|'error'
  - actions: fetchProducts, fetchProductById, setFilters, setSort, setSearch
- cartSlice:
  - state: items: Record<productId, CartItem>, subtotal, shippingCost, total
  - actions: addItem(id, qty), removeItem(id), setQty(id, qty), clearCart, computeTotals
- uiSlice:
  - state: drawerOpen: boolean, loading: boolean, toast: {open: boolean; message: string; severity: 'success'|'error'|'info'}
  - actions: openDrawer, closeDrawer, setLoading, showToast, hideToast

# SERVICES (MOCKS)
All services return Promises resolving mock data and simulate latency with setTimeout.
Place mocks in /src/services/__mocks__ and expose strongly-typed functions.

Example types (/src/services/types.ts):
```ts
export type ID = string;
export interface ProductImage { id: ID; url: string; alt: string; }
export interface Product {
  id: ID; title: string; description: string;
  price: number; condition: 'new'|'used';
  images: ProductImage[]; stock: number; category: string;
}
export interface Address {
  street: string; number: string; city: string; province: string; country: string; postalCode: string;
}
export interface User { id: ID; name: string; email: string; addresses: Address[]; }
export interface CartItem { productId: ID; title: string; price: number; qty: number; }
export interface ShipmentQuote { carrier: 'CorreoArgentino'; cost: number; etaDays: number; }
export interface PaymentIntent { id: ID; method: 'MercadoPago'|'Transferencia'; status: 'created'|'approved'|'rejected'; redirectUrl?: string; }
export interface Order {
  id: ID; userId: ID; items: CartItem[]; address: Address; total: number;
  payment: PaymentIntent; status: 'created'|'paid'|'shipped'|'delivered'|'cancelled';
  tracking?: { checkpoint: string; date: string; }[];
}


Service contracts:

products.service.ts

getProducts(params: {category?: string; minPrice?: number; maxPrice?: number; condition?: 'new'|'used'; sort?: 'priceAsc'|'priceDesc'|'titleAsc'; search?: string;}): Promise<Product[]>

getProductById(id: ID): Promise<Product>

auth.service.ts

loginWithEmail(email: string, password: string): Promise<{user: User; token: string}>

loginWithGoogle(): Promise<{user: User; token: string}>

registerWithEmail(data: {name: string; email: string; password: string}): Promise<{user: User; token: string}>

shipping.service.ts

autocompleteAddress(query: string): Promise<Address[]>

quoteShipment(address: Address, items: CartItem[]): Promise<ShipmentQuote>

payment.service.ts

createPaymentIntentMP(total: number): Promise<PaymentIntent> # mock de Mercado Pago (redirectUrl simulado)

createBankTransferIntent(total: number, user: User): Promise<PaymentIntent> # genera texto para WhatsApp link

orders.service.ts

createOrder(payload: {user: User; items: CartItem[]; address: Address; payment: PaymentIntent; total: number;}): Promise<Order>

getOrderById(id: ID): Promise<Order>

WhatsApp link helper (/src/utils/whatsappLink.ts):

buildTransferMessage(user: User, total: number): string // texto prellenado

buildWhatsAppLink(phoneE164: string, message: string): string

PAGES & KEY REQUIREMENTS

HomePage:

Sección Ofertas exclusivas (grid responsive)

Navegación a ProductsList

ProductsListPage:

Drawer (mobile) con filtros (categoría, precio min/max, condición), sort y search

Persistencia de filtros en querystring

ProductDetailPage:

GalleryCarousel (swipe), info clave, selector de cantidad, CTA “Agregar al carrito” y “Comprar ahora”

LoginPage:

Tabs: Ingresar / Registrar

Email+Password (validación), Login con Google (mock)

CartPage (RequireAuth):

Tabla/card list con qty editable, subtotal por ítem, total parcial

ShippingPage (RequireAuth):

Si user.addresses[0] existe → prellenar; si no → mostrar formulario completo

Autocomplete + quoteShipment onChange postalCode

PaymentPage (RequireAuth):

Radio group: MercadoPago | Transferencia

MP: createPaymentIntentMP → redirectUrl (simulado) → luego success/error

Transferencia: mostrar botón “Contactar por WhatsApp” usando link helper

PaymentSuccessPage / PaymentErrorPage:

Mensaje + CTAs (ver pedido / reintentar)

TrackingPage (RequireAuth):

Línea de tiempo con checkpoints mock

THEME (MUI)

createTheme con palette primary/secondary personalizados, tipografía legible, spacing 8-base.

Breakpoints: diseño mobile-first; usa theme.breakpoints.up('sm'|'md'|'lg') para progresivos.

TESTING (Jest + RTL)

Crea tests para:

Rutas protegidas (RequireAuth) → redirección a /auth/login

products.service: filtros/sort/search (mock)

cartSlice: add/remove/setQty/computeTotals

pages críticas (ProductsListPage, ProductDetailPage, ShippingPage, PaymentPage)

Accesibilidad básica: roles, labels, aria-live en toasts

ESLINT/PRETTIER

TS strict, react-hooks/exhaustive-deps, no-explicit-any, no-floating-promises, import/order

Scripts npm:

"lint": "eslint --ext .ts,.tsx src"

"test": "jest --passWithNoTests"

"typecheck": "tsc --noEmit"

"dev", "build", "preview" (Vite)

DELIVERABLES

Generate the full folder/file scaffold with stubs and TODOs where needed.

Implement the routes and pages with responsive MUI layouts (mobile-first).

Implement services with typed mocks returning Promises (simulated latency).

Implement Zustand slices with selectors and tests.

Provide an initial theme and a few component overrides.

Add unit tests (at least smoke + critical interactions).

Provide README.md with how-to run, scripts, decisions, and future TODOs.

Begin by creating the project structure and boilerplate files, then implement pages in the order of the user flow. Ensure no file uses any or unknown. If a type is uncertain, define a precise union or interface.