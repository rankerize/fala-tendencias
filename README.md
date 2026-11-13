# 📊 Falabella Trends Analytics — Inteligencia de Demanda Colombia

Plataforma analítica estilo **Google Looker Studio / Data Studio** desarrollada para monitorear, filtrar y explorar las tendencias de búsqueda de Google Trends Colombia de cara a eventos comerciales de alto impacto (**Black Friday**, Cyberlunes, temporada navideña).

[![Vercel Deployment](https://img.shields.io/badge/Deploy-Vercel-black?logo=vercel)](https://falla-tendencias.vercel.app)
[![Supabase Database](https://img.shields.io/badge/Database-Supabase-3ECF8E?logo=supabase)](https://supabase.com)
[![Gemini AI](https://img.shields.io/badge/AI-Google%20Gemini-4285F4?logo=google)](https://ai.google.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## ⚡ Enlaces Rápidos y Producción

* **Producción:** [https://falla-tendencias.vercel.app](https://falla-tendencias.vercel.app)
* **Repositorio GitHub:** [https://github.com/rankerize/fala-tendencias.git](https://github.com/rankerize/fala-tendencias.git)
* **Rama Principal:** `main`

---

## 📚 Índice de Documentación del Proyecto

Hemos estructurado la documentación técnica en guías modulares para facilitar la colaboración de cualquier ingeniero o analista:

| Documento | Descripción |
| :--- | :--- |
| [**CONTRIBUTING.md**](file:///Users/cesarandresjimenezarci/Documents/faltendencias/CONTRIBUTING.md) | **Guía de desarrollo local:** cómo correr el proyecto en 1 comando, cómo agregar marcas a los filtros, cómo agregar categorías y flujo de Git. |
| [**docs/DATABASE.md**](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/DATABASE.md) | **Base de Datos Supabase:** esquema SQL completo de la tabla `tendencias_queries`, tipos de datos, índices y script para ingestar nuevos datos. |
| [**docs/ARCHITECTURE.md**](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/ARCHITECTURE.md) | **Arquitectura Técnica:** ciclo de vida de la aplicación, gestión de estado en memoria (`State Management`) y Sistema de Diseño oficial Falabella. |
| [**docs/DEPLOYMENT.md**](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/DEPLOYMENT.md) | **Despliegue y Vercel:** integración continua con GitHub, configuración de `vercel.json`, comandos de CLI y dominios personalizados. |
| [**docs/AI_ASSISTANT.md**](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/AI_ASSISTANT.md) | **Asistente de IA (Gemini):** arquitectura de RAG en cliente, prompts del sistema y conexión con la API de Google Generative Language. |

---

## 🏗️ 1. Arquitectura General del Proyecto

El proyecto opera como una **Single Page Application (SPA) serverless y estática (Zero-Build)** conectada en tiempo real a una base de datos PostgreSQL en la nube y a un motor de Inteligencia Artificial para asistencia comercial.

```
┌────────────────────────────────────────────────────────┐
│             Frontend SPA (Vanilla JS + CSS3)           │
│  - Dashboard Looker Studio (Scorecards, Tablas, Chips) │
│  - Asistente Comercial IA (Gemini Flash-Lite)          │
│  - Archivos: index.html, falabella.svg, vercel.json    │
└───────────────────────────┬────────────────────────────┘
                            │
              ┌─────────────┴─────────────┐
              ▼                           ▼
┌───────────────────────────┐   ┌───────────────────────────┐
│     Supabase Database     │   │      Google Gemini AI     │
│   (PostgreSQL Cloud)      │   │     (Generative API)      │
│  - Tabla:                 │   │  - Modelo:                │
│    tendencias_queries     │   │    gemini-flash-lite      │
│  - ~7,530 registros       │   │  - Q&A sobre demanda y    │
│  - Paginación dinámica    │   │    breakouts de mercado   │
└───────────────────────────┘   └───────────────────────────┘
```

---

## 🗄️ 2. Base de Datos y Tablas (Dónde viven los datos)

* **Proveedor:** [Supabase](https://supabase.com/)
* **URL del Proyecto:** `https://ybfbccrdscgpeujymggu.supabase.co`
* **Tabla Principal:** `tendencias_queries` (~7,530 registros indexados de Google Trends Colombia).

### Campos clave de la tabla:
* `query`: Término de búsqueda en Colombia.
* `search_interest`: Nivel de interés relativo (0 a 100).
* `increase_percent`: % de incremento reportado (ej. `+350%`).
* `is_breakout`: Booleano (`true`/`false`) para términos con crecimiento explosivo (+5,000%).
* `tipo_busqueda`: Clasificación entre búsquedas `'top'` (volumen) o `'rising'` (en ascenso).
* `f1_codigo` / `f2_codigo`: Códigos de categoría y sublínea Falabella (`F00`, `F19`, `F18`, `F13`, `F43`, `F14`, etc.).
* `carpeta_origen` / `archivo_origen`: Producto de origen y trazabilidad de ingesta.

*(Para ver el DDL SQL completo, consulta [docs/DATABASE.md](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/DATABASE.md)).*

---

## 🚀 3. Despliegue y CI/CD (Deploy)

* **Proveedor:** [Vercel](https://vercel.com/)
* **Proyecto en Vercel:** `falla-tendencias`
* **Pipeline:** Integrado automáticamente con la rama `main` de GitHub. Cualquier `git push` a `main` desencadena un despliegue en producción en menos de 30 segundos.

*(Para ver instrucciones de CLI y configuración de dominios, consulta [docs/DEPLOYMENT.md](file:///Users/cesarandresjimenezarci/Documents/faltendencias/docs/DEPLOYMENT.md)).*

---

## 💻 4. Inicio Rápido Local (1 Minuto)

```bash
# 1. Clonar repositorio
git clone https://github.com/rankerize/fala-tendencias.git
cd fala-tendencias

# 2. Servir localmente (cualquier servidor estático)
python3 -m http.server 8000
# o también: npx serve .
```
Abre tu navegador en `http://localhost:8000`. ¡Listo para trabajar!
