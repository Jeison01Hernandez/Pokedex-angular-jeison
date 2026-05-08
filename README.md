# Pokedex Angular

## 📖 Descripción General del Proyecto

**Pokedex Angular** es una aplicación web moderna desarrollada con **Angular** que permite a los usuarios explorar y conocer información detallada sobre todos los Pokémon disponibles en el universo Pokémon. La aplicación se conecta con la **PokéAPI** para obtener datos en tiempo real y proporciona una experiencia de usuario fluida e intuitiva.

El proyecto demuestra el uso de tecnologías modernas de desarrollo front-end, buenas prácticas de seguridad web, y su despliegue en una solución cloud profesional.

### 🎯 Objetivo Principal

Crear una herramienta interactiva que permita:
- **Visualizar** un catálogo completo de Pokémon
- **Buscar y filtrar** Pokémon por nombre, tipo o atributos
- **Consultar estadísticas** detalladas de cada criatura
- **Explorar habilidades** y movimientos disponibles
- **Acceder públicamente** desde cualquier dispositivo con navegador web

---

## 🛠️ Stack Tecnológico

| Componente | Tecnología | Versión |
|-----------|-----------|---------|
| **Framework** | Angular | 14+ |
| **Lenguaje** | TypeScript | 4.7+ |
| **Estilos** | CSS/SCSS | Moderno |
| **API** | PokéAPI | REST |
| **Hosting** | Azure Static Web Apps | - |
| **Control de Versiones** | GitHub | - |
| **Protocolo** | HTTPS | Obligatorio |

---

## 🏗️ Arquitectura de la Aplicación

### Estructura del Proyecto

```
src/
├── app/
│   ├── components/        # Componentes reutilizables
│   ├── services/          # Servicios (API, caché, etc.)
│   ├── models/            # Interfaces y tipos
│   └── pages/             # Componentes de página
├── assets/                # Imágenes, iconos, datos estáticos
├── environments/          # Configuración por entorno
└── styles/                # Estilos globales
dist/                       # Build compilado (output)
staticwebapp.config.json    # Configuración Azure & seguridad
```

### Componentes Principales

1. **Página de Inicio** - Galería principal de Pokémon
2. **Detalle de Pokémon** - Vista individual con estadísticas completas
3. **Búsqueda y Filtros** - Herramientas de navegación
4. **Barra de Navegación** - Menú y controles globales

### Servicios Clave

- **PokemonService** - Consumo de PokéAPI
- **CacheService** - Caché local de datos
- **SearchService** - Lógica de búsqueda y filtrado

---

## 🔐 Seguridad

### Encabezados de Seguridad HTTP Implementados

La aplicación implementa controles de seguridad web robustos mediante headers HTTP configurados en `staticwebapp.config.json`:

| Encabezado | Valor | Protección |
|-----------|--------|-----------|
| **Content-Security-Policy** | `default-src 'self' https://pokeapi.co; img-src * data:; font-src * data:` | Previene XSS y carga de recursos no autorizados |
| **Strict-Transport-Security** | `max-age=31536000; includeSubDomains; preload` | Fuerza HTTPS, previene downgrade |
| **X-Content-Type-Options** | `nosniff` | Evita MIME sniffing |
| **X-Frame-Options** | `DENY` | Previene clickjacking |
| **Referrer-Policy** | `no-referrer` | Protege privacidad del usuario |
| **Permissions-Policy** | `camera=(), microphone=(), geolocation=()` | Deniega permisos innecesarios |

### Medidas de Seguridad Adicionales

✅ **HTTPS Obligatorio** - Azure Static Web Apps administra certificados SSL/TLS  
✅ **Validación en Cliente** - TypeScript con tipado fuerte  
✅ **Sanitización de Entrada** - Angular protege contra XSS por defecto  
✅ **CORS Configurado** - Solo permite origen de PokéAPI  
✅ **Sin Información Sensible** - Aplicación completamente estática sin backend  
✅ **Validación de Security Headers** - Calificación **A** en securityheaders.com

### Resultado de Auditoría de Seguridad

