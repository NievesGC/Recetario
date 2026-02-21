# Recetario — Documentación del Proyecto

> **Versión:** 2.0 · 2025
> **Stack principal:** FastAPI · React · PostgreSQL
> **Estado:** En desarrollo

---

## Índice

1. [Visión del Producto](#1-visión-del-producto)
2. [Roles y Permisos](#2-roles-y-permisos)
3. [Flujos de Usuario](#3-flujos-de-usuario)
4. [Funcionalidades](#4-funcionalidades)
5. [Stack Tecnológico](#5-stack-tecnológico)
6. [Arquitectura del Sistema](#6-arquitectura-del-sistema)
7. [Modelos de Datos](#7-modelos-de-datos)
8. [API REST — Referencia de Endpoints](#8-api-rest--referencia-de-endpoints)
9. [Requisitos No Funcionales](#9-requisitos-no-funcionales)
10. [Roadmap](#10-roadmap)
11. [Infraestructura y Costes](#11-infraestructura-y-costes)

---

## 1. Visión del Producto

**Recetario** es una plataforma web colaborativa para gestionar y descubrir recetas de cocina. Cualquier persona puede explorar el catálogo sin necesidad de registro. Los usuarios que se registran pueden contribuir al catálogo publicando sus propias recetas, y al hacerlo también enriquecen el sistema de búsqueda y filtrado disponible para toda la comunidad.

La plataforma se sustenta en tres principios:

**Catálogo abierto.** No existe una base de datos predefinida de ingredientes, electrodomésticos ni tipos de cocina. Son los propios usuarios quienes construyen ese vocabulario compartido a medida que crean recetas. Cada nuevo ingrediente, electrodoméstico o tipo de cocina registrado queda disponible para toda la comunidad y se incorpora automáticamente al sistema de filtros del catálogo público.

**Experiencia visual cuidada.** La interfaz está diseñada para ser atractiva y fluida en cualquier dispositivo — escritorio, tablet y móvil — con animaciones que hacen la navegación agradable sin sacrificar rendimiento.

**Arquitectura pensada para crecer.** El sistema está diseñado desde el inicio para incorporar en el futuro funcionalidades como generación de imágenes con IA, listas de la compra, planificación de menús semanales y seguimiento calórico, sin necesidad de reescribir la base existente.

---

## 2. Roles y Permisos

La plataforma define tres niveles de acceso acumulativos. Cada nivel incluye todos los permisos del nivel anterior.

### Visitante (sin registro)

El visitante puede acceder a todo el contenido público de la plataforma sin necesidad de crear una cuenta.

| Acción | ✓ / ✗ |
|---|---|
| Ver el catálogo de recetas | ✓ |
| Usar los filtros y el buscador | ✓ |
| Ver el detalle completo de una receta | ✓ |
| Descargar una receta en PDF | ✓ |
| Compartir una receta | ✓ |
| Crear recetas, ingredientes o electrodomésticos | ✗ |

### Usuario registrado

El usuario registrado puede consumir todo el contenido público y además contribuir al catálogo de la plataforma.

| Acción | ✓ / ✗ |
|---|---|
| Todo lo que puede hacer el visitante | ✓ |
| Crear, editar y eliminar sus propias recetas | ✓ |
| Guardar recetas como borrador antes de publicar | ✓ |
| Subir imagen para sus recetas | ✓ |
| Registrar nuevos ingredientes (con enriquecimiento IA) | ✓ |
| Registrar nuevos electrodomésticos | ✓ |
| Registrar nuevos tipos de cocina | ✓ |
| Editar o eliminar contenido de otros usuarios | ✗ |

### Administrador

El administrador tiene control total sobre el contenido y los usuarios de la plataforma.

| Acción | ✓ / ✗ |
|---|---|
| Todo lo que puede hacer el usuario registrado | ✓ |
| Editar o eliminar recetas de cualquier usuario | ✓ |
| Editar o eliminar ingredientes, electrodomésticos y tipos de cocina | ✓ |
| Gestionar cuentas de usuario (activar, suspender, cambiar rol) | ✓ |
| Acceder al panel de administración con estadísticas | ✓ |
| Fusionar o limpiar entradas duplicadas del catálogo | ✓ |

---

## 3. Flujos de Usuario

Esta sección describe los recorridos principales que un usuario puede realizar en la plataforma, paso a paso.

---

### 3.1 Explorar el catálogo sin registro

Este es el flujo de un visitante que llega a la plataforma por primera vez.

1. El visitante accede a la página principal. Ve el catálogo de recetas con una animación de entrada escalonada en las tarjetas.
2. En el panel lateral (o superior en móvil) encuentra los filtros disponibles: tipo de cocina, electrodoméstico, ingrediente y tiempo máximo. Estos filtros se construyen dinámicamente con el contenido que los usuarios han registrado — no son categorías fijas.
3. El visitante activa uno o varios filtros. El catálogo se actualiza en tiempo real mostrando únicamente las recetas que cumplen todos los criterios activos. Un contador indica cuántos resultados coinciden.
4. El visitante también puede usar el buscador libre para encontrar recetas por nombre o descripción, combinable con los filtros.
5. El visitante hace clic en una tarjeta de receta. Se abre la página de detalle con animación de transición.
6. En la página de detalle ve la imagen, descripción, tiempos, ingredientes con cantidades, electrodomésticos necesarios y pasos numerados.
7. Desde la página de detalle puede pulsar **Descargar PDF** para obtener la receta en formato imprimible, o **Compartir** para enviarla por WhatsApp, email u otras apps (en móvil se abre el menú nativo del sistema operativo; en escritorio se copia el enlace).

---

### 3.2 Registro de un nuevo usuario

1. El visitante pulsa **Registrarse** en la barra de navegación.
2. Rellena el formulario: nombre de usuario, email y contraseña. La contraseña requiere un mínimo de seguridad (longitud, caracteres).
3. El sistema valida en tiempo real que el email y el nombre de usuario no estén ya en uso, mostrando feedback inmediato bajo cada campo.
4. Al enviar el formulario, el backend crea la cuenta, genera un `access_token` (JWT, duración 15 minutos) y un `refresh_token` (duración 7 días), y los devuelve al frontend.
5. El frontend almacena los tokens, actualiza el estado global de sesión (Zustand) y redirige al usuario a su dashboard personal.
6. La barra de navegación ahora muestra el nombre de usuario y las opciones de usuario registrado.

---

### 3.3 Inicio de sesión

1. El usuario pulsa **Iniciar sesión**.
2. Introduce su email y contraseña.
3. El backend valida las credenciales, genera nuevos tokens y los devuelve.
4. El frontend almacena los tokens y redirige al dashboard.
5. Si el `access_token` expira durante la sesión, el frontend lo renueva automáticamente usando el `refresh_token` mediante un interceptor de Axios, de forma transparente para el usuario.

---

### 3.4 Registrar un nuevo ingrediente

Los ingredientes son el vocabulario base de las recetas. Cualquier usuario registrado puede añadir ingredientes que quedarán disponibles para toda la comunidad y aparecerán en los filtros del catálogo.

1. El usuario accede a **Mi cuenta → Ingredientes → Nuevo ingrediente**, o bien llega a este formulario directamente desde el formulario de creación de recetas (flujo create-on-the-fly).
2. Rellena el nombre del ingrediente (obligatorio). El sistema comprueba en tiempo real si ya existe un ingrediente con ese nombre para evitar duplicados, mostrando una advertencia si es así.
3. Selecciona la familia del ingrediente (hortalizas, frutas, lácteos, carnes, pescados, cereales, legumbres, frutos secos, especias...). Si la familia que necesita no existe, puede escribirla directamente y se creará como nueva opción.
4. Para las calorías por 100 g, tiene dos opciones:
   - Introduce el valor manualmente.
   - Pulsa **Obtener con IA**. El sistema envía el nombre del ingrediente a OpenAI y en unos segundos precarga el campo con la estimación nutricional obtenida. El usuario puede aceptarla, modificarla o descartarla antes de guardar.
5. Opcionalmente rellena la unidad de medida habitual (gramos, mililitros, unidad, cucharada...) y selecciona los alérgenos asociados de entre los 14 reconocidos por la normativa europea.
6. Al guardar, el ingrediente queda disponible en el buscador de ingredientes del formulario de recetas y aparece como opción en los filtros del catálogo público.

---

### 3.5 Registrar un nuevo electrodoméstico

1. El usuario accede a **Mi cuenta → Electrodomésticos → Nuevo electrodoméstico**, o bien desde el formulario de recetas.
2. Introduce únicamente el nombre del electrodoméstico (horno, thermomix, freidora de aire, vaporera...).
3. Al guardar, el electrodoméstico queda disponible en el selector del formulario de recetas y aparece como opción de filtrado en el catálogo público.

---

### 3.6 Registrar un nuevo tipo de cocina

1. El usuario accede a **Mi cuenta → Tipos de cocina → Nuevo tipo**, o bien desde el formulario de recetas.
2. Introduce el nombre del tipo de cocina (vegana, mediterránea, asiática, sin gluten, mexicana...).
3. El sistema genera automáticamente el slug normalizado (ej. `sin-gluten`).
4. Al guardar, el tipo de cocina queda disponible en el selector de recetas y como filtro en el catálogo.

---

### 3.7 Crear una receta

Este es el flujo central de la plataforma para un usuario registrado.

1. El usuario pulsa **Nueva receta** desde su dashboard o desde la barra de navegación.
2. Rellena el formulario en secciones:

   **Información básica**
   - Nombre de la receta (obligatorio).
   - Descripción breve (obligatorio).
   - Tiempo de preparación en minutos (opcional).
   - Tiempo de cocción en minutos (opcional).

   **Clasificación**
   - Tipos de cocina: selector con búsqueda. Puede seleccionar varios. Si el tipo que necesita no existe, puede crearlo en ese mismo momento sin salir del formulario — se abre un pequeño modal, se crea el tipo y queda seleccionado automáticamente.
   - Electrodomésticos necesarios: funciona igual que los tipos de cocina. Selector múltiple con opción de crear en el momento.

   **Ingredientes**
   - Buscador con icono de lupa que filtra en tiempo real por nombre. También puede filtrar por familia para acotar la búsqueda.
   - Por cada ingrediente seleccionado, el usuario introduce la cantidad y la unidad. La unidad se prerrellena con la habitual del ingrediente pero es editable.
   - Si el ingrediente que necesita no existe en el catálogo, puede crearlo desde aquí (se abre el formulario de ingrediente en un modal, se crea y queda añadido a la receta).

   **Pasos de elaboración**
   - El usuario añade los pasos uno a uno pulsando **Añadir paso**.
   - Cada paso tiene: descripción del paso y duración estimada opcional.
   - Los pasos se pueden reordenar arrastrando y soltando (drag & drop).
   - Se pueden eliminar individualmente.
   - No hay límite en el número de pasos.

   **Imagen**
   - El usuario puede subir una foto propia (JPG, PNG o WebP, máximo 5 MB).
   - La imagen se sube a Cloudinary y se almacena su URL en la base de datos.
   - Si no sube imagen, la receta se muestra con un placeholder en el catálogo.
   - *(La generación automática de imagen con IA está preparada en la arquitectura pero no disponible en esta versión.)*

3. Antes de publicar, el usuario puede guardar la receta como **borrador**. Los borradores son visibles únicamente para el autor en su dashboard, no aparecen en el catálogo público.

4. Al pulsar **Publicar**, la receta queda visible en el catálogo público inmediatamente y los filtros del catálogo se actualizan para incluir los ingredientes, electrodomésticos y tipos de cocina de esa receta si no estaban ya representados.

---

### 3.8 Editar o eliminar una receta propia

1. El usuario accede a su dashboard, donde ve la lista de todas sus recetas (publicadas y borradores).
2. Pulsa **Editar** en la receta que desea modificar.
3. Se carga el formulario con todos los campos precargados con los valores actuales.
4. El usuario modifica lo que necesite — puede cambiar cualquier campo, reordenar pasos, añadir o quitar ingredientes, cambiar la imagen o despublicar la receta convirtiéndola en borrador.
5. Al guardar, los cambios se reflejan inmediatamente en la página de detalle de la receta.
6. Para eliminar la receta, el usuario pulsa **Eliminar** y confirma en un diálogo. La receta se borra junto con todos sus pasos y relaciones.

---

### 3.9 Descargar o compartir una receta

Disponible para cualquier usuario, con o sin registro, desde la página de detalle de cualquier receta.

**Descargar como PDF**
1. El usuario pulsa **Descargar PDF**.
2. El navegador genera el PDF en el cliente (sin llamada al servidor) usando `jsPDF` y `html2canvas`.
3. El archivo se descarga con el nombre `nombre-de-la-receta.pdf`.
4. El PDF incluye: nombre, descripción, imagen, tiempos, tipos de cocina, electrodomésticos, lista de ingredientes con cantidades y pasos numerados. A pie de página figura la URL de la receta en la plataforma.
5. Funciona en escritorio, tablet y móvil. En móvil, el PDF se descarga a la carpeta de descargas del dispositivo o se abre con el visor de PDF nativo.

**Compartir**
1. El usuario pulsa **Compartir**.
2. En móvil y tablet, el sistema operativo abre su menú nativo de compartir: el usuario puede enviar la receta por WhatsApp, Telegram, email, guardarla en notas, compartirla por AirDrop (iOS) o cualquier otra app instalada.
3. En escritorio, si el navegador soporta Web Share API se abre el diálogo nativo; si no, aparece un pequeño popover con la URL de la receta y el botón **Copiar enlace**.

---

### 3.10 Flujo de administración

1. El administrador accede a **/admin** desde su menú de usuario.
2. El panel de administración muestra estadísticas básicas: número de recetas publicadas, ingredientes registrados, usuarios activos.
3. Desde el panel puede:
   - **Gestionar usuarios**: ver la lista completa, activar o suspender cuentas, cambiar roles.
   - **Moderar recetas**: buscar y eliminar recetas que incumplan las normas de la plataforma.
   - **Limpiar el catálogo**: eliminar ingredientes, electrodomésticos o tipos de cocina duplicados o erróneos que no estén en uso por ninguna receta.
4. Todas las acciones destructivas requieren confirmación explícita.

---

## 4. Funcionalidades

### 4.1 Catálogo público

- Grid de recetas con animación de entrada escalonada al cargar.
- Filtros dinámicos construidos en tiempo real desde la base de datos: tipo de cocina, electrodoméstico, ingrediente y tiempo máximo.
- Filtros combinables y acumulativos. Contador de resultados activo.
- Buscador de texto libre combinable con los filtros.
- Paginación de resultados.
- Transiciones animadas entre el catálogo y el detalle de receta.

### 4.2 Detalle de receta

- Imagen, nombre, descripción y tiempos.
- Etiquetas de tipos de cocina y electrodomésticos necesarios.
- Lista de ingredientes con cantidades y unidades.
- Pasos numerados con duración opcional por paso.
- Botón de descarga en PDF y botón de compartir.

### 4.3 Gestión de contenido (usuario registrado)

- Formulario de creación y edición de recetas con pasos drag & drop.
- Buscador de ingredientes con filtro por familia dentro del formulario de receta.
- Creación de ingredientes, electrodomésticos y tipos de cocina desde el formulario de receta sin salir de la pantalla (create-on-the-fly).
- Enriquecimiento automático de calorías vía IA al registrar ingredientes.
- Subida de imagen a Cloudinary.
- Gestión de borradores.
- Dashboard personal con listado de recetas propias.

### 4.4 Sistema de filtros dinámicos

Cuando un usuario registra un nuevo ingrediente, electrodoméstico o tipo de cocina, este queda disponible para toda la plataforma y se incorpora automáticamente a los filtros del catálogo. El frontend invalida la caché de TanStack Query al crear cualquier elemento de estas categorías, de modo que los filtros se actualizan en la siguiente visita al catálogo sin necesidad de recargar la página manualmente.

### 4.5 Exportación

- **PDF:** generado en el navegador del usuario con `jsPDF` + `html2canvas`. Sin coste de servidor. Compatible con todos los dispositivos y navegadores modernos.
- **Compartir:** Web Share API nativa con fallback de copiar enlace en escritorio.

### 4.6 Autenticación y seguridad

- Registro e inicio de sesión con email y contraseña.
- JWT con `access_token` de corta duración (15 min) y `refresh_token` de larga duración (7 días).
- Renovación automática del token mediante interceptor en Axios.
- Contraseñas hasheadas con bcrypt.
- Rutas protegidas en el frontend según el rol del usuario.
- Autorización en cada endpoint del backend: validación del rol antes de ejecutar operaciones sensibles.

### 4.7 Panel de administración

- Gestión de usuarios: listar, suspender, cambiar rol.
- Moderación de recetas, ingredientes, electrodomésticos y tipos de cocina.
- Estadísticas básicas de uso de la plataforma.

---

## 5. Stack Tecnológico

### 5.1 Backend

| Tecnología | Versión | Uso |
|---|---|---|
| **Python** | ^3.12 | Lenguaje base |
| **FastAPI** | ^0.110 | Framework API REST |
| **PostgreSQL** | ^16 | Base de datos relacional |
| **SQLAlchemy** | ^2.0 | ORM |
| **Alembic** | ^1.13 | Migraciones de base de datos |
| **Pydantic v2** | ^2.6 | Validación de datos y esquemas |
| **Python-Jose** | ^3.3 | JWT |
| **Passlib + bcrypt** | latest | Hashing de contraseñas |
| **Httpx** | ^0.27 | Cliente HTTP async |
| **OpenAI SDK** | ^1.x | Enriquecimiento nutricional con GPT-4o mini |
| **Cloudinary SDK** | ^1.x | Almacenamiento y CDN de imágenes |
| **Pytest + pytest-asyncio** | ^8.x | Tests |
| **Uvicorn** | ^0.29 | Servidor ASGI |

### 5.2 Frontend

| Tecnología | Versión | Uso |
|---|---|---|
| **React** | ^18.3 | Framework UI |
| **Vite** | ^5.x | Bundler |
| **TypeScript** | ^5.x | Tipado estático |
| **React Router v6** | ^6.x | Enrutamiento SPA |
| **TanStack Query** | ^5.x | Estado del servidor y caché |
| **Zustand** | ^4.x | Estado global del cliente |
| **React Hook Form** | ^7.x | Formularios |
| **Zod** | ^3.x | Validación de esquemas |
| **Axios** | ^1.x | HTTP client con interceptores JWT |
| **React Beautiful DnD** | ^13.x | Drag & drop en pasos de receta |
| **jsPDF + html2canvas** | latest | Generación de PDF en el cliente |

### 5.3 Animación

| Tecnología | Versión | Uso |
|---|---|---|
| **GSAP** | ^3.12 | Animaciones de gran escala: entrada del catálogo, efectos de scroll, transiciones de página |
| **Framer Motion** | ^11.x | Animaciones declarativas por componente: modales, listas, micro-interacciones |

GSAP se usa para animaciones de gran escala o basadas en scroll. Framer Motion se aplica a nivel de componente para transiciones de estado y presencia en el DOM. Ambas librerías conviven sin conflicto.

### 5.4 Estilos y Diseño

| Tecnología | Versión | Uso |
|---|---|---|
| **Sass (SCSS)** | ^1.x | Sistema de diseño global: tokens de color, tipografía, espaciado, breakpoints, mixins |
| **Radix UI Primitives** | latest | Componentes headless accesibles: diálogos, dropdowns, selects, tooltips, tabs |
| **shadcn/ui** | latest | Capa de estilos sobre Radix UI, personalizable con CSS variables. Se copia directamente al proyecto, sin dependencia npm |
| **Lucide React** | ^0.400 | Iconos SVG ligeros y tree-shakeable |
| **clsx** | ^2.x | Condicionar clases CSS de forma limpia |
| **React Toastify** | ^10.x | Notificaciones de feedback al usuario |

> **Estrategia de estilos:** Sass gestiona el sistema de diseño global (variables, mixins, reset, grids). shadcn/ui proporciona los componentes de interfaz comunes (botones, inputs, modales, selects) ya accesibles y personalizables. Al ser código copiado al proyecto y no una dependencia externa, los componentes de shadcn/ui se pueden modificar libremente sin conflictos con los estilos SCSS.

### 5.5 Infraestructura

| Servicio | Uso |
|---|---|
| **Docker + Docker Compose** | Entorno local completo levantado con `docker-compose up` |
| **Supabase** | PostgreSQL gestionado en la nube |
| **Cloudinary** | CDN y almacenamiento de imágenes |
| **Railway / Render** | Despliegue del backend |
| **Vercel** | Despliegue del frontend |
| **GitHub Actions** | CI: tests y linting automáticos en cada PR |

---

## 6. Arquitectura del Sistema

### 6.1 Visión General

Arquitectura cliente-servidor desacoplada. El frontend React se comunica exclusivamente con el backend FastAPI a través de la API REST. El backend gestiona la lógica de negocio, la autenticación y la comunicación con servicios externos.

```
┌─────────────────┐         ┌──────────────────────┐         ┌─────────────┐
│   React (SPA)   │ ──────▶ │   FastAPI (REST API)  │ ──────▶ │ PostgreSQL  │
│  Vercel / CDN   │ ◀────── │  Railway / Render     │         │  Supabase   │
└─────────────────┘         └──────────────────────┘         └─────────────┘
                                        │
                          ┌─────────────┼──────────────┐
                          ▼             ▼              ▼
                     ┌─────────┐  ┌──────────┐  ┌──────────────┐
                     │ OpenAI  │  │Cloudinary│  │ (servicios   │
                     │   API   │  │   CDN    │  │  futuros)    │
                     └─────────┘  └──────────┘  └──────────────┘
```

### 6.2 Estructura de Carpetas — Backend

```
backend/
├── app/
│   ├── api/
│   │   └── v1/
│   │       ├── endpoints/
│   │       │   ├── auth.py
│   │       │   ├── users.py
│   │       │   ├── recipes.py
│   │       │   ├── ingredients.py
│   │       │   ├── appliances.py
│   │       │   └── cuisine_types.py
│   │       └── router.py
│   ├── core/
│   │   ├── config.py              # Variables de entorno (Pydantic Settings)
│   │   ├── security.py            # JWT, hashing, dependencias de auth
│   │   └── dependencies.py        # Inyección de dependencias comunes
│   ├── db/
│   │   ├── session.py
│   │   └── base.py
│   ├── models/                    # Modelos SQLAlchemy
│   ├── schemas/                   # Esquemas Pydantic (request / response)
│   ├── crud/                      # Acceso a base de datos
│   │   └── base.py                # CRUD genérico reutilizable
│   ├── services/
│   │   ├── ai_service.py          # OpenAI: calorías + preparado para imagen
│   │   └── image_service.py       # Cloudinary
│   └── main.py
├── alembic/
├── tests/
├── .env.example
├── Dockerfile
└── docker-compose.yml
```

### 6.3 Estructura de Carpetas — Frontend

```
frontend/
├── src/
│   ├── components/
│   │   ├── ui/                    # Componentes base (shadcn/ui adaptados)
│   │   ├── recipes/               # RecipeCard, RecipeGrid, RecipeForm, RecipeFilters
│   │   ├── ingredients/           # IngredientSearch, IngredientForm
│   │   └── layout/                # Navbar, ProtectedRoute
│   ├── pages/
│   │   ├── Home.tsx               # Catálogo público
│   │   ├── RecipeDetail.tsx
│   │   ├── Login.tsx / Register.tsx
│   │   ├── Dashboard.tsx          # Panel del usuario
│   │   └── Admin.tsx
│   ├── hooks/
│   │   ├── useRecipes.ts
│   │   ├── useIngredients.ts
│   │   ├── useAuth.ts
│   │   └── useRecipeExport.ts     # PDF y Web Share API
│   ├── services/                  # Llamadas a la API
│   │   └── api.ts                 # Instancia Axios con interceptores JWT
│   ├── store/
│   │   └── authStore.ts           # Sesión global (Zustand)
│   ├── styles/
│   │   ├── _variables.scss        # Tokens de diseño
│   │   ├── _mixins.scss
│   │   └── globals.scss
│   ├── animations/
│   │   ├── gsap.ts                # Timelines GSAP reutilizables
│   │   └── variants.ts            # Variantes Framer Motion
│   └── types/
│       └── index.ts
├── vite.config.ts
└── Dockerfile
```

---

## 7. Modelos de Datos

### 7.1 User

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | Identificador único |
| `email` | VARCHAR UNIQUE NOT NULL | Email, usado como credencial de login |
| `username` | VARCHAR UNIQUE NOT NULL | Nombre público |
| `hashed_password` | VARCHAR NOT NULL | Contraseña hasheada con bcrypt |
| `role` | ENUM NOT NULL | `user` o `admin`. Default: `user` |
| `is_active` | BOOLEAN | Default: `true` |
| `created_at` | TIMESTAMP | Fecha de registro |
| `updated_at` | TIMESTAMP | Última modificación |

### 7.2 Recipe

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | Identificador único |
| `name` | VARCHAR NOT NULL | Nombre de la receta |
| `description` | TEXT NOT NULL | Descripción breve |
| `prep_time_min` | INTEGER nullable | Tiempo de preparación en minutos |
| `cook_time_min` | INTEGER nullable | Tiempo de cocción en minutos |
| `image_url` | VARCHAR nullable | URL en Cloudinary |
| `image_generated_by_ai` | BOOLEAN | Flag reservado para futura generación con IA. Default: `false` |
| `is_published` | BOOLEAN | Publicada o borrador. Default: `true` |
| `author_id` | UUID FK → User | Autor de la receta |
| `created_at` | TIMESTAMP | |
| `updated_at` | TIMESTAMP | |

### 7.3 RecipeStep

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | |
| `recipe_id` | UUID FK → Recipe | |
| `order_index` | INTEGER NOT NULL | Posición del paso, permite reordenación |
| `description` | TEXT NOT NULL | Instrucción del paso |
| `duration_minutes` | INTEGER nullable | Duración estimada del paso |

### 7.4 Ingredient

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | |
| `name` | VARCHAR UNIQUE NOT NULL | |
| `family` | VARCHAR NOT NULL | Hortalizas, frutas, lácteos, etc. |
| `calories_per_100g` | FLOAT nullable | Obtenido por IA o introducido manualmente |
| `unit` | VARCHAR nullable | Unidad habitual: g, ml, unidad... |
| `allergens` | TEXT[] nullable | Array de alérgenos (normativa UE) |
| `created_by` | UUID FK → User | |
| `created_at` | TIMESTAMP | |

### 7.5 Appliance

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | |
| `name` | VARCHAR UNIQUE NOT NULL | Nombre del electrodoméstico |
| `created_by` | UUID FK → User | |
| `created_at` | TIMESTAMP | |

### 7.6 CuisineType

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | UUID PK | |
| `name` | VARCHAR UNIQUE NOT NULL | Nombre visible (ej. "Vegana") |
| `slug` | VARCHAR UNIQUE NOT NULL | Identificador URL (ej. "vegana") |
| `created_by` | UUID FK → User | |
| `created_at` | TIMESTAMP | |

### 7.7 Tablas pivot

```
recipe_ingredients
  recipe_id      UUID FK → Recipe
  ingredient_id  UUID FK → Ingredient
  quantity       FLOAT NOT NULL
  unit           VARCHAR NOT NULL

recipe_appliances
  recipe_id      UUID FK → Recipe
  appliance_id   UUID FK → Appliance

recipe_cuisine_types
  recipe_id         UUID FK → Recipe
  cuisine_type_id   UUID FK → CuisineType
```

---

## 8. API REST — Referencia de Endpoints

### Convenciones

- **Base URL:** `/api/v1`
- **Autenticación:** `Authorization: Bearer <access_token>`
- **Formato:** JSON en todos los endpoints
- **Paginación:** `?skip=0&limit=20` en endpoints de listado
- **Errores:** `{ "detail": "mensaje", "code": "ERROR_CODE" }`
- **Documentación automática:** `/docs` (Swagger UI) · `/redoc` (ReDoc)

### 8.1 Autenticación

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `POST` | `/api/v1/auth/register` | Público | Registro. Devuelve `access_token` y `refresh_token`. |
| `POST` | `/api/v1/auth/login` | Público | Login. Devuelve `access_token` y `refresh_token`. |
| `POST` | `/api/v1/auth/refresh` | Autenticado | Renueva el `access_token`. |
| `POST` | `/api/v1/auth/logout` | Autenticado | Invalida el token activo. |
| `GET` | `/api/v1/auth/me` | Autenticado | Perfil del usuario en sesión. |

### 8.2 Usuarios

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `GET` | `/api/v1/users` | Admin | Lista todos los usuarios con paginación. |
| `GET` | `/api/v1/users/{user_id}` | Admin / Propio | Perfil de un usuario. |
| `PUT` | `/api/v1/users/{user_id}` | Admin / Propio | Actualiza username o email. |
| `DELETE` | `/api/v1/users/{user_id}` | Admin | Elimina una cuenta. |
| `PATCH` | `/api/v1/users/{user_id}/role` | Admin | Cambia el rol: `user` ↔ `admin`. |
| `PATCH` | `/api/v1/users/{user_id}/status` | Admin | Activa o suspende la cuenta. |

### 8.3 Recetas

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `GET` | `/api/v1/recipes` | Público | Catálogo con filtros y paginación. |
| `GET` | `/api/v1/recipes/{recipe_id}` | Público | Detalle completo de una receta. |
| `GET` | `/api/v1/recipes/me` | Autenticado | Recetas del usuario en sesión (incluye borradores). |
| `POST` | `/api/v1/recipes` | Autenticado | Crea una receta. |
| `PUT` | `/api/v1/recipes/{recipe_id}` | Autor / Admin | Actualiza una receta. |
| `DELETE` | `/api/v1/recipes/{recipe_id}` | Autor / Admin | Elimina una receta y sus pasos. |
| `POST` | `/api/v1/recipes/{recipe_id}/image` | Autor / Admin | Sube imagen (multipart/form-data) a Cloudinary. |
| `DELETE` | `/api/v1/recipes/{recipe_id}/image` | Autor / Admin | Elimina la imagen de una receta. |

**Parámetros de filtrado en `GET /api/v1/recipes`:**

| Parámetro | Tipo | Descripción |
|---|---|---|
| `search` | string | Texto libre en nombre y descripción |
| `cuisine_type` | string | Slug del tipo de cocina (ej. `vegana`) |
| `ingredient_id` | UUID | Recetas que contienen ese ingrediente |
| `appliance_id` | UUID | Recetas que requieren ese electrodoméstico |
| `max_time` | integer | Tiempo total máximo en minutos |
| `author_id` | UUID | Recetas de un usuario concreto |
| `skip` | integer | Offset de paginación. Default: `0` |
| `limit` | integer | Resultados por página. Default: `20`, máx: `100` |

### 8.4 Ingredientes

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `GET` | `/api/v1/ingredients` | Público | Lista con búsqueda y filtro por familia. |
| `GET` | `/api/v1/ingredients/{ingredient_id}` | Público | Detalle de un ingrediente. |
| `POST` | `/api/v1/ingredients` | Autenticado | Crea un ingrediente. |
| `PUT` | `/api/v1/ingredients/{ingredient_id}` | Autor / Admin | Actualiza un ingrediente. |
| `DELETE` | `/api/v1/ingredients/{ingredient_id}` | Admin | Elimina un ingrediente. |
| `POST` | `/api/v1/ingredients/enrich` | Autenticado | Consulta OpenAI con el nombre del ingrediente y devuelve la estimación de calorías por 100 g. No guarda en base de datos: el frontend recibe el dato para que el usuario lo confirme. |

### 8.5 Electrodomésticos

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `GET` | `/api/v1/appliances` | Público | Lista todos los electrodomésticos. |
| `GET` | `/api/v1/appliances/{appliance_id}` | Público | Detalle. |
| `POST` | `/api/v1/appliances` | Autenticado | Crea un electrodoméstico. Body: `{ "name": "..." }`. |
| `PUT` | `/api/v1/appliances/{appliance_id}` | Autor / Admin | Actualiza el nombre. |
| `DELETE` | `/api/v1/appliances/{appliance_id}` | Admin | Elimina. Solo si no está en uso por ninguna receta. |

### 8.6 Tipos de Cocina

| Método | Endpoint | Acceso | Descripción |
|---|---|---|---|
| `GET` | `/api/v1/cuisine-types` | Público | Lista todos los tipos de cocina. |
| `GET` | `/api/v1/cuisine-types/{cuisine_type_id}` | Público | Detalle. |
| `POST` | `/api/v1/cuisine-types` | Autenticado | Crea un tipo de cocina. El slug se genera automáticamente. |
| `PUT` | `/api/v1/cuisine-types/{cuisine_type_id}` | Autor / Admin | Actualiza nombre y regenera slug. |
| `DELETE` | `/api/v1/cuisine-types/{cuisine_type_id}` | Admin | Elimina. Solo si no está en uso. |

---

## 9. Requisitos No Funcionales

**Responsive.** La interfaz funciona correctamente en escritorio, tablet y móvil. El diseño adapta el layout de forma fluida en todos los breakpoints.

**Accesibilidad.** Los componentes de Radix UI garantizan accesibilidad base: atributos ARIA correctos, navegación por teclado, gestión del foco en modales y dropdowns.

**Rendimiento.** TanStack Query gestiona la caché de datos del servidor con tiempos de revalidación apropiados para cada tipo de dato (los filtros del catálogo tienen un `staleTime` mayor que los resultados de búsqueda, por ejemplo). Las imágenes se sirven optimizadas desde Cloudinary.

**Seguridad.** Ningún secreto está hardcodeado en el código. Todas las claves y configuraciones sensibles se gestionan mediante variables de entorno. La autorización se valida en cada endpoint del backend. Las contraseñas se almacenan siempre hasheadas.

**Escalabilidad.** La arquitectura mantiene separación estricta de capas (router → crud → service → model) y usa inyección de dependencias para los servicios externos, lo que permite sustituirlos o ampliarlos sin tocar la lógica de negocio.

**Reproducibilidad.** El entorno completo se levanta en local con un único comando: `docker-compose up`. El repositorio incluye un `.env.example` con todas las variables necesarias documentadas.

**Tests.** El proyecto incluye tests unitarios y de integración para los módulos de autenticación, recetas e ingredientes. Cobertura mínima orientativa del 60% en el backend.

**Documentación de la API.** FastAPI genera automáticamente documentación OpenAPI. Todos los endpoints incluyen descripción, tipos de parámetros y ejemplos de respuesta. Accesible en `/docs` y `/redoc`.

---

## 10. Roadmap

Las siguientes funcionalidades están previstas para iteraciones futuras. La arquitectura actual las contempla y no requerirá cambios estructurales para incorporarlas.

### Generación de imagen con IA *(preparado, no implementado)*

El modelo `Recipe` ya incluye el campo `image_generated_by_ai` y el servicio `ai_service.py` está estructurado para añadir un método de generación de imágenes (DALL·E 3 o equivalente) como opción adicional en el formulario de receta. Cuando se active, el coste estimado será de 0,04–0,08 € por imagen generada.

### Listas de la compra

A partir de una selección de recetas, la IA agrupa y suma los ingredientes necesarios para generar una lista de la compra consolidada, eliminando duplicados y ajustando cantidades al número de comensales.

### Planificación de menús semanales

El usuario indica restricciones (tipos de cocina, ingredientes a evitar, número de comensales y número de comidas al día) y la IA propone un menú semanal completo usando las recetas disponibles en la plataforma.

### Seguimiento calórico

Aprovechando los datos de `calories_per_100g` almacenados en los ingredientes, el sistema calculará automáticamente las calorías por ración de cada receta y podrá llevar un registro de ingesta diaria.

---

## 11. Infraestructura y Costes

### Servicios con tier gratuito suficiente para desarrollo y MVP

| Servicio | Uso | Coste |
|---|---|---|
| **GitHub** | Repositorio, Issues, CI | Gratuito |
| **Supabase** | PostgreSQL gestionado (500 MB, 2 proyectos) | Gratuito |
| **Cloudinary** | Almacenamiento de imágenes (25 GB, 25 k transformaciones/mes) | Gratuito |
| **Railway / Render** | Backend FastAPI | Gratuito (con limitaciones de sleep en inactividad) |
| **Vercel** | Frontend React | Gratuito |

### Servicios de pago

| Servicio | Concepto | Coste estimado |
|---|---|---|
| **OpenAI API** | GPT-4o mini para enriquecimiento de calorías | < 1 €/mes con uso moderado |
| **Dominio** | .com o .es (Cloudflare Registrar / Namecheap) | ~12 €/año |
| **Hetzner CX22** | VPS si se supera el tier gratuito | Desde 4 €/mes |

### Proyección de costes por escenario

| Escenario | Coste mensual |
|---|---|
| Desarrollo y pruebas en local | 0 € |
| MVP publicado con uso bajo | < 1 €/mes + ~1 €/año de dominio |
| Producción con tráfico real | 4–10 €/mes (VPS) + dominio |
| Con generación de imágenes IA activa | +0,04–0,08 € por imagen generada |
