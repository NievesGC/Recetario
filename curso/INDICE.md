# 📚 Índice del Curso — Proyecto Recetario

> **Objetivo:** Aprender React, FastAPI, PostgreSQL y el stack completo de desarrollo web moderno construyendo una aplicación real paso a paso.

---

## 🟦 FASE 1: Fundamentos de React

### 1.1 Introducción a React
- ¿Qué es React y por qué existe?
- Diferencias con JavaScript vanilla
- El concepto de "componente"
- Virtual DOM (qué es y por qué importa)
- JSX: HTML dentro de JavaScript

### 1.2 Tu primer componente
- Estructura básica de un componente
- Props: pasar datos entre componentes
- Renderizado condicional
- Listas y el atributo `key`

### 1.3 Estado y eventos
- `useState`: el hook más importante
- Eventos en React (onClick, onChange...)
- Formularios controlados
- Re-renderizado: cuándo y por qué

### 1.4 Efectos y ciclo de vida
- `useEffect`: ejecutar código en momentos específicos
- Efectos de montaje y desmontaje
- Dependencias del useEffect
- Cuándo NO usar useEffect

### 1.5 Estructura de un proyecto React moderno
- Vite: qué es y por qué lo usamos
- Organización de carpetas
- Componentes vs páginas
- Importaciones y exports

**🎯 Resultado de la Fase 1:**
- Entiendes qué es React y cómo funciona
- Sabes crear componentes funcionales
- Manejas estado con useState
- Comprendes el flujo de datos (props y estado)

---

## 🟦 FASE 2: Construir la interfaz del catálogo

### 2.1 Configuración inicial
- Crear proyecto con Vite
- Estructura de carpetas del frontend
- Instalar dependencias básicas
- Primer componente: `App.tsx`

### 2.2 Componentes del catálogo
- `RecipeCard`: tarjeta de receta
- `RecipeGrid`: grid de recetas
- Props y tipos en TypeScript (introducción suave)
- CSS Modules o SCSS (según prefieras)

### 2.3 Navegación con React Router
- Qué es React Router y para qué sirve
- Configurar rutas básicas
- Navegación entre páginas
- Parámetros de URL (para detalle de receta)

### 2.4 Página de detalle
- `RecipeDetail`: mostrar una receta completa
- Obtener el ID desde la URL
- Layout común (Navbar, footer)

**🎯 Resultado de la Fase 2:**
- Interfaz visual del catálogo funcionando
- Navegación entre páginas
- Datos de ejemplo (mock data) mostrándose correctamente

---

## 🟩 FASE 3: Backend con FastAPI y PostgreSQL

### 3.1 Conceptos de API REST
- ¿Qué es una API REST?
- HTTP: GET, POST, PUT, DELETE
- JSON: el lenguaje de las APIs
- Códigos de estado (200, 404, 500...)
- Estructura de una petición y una respuesta

### 3.2 SQL desde cero
- ¿Qué es una base de datos relacional?
- Tablas, filas y columnas
- Tipos de datos en SQL
- Primary Key y Foreign Key
- Relaciones: uno a muchos, muchos a muchos

### 3.3 Consultas SQL básicas
- `SELECT`: leer datos
- `INSERT`: crear datos
- `UPDATE`: modificar datos
- `DELETE`: eliminar datos
- `WHERE`: filtrar resultados
- `JOIN`: combinar tablas

### 3.4 PostgreSQL en práctica
- Instalar PostgreSQL (con Docker)
- Crear la base de datos `recetario`
- Crear las tablas del proyecto
- Insertar datos de ejemplo
- Consultas básicas desde la terminal

### 3.5 Introducción a FastAPI
- ¿Qué es FastAPI y en qué se diferencia de Flask?
- Instalación y configuración
- Tu primer endpoint: `GET /`
- Ejecutar el servidor con Uvicorn
- Documentación automática en `/docs`

### 3.6 Conectar FastAPI con PostgreSQL
- SQLAlchemy: qué es un ORM
- Modelos de datos (User, Recipe, Ingredient...)
- Crear la conexión con la base de datos
- Migraciones con Alembic (introducción)

### 3.7 CRUD completo de Recetas
- `GET /api/v1/recipes`: listar recetas
- `GET /api/v1/recipes/{id}`: detalle de una receta
- `POST /api/v1/recipes`: crear receta
- `PUT /api/v1/recipes/{id}`: editar receta
- `DELETE /api/v1/recipes/{id}`: eliminar receta

### 3.8 Validación con Pydantic
- Schemas: separar modelos de BD de modelos de API
- Validación automática de datos
- Respuestas tipadas

**🎯 Resultado de la Fase 3:**
- API REST completa funcionando
- Base de datos PostgreSQL configurada
- Endpoints que devuelven recetas reales
- Comprendes SQL y cómo funciona una base de datos

---

## 🟨 FASE 4: Conectar frontend con backend

### 4.1 Peticiones HTTP desde React
- `fetch` vs `axios`
- Configurar axios con interceptores
- Variables de entorno en Vite

### 4.2 TanStack Query (React Query)
- ¿Qué problema resuelve?
- `useQuery`: obtener datos del servidor
- `useMutation`: enviar datos al servidor
- Caché automática
- Estados de carga y error

