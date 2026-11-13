# 🤖 Asistente Comercial de IA — Especificación Técnica

Este documento describe la integración de Inteligencia Artificial que potencia el **Asistente IA Falabella**, ubicado en el botón flotante inferior derecho del dashboard.

---

## 1. Propósito y Capacidades

El asistente permite a gerentes comerciales, category managers y analistas de demanda consultar mediante lenguaje natural las tendencias de búsqueda en Colombia. 

Ejemplos de preguntas admitidas:
* *"¿Cuáles son los 5 productos con mayor porcentaje de crecimiento en calzado?"*
* *"¿Qué marcas de celulares tienen mayor volumen de búsqueda previo a Black Friday?"*
* *"¿Qué auriculares inalámbricos o marcas de sonido están en breakout?"*

---

## 2. Proveedor y Modelo Utilizado

* **Proveedor:** Google AI (Google DeepMind / Generative AI Studio)
* **Endpoint:** `https://generativelanguage.googleapis.com/v1beta/models/gemini-flash-lite-latest:generateContent`
* **Modelo:** `gemini-flash-lite-latest` (optimizado para respuestas conversacionales ultra-rápidas con latencia mínima).

---

## 3. Inyección de Contexto en Tiempo Real (RAG Dinámico en Cliente)

En lugar de requerir un servidor intermedio o una base de datos vectorial compleja, el asistente implementa un enfoque de **inyección contextual dinámica en el cliente**.

### Flujo de la Petición:
1. El usuario escribe una consulta en el chat o hace clic en un pill sugerido.
2. El cliente JavaScript evalúa la pregunta y extrae del `masterData` un resumen estadístico relevante:
   * Volumen de términos indexados en la categoría consultada.
   * Lista de los 20 términos con mayor crecimiento (`is_breakout = true` o mayor `%`).
   * Marcas más repetidas en el segmento.
3. Se arma el prompt del sistema (*System Prompt*) con directrices comerciales de Falabella.
4. Se despacha a Gemini y se renderiza el resultado formateado en Markdown dentro de la burbuja del bot.

---

## 4. Estructura del Prompt del Asistente

El prompt instruye al modelo a responder con el siguiente tono:
* **Rol:** Consultor Senior de Inteligencia Comercial y Retail para Falabella Colombia.
* **Tono:** Ejecutivo, cuantitativo, directo y accionable.
* **Reglas:**
  * Citar métricas exactas (ej. *"iPhone 16 Pro Max (+1,200% o Breakout)"*).
  * No inventar datos fuera de las 7,530 búsquedas indexadas.
  * Resaltar siempre oportunidades de inventario o promociones para Black Friday y fin de año.

---

## 5. Mantenimiento y Rotación de API Keys

En `index.html`:
```javascript
const GEMINI_KEY = atob("..."); // Clave codificada para cliente web
```

> [!TIP]
> Si se desea cambiar el modelo a `gemini-1.5-pro` o actualizar la clave de API, basta con editar la constante `GEMINI_KEY` o el endpoint en la función `sendChatMessage()` en [index.html](file:///Users/cesarandresjimenezarci/Documents/faltendencias/index.html). Para ambientes empresariales cerrados, se recomienda mover este llamado a una Vercel Serverless Function (`/api/chat.js`) para resguardar la API key en variables de entorno.
