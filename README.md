# Taller Gerardito — Landing Page

Sitio web de página única (landing page) para **Taller Gerardito**, un taller de reparación automotriz especializado en diagnósticos electrónicos, ubicado en San Manuel, Cortés, Honduras.

## Stack tecnológico

- [Astro 5](https://astro.build/) — framework principal (SSG, server-first)
- [Tailwind CSS 4](https://tailwindcss.com/) — estilos con clases utilitarias
- [GSAP 3 + ScrollTrigger](https://gsap.com/) — animaciones de entrada y scroll

## Requisitos previos

- Node.js 18 o superior
- npm 9 o superior

## Instalación

```bash
# Clonar el repositorio
git clone <repo-url>
cd taller-gerardo

# Instalar dependencias
npm install
```

## Comandos de desarrollo

```bash
# Servidor de desarrollo (http://localhost:4321)
npm run dev

# Compilar para producción
npm run build

# Previsualizar el build antes de desplegar
npm run preview
```

## Estructura del proyecto

```
taller-gerardo/
├── public/
│   ├── favicon.ico
│   └── favicon.svg
├── src/
│   ├── components/          # Secciones de la landing page
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── Services.astro
│   │   ├── About.astro
│   │   ├── Equipment.astro
│   │   ├── WhyUs.astro
│   │   ├── Brands.astro
│   │   ├── Testimonials.astro
│   │   ├── Contact.astro
│   │   ├── Map.astro
│   │   ├── Footer.astro
│   │   └── WhatsAppButton.astro
│   ├── layouts/
│   │   └── Layout.astro     # Layout base con SEO y metadatos
│   ├── pages/
│   │   └── index.astro      # Página principal (une todas las secciones)
│   └── styles/
│       └── global.css       # Estilos globales y animaciones keyframe
├── astro.config.mjs
├── package.json
└── tsconfig.json
```

## Secciones de la landing

| Componente | Descripcion |
|------------|-------------|
| `Navbar` | Navegacion fija con enlaces a las secciones |
| `Hero` | Encabezado principal con animacion de entrada GSAP |
| `Services` | Servicios ofrecidos por el taller |
| `About` | Historia y presentacion del taller |
| `Equipment` | Equipos de diagnostico electronico disponibles |
| `WhyUs` | Razones para elegir Taller Gerardito |
| `Brands` | Marcas de vehiculos que atiende el taller |
| `Testimonials` | Opiniones de clientes |
| `Contact` | Informacion de contacto y formulario |
| `Map` | Mapa de Google Maps con carga diferida (lazy) |
| `Footer` | Pie de pagina con enlaces y datos del negocio |
| `WhatsAppButton` | Boton flotante de acceso rapido a WhatsApp |

## Caracteristicas tecnicas

### Animaciones
- Animacion de entrada en el `Hero` al cargar la pagina
- Efectos de aparicion progresiva (scroll reveal) en cada seccion
- Efectos escalonados (stagger) en listas y tarjetas
- Respeto por `prefers-reduced-motion` para accesibilidad

### SEO
- Meta tags completos (`title`, `description`, `keywords`)
- Open Graph para redes sociales
- Schema.org JSON-LD con tipo `AutoRepair`
- Atributo `lang="es"` en el documento HTML

### Rendimiento
- Google Maps cargado con lazy loading
- Componentes Astro compilados como HTML estatico (sin JS innecesario)
- GSAP importado solo donde se necesita

### Diseno
- Mobile-first y totalmente responsivo
- Paleta de colores: azul oscuro `#1e3a5f` + naranja `#f97316`
- Contraste WCAG AA en textos principales
- HTML semantico con roles y landmarks accesibles

## Despliegue

El sitio genera archivos estaticos en la carpeta `dist/` tras ejecutar `npm run build`. Puede desplegarse en cualquier servicio de hosting estatico:

- [Netlify](https://netlify.com)
- [Vercel](https://vercel.com)
- [Cloudflare Pages](https://pages.cloudflare.com)
- Cualquier servidor con soporte para archivos HTML/CSS/JS

```bash
# Generar archivos de produccion
npm run build

# El contenido listo para subir estara en ./dist/
```

## Licencia

Proyecto privado — Taller Gerardito, San Manuel, Cortes, Honduras.