### 4.3 Mostrar recetas reales
- Reemplazar mock data por datos del backend
- Manejar estados de carga
- Manejar errores de red
- Actualizar el catálogo cuando se crea una receta

### 4.4 Formularios conectados
- React Hook Form: gestión de formularios
- Validación con Zod
- Enviar datos al backend
- Feedback al usuario (éxito, error)

**🎯 Resultado de la Fase 4:**
- El frontend consume la API real
- Catálogo muestra recetas de la base de datos
- Puedes crear recetas desde la interfaz

---

## 🟥 FASE 5: Autenticación con JWT

### 5.1 ¿Qué es JWT?
- Tokens de acceso vs sesiones tradicionales
- Estructura de un JWT
- Access token y refresh token
- ¿Dónde se guarda el token?

### 5.2 Sistema de registro y login (backend)
- Hash de contraseñas con bcrypt
- Endpoint de registro
- Endpoint de login
- Generar y devolver JWT

### 5.3 Proteger endpoints
- Middleware de autenticación
- Obtener el usuario desde el token
- Autorización por roles (user, admin)

### 5.4 Autenticación en React
- Zustand: estado global simple
- Guardar token en localStorage
- Interceptor de axios para añadir el token
- Renovar token automáticamente

### 5.5 Rutas protegidas
- `ProtectedRoute`: componente envolvente
- Redirección si no estás autenticado
- Mostrar/ocultar elementos según el usuario

**🎯 Resultado de la Fase 5:**
- Sistema completo de login y registro
- Solo usuarios autenticados pueden crear recetas
- El token se renueva automáticamente

---

## 🟪 FASE 6: Funcionalidades avanzadas

### 6.1 Formulario completo de recetas
- Paso a paso con drag & drop (React Beautiful DnD)
- Ingredientes con cantidades
- Selector de tipos de cocina
- Selector de electrodomésticos

### 6.2 Subida de imágenes
- Configurar Cloudinary
- Endpoint de subida en FastAPI
- Componente de upload en React
- Preview de imagen antes de subir

### 6.3 Filtros dinámicos
- Construir filtros desde la base de datos
- Combinar múltiples filtros
- Actualizar resultados en tiempo real
- Contador de resultados

### 6.4 Búsqueda y paginación
- Buscador de texto libre
- Paginación de resultados
- Parámetros de URL para compartir búsquedas

### 6.5 Exportar recetas
- Generar PDF con jsPDF
- Web Share API para compartir
- Fallback para navegadores sin soporte

**🎯 Resultado de la Fase 6:**
- Aplicación completamente funcional
- Todas las features del proyecto implementadas

---

## ⬛ FASE 7: Infraestructura y despliegue

### 7.1 Docker desde cero
- ¿Qué es Docker y para qué sirve?
- Contenedores vs máquinas virtuales
- Dockerfile para backend
- Dockerfile para frontend
- Docker Compose: levantar todo junto

### 7.2 Variables de entorno
- `.env` en desarrollo
- Secrets en producción
- No subir secretos a Git

### 7.3 Despliegue del backend
- Railway o Render (tutorial completo)
- Configurar PostgreSQL en la nube (Supabase)
- Variables de entorno en producción

### 7.4 Despliegue del frontend
- Vercel (tutorial completo)
- Build de producción
- Variables de entorno

### 7.5 Dominio personalizado
- Configurar DNS
- HTTPS automático

**🎯 Resultado de la Fase 7:**
- Proyecto desplegado y accesible online
- Todo funciona en producción

---

## 📖 APÉNDICES

### A. TypeScript para React
- Tipos básicos
- Interfaces vs Types
- Props tipadas
- Hooks tipados

### B. SCSS avanzado
- Variables y mixins
- Nesting
- Imports y organización

### C. Animaciones
- GSAP básico
- Framer Motion para componentes
- Cuándo usar cada una

### D. Testing (opcional)
- Jest para backend
- React Testing Library
- Tests de integración

### E. Git y GitHub
- Workflow con ramas
- Pull requests
- Resolver conflictos

---

## 🎯 Glosario de términos

**API:** Application Programming Interface. Forma en que dos programas se comunican.

**Backend:** Parte del servidor, donde está la lógica y la base de datos.

**Component:** Pieza reutilizable de interfaz en React.

**CRUD:** Create, Read, Update, Delete. Operaciones básicas de datos.

**Endpoint:** URL específica de una API (ej: `/api/v1/recipes`).

**Frontend:** Parte del navegador, lo que ve el usuario.

**Hook:** Función especial de React que empieza por `use`.

**JWT:** JSON Web Token. Sistema de autenticación sin sesiones.

**ORM:** Object-Relational Mapping. Trabajar con BD usando objetos.

**Props:** Datos que se pasan de un componente padre a uno hijo.

**REST:** Estilo de arquitectura para APIs (usa GET, POST, PUT, DELETE).

**State:** Datos que React vigila y que provocan re-renderizado.

**Token:** Cadena de texto que identifica a un usuario autenticado.

---

**Última actualización:** Febrero 2025  
**Versión del índice:** 1.0
