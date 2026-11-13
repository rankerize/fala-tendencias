# 🏛️ Arquitectura Técnica y Sistema de Diseño — Frontend

Este documento detalla la estructura interna de la aplicación, el ciclo de vida de los datos, la gestión de estado y el sistema de diseño corporativo.

---

## 1. Filosofía de Arquitectura (Zero-Build SPA)

La aplicación fue concebida intencionalmente como una **Single Page Application de arquitectura limpia (Zero-Build)** contenida en [index.html](file:///Users/cesarandresjimenezarci/Documents/faltendencias/index.html).

### Ventajas de este enfoque:
* **Sin dependencias pesadas de compilación:** No requiere Webpack, Vite, Node modules ni pipelines largos de CI/CD.
* **Despliegue ultrarrápido:** Vercel la sirve de forma estática en CDN global con latencia inferior a 25ms.
* **Cero riesgo de vulnerabilidades de dependencias:** Las librerías de terceros (`@supabase/supabase-js`, fuentes de Google Fonts) se cargan desde CDNs seguras y versionadas.
* **Mantenibilidad directa:** Cualquier desarrollador puede clonar el repositorio y correrlo inmediatamente sin `npm install`.

---

## 2. Ciclo de Vida y Flujo de Datos

```
[ Inicia la aplicación ]
        │
        ▼
[ Carga SDK Supabase & Assets ]
        │
        ▼
[ Consulta paginada a Supabase (.range()) ]
  - Trae en lotes de 1,000 registros
  - Se consolida en `masterData` (~7,530 elementos)
        │
        ▼
[ Poblado Dinámico de Selectores (Cascada) ]
  - Se calculan conteos reales por categoría F1 y F2
  - Se eliminan opciones vacías
  - Se construyen los chips de marcas populares con Regex
        │
        ▼
[ Ejecución del Motor de Filtrado `executeFiltering()` ]
  - Cruce de F1 + F2 + Producto + Marca + Tipo + Keyword
  - Ordenamiento (Crecimiento, Interés o Alfabético)
        │
        ▼
[ Renderizado en Pantalla ]
  - Actualización de Scorecards KPI (Volumen, Breakouts, Cobertura)
  - Renderizado de la tabla paginada (50 ítems por página)
```

---

## 3. Gestión de Estado en Memoria (`State Management`)

En `index.html`, el estado global está centralizado en variables reactivas:

```javascript
let masterData = [];         // Colección maestra inmutable de los ~7,530 registros
let currentFiltered = [];    // Subconjunto filtrado activo en la vista
let selectedF1 = "";         // Filtro de Nivel 1 activo (ej. 'F19')
let selectedF2 = "";         // Filtro de Nivel 2 activo (ej. 'F1901')
let selectedProduct = "";    // Filtro por producto específico
let selectedBrand = "";      // Marca seleccionada (id de la marca)
let selectedSort = "growth"; // Criterio: 'growth', 'interest', 'alpha'
let selectedType = "all";    // Criterio: 'all', 'breakout', 'top'
let selectedKeyword = "";    // Término de búsqueda libre
let currentPage = 1;         // Paginador
let pageSize = 50;           // Registros por página
```

---

## 4. Regla de Negocio y UI: Nombres Visibles en Menús F1 y F2

> [!IMPORTANT]
> **REGLA OBLIGATORIA DEL PROYECTO**: En todos los selectores de categorías (**F1**) y subcategorías (**F2**), así como en la tabla de resultados y en los archivos CSV exportados, el **NOMBRE COMPLETO** debe ser visible siempre de forma prioritaria, acompañado de su código identificador y conteo de términos.
> 
> * **En F1:** `[Nombre Completo] ([Código F1]) — [N] términos` (ej. `Tecnología y Telefonía (F19) — 2,075 términos`).
> * **En F2:** `[Nombre de Subcategoría] ([Código F2 si aplica]) — [N] términos` (ej. `Celulares y Smartphones (F1901) — 557 términos`, `Tenis y Zapatillas Deportivas — 1,582 términos`).
> * **Prohibición estricta:** Queda estrictamente prohibido mostrar opciones con códigos huérfanos (como `F04 - F04`) o textos genéricos deshabilitados (como `Línea única` o `Depende de F1`). Si no hay código formal F2, se mapea la línea de producto mediante `SUBCAT_LABELS`.

---

## 5. Detección Inteligente de Marcas con Expresiones Regulares

Para permitir a los gerentes comerciales filtrar con un solo clic por sus marcas clave sin requerir que la base de datos tenga un campo `marca`, el dashboard implementa un motor de coincidencia basado en regex (`POPULAR_BRANDS`):

```javascript
const POPULAR_BRANDS = [
  { id: 'samsung', label: 'Samsung', regex: /\bsamsung\b/i },
  { id: 'adidas', label: 'Adidas', regex: /\badidas\b/i },
  { id: 'apple', label: 'Apple / iPhone', regex: /\b(apple|iphone|ipad|macbook|airpods)\b/i },
  { id: 'nike', label: 'Nike', regex: /\bnike\b/i },
  { id: 'xiaomi', label: 'Xiaomi / Redmi', regex: /\b(xiaomi|redmi|poco)\b/i },
  { id: 'dji', label: 'DJI', regex: /\bdji\b/i },
  { id: 'lg', label: 'LG', regex: /\blg\b/i },
  { id: 'huawei', label: 'Huawei', regex: /\bhuawei\b/i },
  { id: 'new balance', label: 'New Balance', regex: /\bnew balance\b/i },
  { id: 'puma', label: 'Puma', regex: /\bpuma\b/i },
  { id: 'gw', label: 'GW Ciclismo', regex: /\bgw\b/i },
  { id: 'skechers', label: 'Skechers', regex: /\bskechers\b/i },
  { id: 'motorola', label: 'Motorola', regex: /\bmotorola\b/i },
  { id: 'honor', label: 'Honor', regex: /\bhonor\b/i },
  { id: 'oster', label: 'Oster', regex: /\boster\b/i },
  { id: 'sony', label: 'Sony', regex: /\bsony\b/i },
  { id: 'kalley', label: 'Kalley', regex: /\bkalley\b/i }
];
```

---

## 5. Sistema de Diseño Falabella (Looker Studio UI)

La interfaz replica la experiencia de una consola de Inteligencia de Negocios con los lineamientos de marca de **Falabella**:

### Paleta de Colores
* **Verde Oficial Falabella:** `#AAD63E` (Pantone 375 C)
* **Verde de Contraste para Texto:** `#166534`
* **Fondos de Alerta Suave:** `#f4fae6`
* **Bordes Activos:** `#cde875`
* **Superficies:** `#ffffff` (tarjetas) y `#f8fafc` (lienzo)
* **Tipografía de Texto:** `#0f172a` (principal), `#334155` (secundario), `#64748b` (muted)

### Tipografía Oficial
1. **Titulares y Marca:** `Libre Bodoni` (Serif itálica emblemática del isotipo Falabella con punto final).
2. **Interfaz y Datos Numéricos:** `Plus Jakarta Sans` (Sans-serif geométrica corporativa moderna de alta legibilidad).
