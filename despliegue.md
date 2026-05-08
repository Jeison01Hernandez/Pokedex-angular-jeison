# 🚀 Guía de Despliegue - Pokedex Angular

**Proyecto:** Pokedex Angular  
**Autor:** Jeison Hernández  
**Plataforma de Hosting:** Microsoft Azure Static Web Apps  
**URL Producción:** https://polite-dune-0fd20d71e.7.azurestaticapps.net/

---

## 📋 Tabla de Contenidos

1. [Requisitos Previos](#requisitos-previos)
2. [Preparación del Proyecto](#preparación-del-proyecto)
3. [Configuración de Seguridad](#configuración-de-seguridad)
4. [Despliegue en Azure Static Web Apps](#despliegue-en-azure-static-web-apps)
5. [Validación Post-Despliegue](#validación-post-despliegue)
6. [Mantenimiento y Actualizaciones](#mantenimiento-y-actualizaciones)
7. [Solución de Problemas](#solución-de-problemas)

---

## ✅ Requisitos Previos

### Herramientas Necesarias

- ✅ **Git** - Control de versiones
- ✅ **Node.js** v14+ - Runtime de JavaScript
- ✅ **npm** o **yarn** - Gestor de paquetes
- ✅ **Angular CLI** - Herramienta de desarrollo
- ✅ **Cuenta GitHub** - Repositorio de código
- ✅ **Cuenta Microsoft Azure** - Hosting en cloud
- ✅ **CLI de Azure** (opcional) - Administración desde terminal

### Instalación de Herramientas

```bash
# Instalar Node.js desde https://nodejs.org (LTS recomendado)
# Verificar instalación:
node --version
npm --version

# Instalar Angular CLI globalmente:
npm install -g @angular/cli

# Instalar CLI de Azure (opcional):
npm install -g @azure/cli
```

---

## 🔧 Preparación del Proyecto

### Paso 1: Preparar el Repositorio Local

#### 1.1 Clonar o Actualizar el Repositorio

```bash
# Si es la primera vez:
git clone https://github.com/Jeison01hernandez/Pokedex-angular-jeison.git
cd pokedex-angular-jeison

# Si ya tienes el repositorio:
cd /ruta/del/proyecto
git pull origin main
```

#### 1.2 Instalar Dependencias

```bash
npm install
```

**Salida esperada:**
```
added XXX packages in Xs
```

### Paso 2: Verificar la Configuración de Angular

Asegúrate de que `angular.json` tenga la configuración correcta:

```json
{
  "projects": {
    "pokedex-angular": {
      "architect": {
        "build": {
          "configurations": {
            "production": {
              "outputPath": "dist/pokedex-angular",
              "optimization": true,
              "sourceMap": false,
              "namedChunks": false,
              "extractLicenses": true
            }
          }
        }
      }
    }
  }
}
```

**Punto clave:** `outputPath` debe ser `dist/pokedex-angular`

### Paso 3: Compilar para Producción

```bash
ng build --configuration production
```

**Salida esperada:**
```
✔ Compilation successful
✔ 2 bundles created
✔ Build at: dist/pokedex-angular/
Build time: XXXms
```

**Archivos generados:**
- `dist/pokedex-angular/` - Carpeta compilada lista para desplegar
- `dist/pokedex-angular/index.html` - Punto de entrada SPA
- `dist/pokedex-angular/main.*.js` - Bundel principal de la aplicación
- `dist/pokedex-angular/styles.*.css` - Estilos compilados

---

## 🔐 Configuración de Seguridad

### Paso 4: Crear el Archivo de Configuración de Azure

Crea el archivo `staticwebapp.config.json` en la raíz del proyecto:

```json
{
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/images/*", "/css/*", "/js/*", "/assets/*"]
  },
  "responseOverrides": {
    "400": {
      "rewrite": "/index.html"
    },
    "404": {
      "rewrite": "/index.html"
    }
  },
  "globalHeaders": {
    "content-security-policy": "default-src 'self' https://pokeapi.co; img-src * data:; font-src * data:; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'",
    "strict-transport-security": "max-age=31536000; includeSubDomains; preload",
    "x-content-type-options": "nosniff",
    "x-frame-options": "DENY",
    "referrer-policy": "no-referrer",
    "permissions-policy": "camera=(), microphone=(), geolocation=()"
  }
}
```

### Explicación de Headers de Seguridad

| Header | Función | Valor Configurado |
|--------|---------|-------------------|
| **CSP** | Permite solo recursos de fuentes de confianza | Origen propio + pokeapi.co |
| **HSTS** | Força conexión HTTPS permanente | 1 año + subdominios |
| **X-Content-Type-Options** | Previene MIME sniffing | `nosniff` |
| **X-Frame-Options** | Previene clickjacking | `DENY` (no embeber en frames) |
| **Referrer-Policy** | Protege privacidad del usuario | `no-referrer` |
| **Permissions-Policy** | Deniega permisos innecesarios | Camera, Mic, Geolocation |

---

## 🌐 Despliegue en Azure Static Web Apps

### Paso 5: Crear una Aplicación en Azure Static Web Apps

#### 5.1 Acceder a Azure Portal

1. Ir a https://portal.azure.com
2. Iniciar sesión con tu cuenta Microsoft
3. Buscar **"Static Web Apps"** en la barra de búsqueda
4. Hacer clic en **"+ Create"** o **"Crear"**

#### 5.2 Configurar la Aplicación

**Formulario de Creación:**

| Campo | Valor |
|-------|-------|
| **Subscription** | Selecciona tu suscripción |
| **Resource Group** | Crea uno nuevo: `rg-pokedex` |
| **Name** | `pokedex-angular` |
| **Plan Type** | Free (para pruebas) o Standard |
| **Region** | Selecciona la más cercana (ej: West US 2) |
| **Deployment details** | GitHub |
| **GitHub Account** | Autoriza tu cuenta GitHub |
| **Organization** | Tu usuario GitHub |
| **Repository** | `Pokedex-angular-jeison` |
| **Branch** | `main` |
| **Build Presets** | `Angular` |
| **App location** | `.` (raíz) |
| **API location** | Dejar vacío (no hay backend) |
| **Output location** | `dist/pokedex-angular` |

#### 5.3 Revisar y Crear

1. Revisar configuración en **"Review + Create"**
2. Hacer clic en **"Create"**
3. Azure creará la aplicación y configurará GitHub Actions automáticamente

**Tiempo estimado:** 2-5 minutos

### Paso 6: Verificar la Configuración de GitHub Actions

Una vez creada la aplicación Static Web Apps, Azure automáticamente:

1. ✅ Añade un secreto `DEPLOYMENT_TOKEN` en GitHub
2. ✅ Crea un workflow de GitHub Actions en `.github/workflows/`
3. ✅ Dispara un build automático en cada push a `main`

**Para verificar:**

```bash
# En tu repositorio GitHub
# Ir a Settings > Secrets and variables > Actions
# Debe aparecer: AZURE_STATIC_WEB_APPS_DEPLOYMENT_TOKEN_XXXXX
```

### Paso 7: Desplegar

#### Opción A: Despliegue Automático (Recomendado)

```bash
# Solo hacer push a main - GitHub Actions dispara automáticamente
git add .
git commit -m "Despliegue inicial en Azure Static Web Apps"
git push origin main
```

**El workflow automático:**
1. Detecta el push en `main`
2. Instala dependencias (`npm install`)
3. Compila el proyecto (`ng build --configuration production`)
4. Carga los archivos a Azure
5. Asigna una URL pública
6. Aplica headers de seguridad

#### Opción B: Despliegue Manual con Azure CLI

```bash
# Instalar Azure CLI
npm install -g @azure/cli

# Autenticarse en Azure
az login

# Desplegar la carpeta dist/
az staticwebapp upload \
  --name pokedex-angular \
  --source ./dist/pokedex-angular \
  --resource-group rg-pokedex
```

---

## ✨ Validación Post-Despliegue

### Paso 8: Verificar Despliegue Exitoso

#### 8.1 Acceder a la Aplicación

Después de 2-3 minutos, la aplicación estará disponible en:

```
https://polite-dune-0fd20d71e.7.azurestaticapps.net/
```

**Cosas a verificar:**
- ✅ La página carga sin errores 404
- ✅ Los estilos CSS se aplican correctamente
- ✅ Las imágenes de Pokémon se cargan
- ✅ La búsqueda y filtros funcionan
- ✅ No hay errores en la consola del navegador

#### 8.2 Verificar Headers de Seguridad

```bash
# Usando curl para verificar headers
curl -I https://polite-dune-0fd20d71e.7.azurestaticapps.net/

# Salida esperada debe incluir:
# Content-Security-Policy: ...
# Strict-Transport-Security: ...
# X-Content-Type-Options: nosniff
# X-Frame-Options: DENY
# Referrer-Policy: no-referrer
# Permissions-Policy: camera=(), microphone=(), geolocation=()
```

#### 8.3 Auditar Seguridad en securityheaders.com

1. Ir a https://securityheaders.com/
2. Ingresar URL: `https://polite-dune-0fd20d71e.7.azurestaticapps.net/`
3. Analizar resultados
4. **Objetivo:** Calificación A o superior

**Resultado esperado:**
```
Grade: A
✓ Content-Security-Policy
✓ Strict-Transport-Security
✓ X-Content-Type-Options
✓ X-Frame-Options
✓ Referrer-Policy
✓ Permissions-Policy
```

#### 8.4 Verificar Rendimiento

```bash
# Usando Lighthouse en Chrome DevTools
# F12 > Lighthouse > Analyze page load
# Observar métricas de Performance, Accessibility, Best Practices, SEO
```

---

## 🔄 Mantenimiento y Actualizaciones

### Flujo de Actualización

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Crear rama de features                                   │
│    git checkout -b feature/nueva-funcionalidad               │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. Hacer cambios locales                                    │
│    - Editar componentes                                     │
│    - Actualizar servicios                                   │
│    - Probar en localhost (ng serve)                         │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. Commit y push                                            │
│    git add .                                                │
│    git commit -m "Descripción del cambio"                   │
│    git push origin feature/nueva-funcionalidad              │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. Crear Pull Request en GitHub                            │
│    - Describir cambios                                      │
│    - Solicitar revisión                                     │
│    - Esperar aprobación                                     │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. Merge a main (Squash merge recomendado)                 │
│    - GitHub Actions dispara automáticamente                 │
│    - Build se inicia                                        │
│    - Despliegue se ejecuta                                  │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. Verificar despliegue en producción                       │
│    - Acceder a URL pública                                  │
│    - Probar funcionalidades                                 │
│    - Monitorear logs de Azure                               │
└─────────────────────────────────────────────────────────────┘
```

### Monitoreo del Despliegue

**En Azure Portal:**
1. Ir a Static Web Apps > pokedex-angular
2. Sección **"Deployments"** - Ver historial de despliegues
3. Sección **"Monitoring"** - Ver estadísticas de uso
4. Sección **"Configuration"** - Ajustar settings globales

**En GitHub Actions:**
1. Ir a Actions en el repositorio
2. Ver workflow en tiempo real
3. Logs detallados de build y despliegue

---

## 🔧 Solución de Problemas

### ❌ Error: "Build failed"

**Síntomas:**
- El workflow de GitHub Actions falla
- Mensaje de error en los logs

**Solución:**
```bash
# 1. Compilar localmente para verificar
ng build --configuration production

# 2. Si hay errores de TypeScript:
ng build --configuration production --aot=false

# 3. Limpiar cache y reinstalar
rm -rf node_modules dist
npm install
ng build --configuration production

# 4. Push nuevamente
git add .
git commit -m "Corrección de errores de build"
git push origin main
```

### ❌ Error: "404 - Not Found"

**Síntomas:**
- Actualizar la página de un componente da 404
- Solo funciona la ruta raíz /

**Causa:** La configuración SPA fallback no está correcta

**Solución:**
Verificar `staticwebapp.config.json`:
```json
{
  "navigationFallback": {
    "rewrite": "/index.html"
  }
}
```

### ❌ Error: "CORS error al conectar PokéAPI"

**Síntomas:**
```
Access to XMLHttpRequest... blocked by CORS policy
```

**Causa:** El header CSP es muy restrictivo

**Solución:**
Actualizar CSP en `staticwebapp.config.json`:
```json
"content-security-policy": "default-src 'self' https:; img-src * data:; font-src * data:; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'"
```

### ❌ Estilos no se aplican

**Síntomas:**
- Página sin estilos CSS
- Elementos sin formato

**Causa:** Rutas incorrectas en HTML

**Solución:**
Verificar que todos los `<link>` y `<script>` usen rutas relativas:
```html
<!-- Correcto -->
<link rel="stylesheet" href="styles.xxxxx.css">

<!-- Incorrecto -->
<link rel="stylesheet" href="/styles.xxxxx.css">
```

### ❌ Las imágenes no cargan

**Síntomas:**
- Imágenes rotas (icono de imagen rota)

**Causa:**
1. Rutas incorrectas
2. CORS bloqueado
3. Header CSP

**Solución:**
```bash
# Verificar que las imágenes existan
ls dist/pokedex-angular/assets/

# Verificar rutas en el código
grep -r "img-src" .
```

---

## 📊 Dashboard de Monitoreo

### Acceder al Dashboard de Azure

1. **Azure Portal** → Static Web Apps → pokedex-angular
2. **Secciones útiles:**
   - **Overview** - Estado general y URL pública
   - **Deployments** - Historial de despliegues
   - **Logs** - Logs de ejecución
   - **Application Insights** (si está habilitado) - Analítica y errores

### Ver Logs de GitHub Actions

1. **GitHub** → Repository → Actions
2. Seleccionar el workflow más reciente
3. Expandir pasos para ver detalles

---

## 🎉 Resumen del Despliegue

**Configuración Final:**

| Componente | Valor |
|-----------|-------|
| **Framework** | Angular 14+ |
| **Hosting** | Azure Static Web Apps |
| **Región Azure** | West US 2 (o tu selección) |
| **Build Trigger** | Push a `main` en GitHub |
| **Build Command** | `ng build --configuration production` |
| **Output Folder** | `dist/pokedex-angular` |
| **URL Pública** | https://polite-dune-0fd20d71e.7.azurestaticapps.net/ |
| **HTTPS** | ✅ Automático |
| **Headers de Seguridad** | ✅ Configurados (Grado A) |
| **CDN Global** | ✅ Activado |

**Automatización Lograda:**
- ✅ CI/CD completo con GitHub Actions
- ✅ Despliegue automático en cada push
- ✅ Compilación automática de Angular
- ✅ Headers de seguridad en todas las respuestas
- ✅ Certificados SSL/TLS administrados por Azure
- ✅ Fallback SPA configurado

---

**Última actualización:** Mayo 2026  
**Versión del Despliegue:** 1.0  
**Estado:** ✅ Operativo