```
Sitio: https://polite-dune-0fd20d71e.7.azurestaticapps.net/
Calificación: A
Encabezados Detectados: ✓ CSP ✓ HSTS ✓ X-Content-Type-Options ✓ X-Frame-Options ✓ Referrer-Policy ✓ Permissions-Policy
```

---

## 📦 Requisitos Previos

Para ejecutar la aplicación localmente, necesitas:

- **Node.js** (v14 o superior)
- **npm** (v6 o superior) o **yarn**
- **Angular CLI** (v14 o superior)
- Navegador web moderno (Chrome, Firefox, Safari, Edge)

---

## 🚀 Instalación Local

### 1. Clonar el Repositorio

```bash
git clone https://github.com/Jeison01hernandez/Pokedex-angular-jeison.git
cd pokedex-angular-jeison
```

### 2. Instalar Dependencias

```bash
npm install
```

### 3. Ejecutar en Desarrollo

```bash
ng serve
# o
npm start
```

La aplicación estará disponible en `http://localhost:4200/`

### 4. Compilar para Producción

```bash
ng build --configuration production
```

El build se generará en la carpeta `dist/pokedex-angular/`

---

## 🌐 Acceso a la Aplicación Desplegada

**URL Pública:** https://polite-dune-0fd20d71e.7.azurestaticapps.net/

La aplicación está desplegada en **Azure Static Web Apps** y se actualiza automáticamente con cada push a la rama `main` del repositorio GitHub.

### Características del Despliegue

✅ Despliegue automático desde GitHub  
✅ HTTPS administrado por Azure  
✅ CDN global para baja latencia  
✅ Compilación automática de Angular  
✅ Headers de seguridad aplicados en todas las respuestas  
✅ Configuración de SPA con fallback a index.html  

---

## 🔧 Configuración de Desarrollo

### Variables de Entorno

El proyecto usa archivos de configuración de Angular:

- **environment.ts** - Desarrollo local
- **environment.prod.ts** - Producción (Azure)

### Build

El proyecto está configurado para:
- Output location: `dist/pokedex-angular`
- Build command: `ng build --configuration production`
- Ubicación del archivo de configuración: `staticwebapp.config.json`

---

## 📱 Funcionalidades Principales

### ✨ Características Implementadas

- 🔍 **Búsqueda** - Encuentra Pokémon por nombre
- 🏷️ **Filtrado** - Filtra por tipo, generación, o estadísticas
- 📊 **Estadísticas Detalladas** - HP, Ataque, Defensa, Velocidad, etc.
- 🎨 **Diseño Responsivo** - Funciona en desktop, tablet y móvil
- ⚡ **Caché Local** - Mejora el rendimiento de búsquedas
- 🌍 **Datos en Tiempo Real** - Conecta con PokéAPI
- 📦 **Información Completa** - Habilidades, movimientos, evoluciones

---

## 🛠️ Mantenimiento y Actualizaciones

### Workflow de Cambios

1. Crear una rama desde `main`
2. Realizar cambios locales
3. Commit y push a la rama
4. Crear Pull Request en GitHub
5. Merge a `main` (dispara despliegue automático)

### Monitoreo

- Revisar logs de despliegue en Azure Portal
- Validar funcionalidad en URL pública
- Verificar headers de seguridad con securityheaders.com

---

## 📚 Documentación Complementaria

Este proyecto incluye documentación adicional:

- **`despliegue.md`** - Procedimiento técnico detallado del despliegue en Azure Static Web Apps
- **`documentacion-proyecto.md`** - Consolidación ejecutiva y técnica de la solución implementada

---

## 📄 Licencia

Este proyecto es una implementación educativa basada en el repositorio proporcionado por el profesor. Se comparte bajo fines académicos.

---

## 📧 Autor

**Jeison Hernández**  
Estudiante - Desarrollo de Aplicaciones Web  
Repositorio: https://github.com/Jeison01hernandez/Pokedex-angular-jeison

---

## 🙏 Créditos

- **PokéAPI** - Datos de Pokémon en tiempo real
- **Angular Team** - Framework de desarrollo
- **Microsoft Azure** - Hosting y servicios en cloud
- **Profesor** - Proporciona el repositorio base y requisitos del proyecto

---

**Última actualización:** Mayo 2026  
**Estado:** Producción ✅  
**Calificación de Seguridad:** A ⭐
