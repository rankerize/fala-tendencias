# 🛠️ Guía para Desarrolladores y Contribución — CONTRIBUTING.md

¡Bienvenido al proyecto **Falabella Trends Analytics**! Esta guía detalla cómo clonar el repositorio, ejecutar la aplicación localmente, extender componentes y colaborar bajo las mejores prácticas.

---

## 1. Inicio Rápido en Entorno Local (Quickstart)

El proyecto es una **SPA estática (Zero-Build)**. No requiere `npm install`, empaquetadores ni dependencias pesadas.

### Paso 1: Clonar el repositorio
```bash
git clone https://github.com/rankerize/fala-tendencias.git
cd fala-tendencias
```

### Paso 2: Servir localmente
Cualquier servidor HTTP estático sirve la aplicación perfectamente:

* **Con Python (instalado por defecto en macOS/Linux):**
  ```bash
  python3 -m http.server 8000
  ```
  Abrir en el navegador: [http://localhost:8000](http://localhost:8000)

* **Con Node.js (npx serve):**
  ```bash
  npx serve .
  ```

* **Con VS Code:**
  Instalar la extensión *Live Server* y presionar "Go Live".

---

## 2. Cómo Extender la Aplicación

### A. ¿Cómo agregar una nueva marca al selector visual?
En [index.html](file:///Users/cesarandresjimenezarci/Documents/faltendencias/index.html), ubicar el arreglo `POPULAR_BRANDS` (alrededor de la línea 815):

```javascript
const POPULAR_BRANDS = [
  // ... marcas existentes ...
  { id: 'bose', label: 'Bose', regex: /\bbose\b/i },
  { id: 'whirlpool', label: 'Whirlpool', regex: /\bwhirlpool\b/i }
];
```
El motor automáticamente creará el chip visual, calculará el conteo dinámico de búsquedas y lo filtrará en la tabla al hacer clic.

---

### B. ¿Cómo registrar nuevas categorías comerciales (F1 o F2)?
En [index.html](file:///Users/cesarandresjimenezarci/Documents/faltendencias/index.html), editar los diccionarios `F1_LABELS` y `F2_LABELS`:

```javascript
const F1_LABELS = {
  'F00': 'Eventos (Black Friday)',
  'F19': 'Tecnología y Telefonía',
  // Agregar nueva:
  'F50': 'Juguetería y Niños'
};

const F2_LABELS = {
  'F5001': 'Juguetes Educativos',
  'F5002': 'Figuras de Acción'
};
```

---

## 3. Flujo de Trabajo en Git

1. **Crear una rama para la tarea:**
   ```bash
   git checkout -b feature/nombre-de-la-mejora
   ```
2. **Probar localmente en el navegador** asegurando que los KPIs, filtros de marcas y el asistente de chat funcionen correctamente.
3. **Confirmar los cambios con Conventional Commits:**
   * `feat: ...` para nuevas funcionalidades.
   * `fix: ...` para corrección de bugs o filtros.
   * `docs: ...` para cambios en documentación.
   * `style: ...` para ajustes visuales en CSS.
4. **Push y Pull Request:**
   ```bash
   git push origin feature/nombre-de-la-mejora
   ```
5. Una vez aprobado y fusionado en `main`, Vercel desplegará automáticamente los cambios a producción.
