# 🚀 Guía de Despliegue y Hosting — Vercel

Este documento describe la infraestructura de despliegue, configuración de Vercel, vinculación con GitHub y procedimientos de publicación para nuevos colaboradores.

---

## 1. Infraestructura de Producción

* **Plataforma:** [Vercel](https://vercel.com/)
* **Organización:** `rankerizes-projects`
* **ID de Organización:** `team_ceDuXboXaNKd4U5zICy15OwZ`
* **Nombre del Proyecto:** `falla-tendencias`
* **Project ID:** `prj_4w1QPA9esO5Spz28KQHYrp8BTQRa`
* **URL Pública en Producción:** [https://falla-tendencias.vercel.app](https://falla-tendencias.vercel.app)

---

## 2. Flujo de Despliegue Continuo (CI/CD Automático)

El despliegue está totalmente automatizado mediante la integración oficial entre **GitHub y Vercel**:

```
[ Desarrollador hace commit ] 
            │
            ▼
[ git push origin main ] 
            │
            ▼
[ Webhook de GitHub hacia Vercel ]
            │
            ▼
[ Vercel Build & Deploy (~15-30 segundos) ]
            │
            ▼
[ Nuevo release visible en falla-tendencias.vercel.app ]
```

Cada commit enviado a la rama `main` en `rankerize/fala-tendencias` genera un nuevo despliegue instantáneo sin tiempo de inactividad (*zero-downtime deployment*).

---

## 3. Configuración del Servidor (`vercel.json`)

El archivo [vercel.json](file:///Users/cesarandresjimenezarci/Documents/faltendencias/vercel.json) contiene la siguiente definición:

```json
{
  "cleanUrls": true,
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

* **`cleanUrls: true`:** Elimina extensiones `.html` de las URLs en el navegador.
* **`rewrites`:** Redirige cualquier sub-ruta al archivo central `index.html` para permitir el comportamiento de Single Page Application sin errores 404.

---

## 4. Despliegue Manual con Vercel CLI

Si un desarrollador cuenta con acceso al equipo en Vercel y necesita desplegar directamente desde la terminal:

```bash
# 1. Instalar Vercel CLI globalmente (si no está instalado)
npm install -g vercel

# 2. Iniciar sesión en Vercel
vercel login

# 3. Vincular el directorio local con el proyecto en Vercel
vercel link

# 4. Desplegar en modo Preview
vercel

# 5. Desplegar a Producción directa
vercel --prod
```

---

## 5. Configuración de Dominios Personalizados

Si en el futuro se desea asignar un subdominio corporativo (por ejemplo `tendencias.falabella.com.co` o `tendencias.rankerize.com`):
1. Ingresar a [Vercel Dashboard](https://vercel.com/rankerizes-projects/falla-tendencias/settings/domains).
2. Ir a **Settings** > **Domains**.
3. Agregar el dominio deseado.
4. Configurar el registro `CNAME` apuntando a `cname.vercel-dns.com` en el proveedor de DNS.
