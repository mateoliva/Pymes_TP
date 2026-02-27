# Pymes_TP

Proyecto realizado en clase de **Desarrollo de Software (2025)**.

Aplicación web tipo “Pymes” con:
- **Backend**: API REST en **Node.js + Express**, persistencia con **Sequelize + SQLite**, y autenticación **JWT**.
- **Frontend**: **SPA** en **React (Vite)** + **Bootstrap**, consumiendo la API en `http://localhost:3000`.

---

## Tecnologías utilizadas

### Backend
- Node.js, Express
- API REST
- Sequelize + SQLite
- JWT (login, refresh token)
- Tests: Jest + Supertest

### Frontend
- React + Vite (SPA)
- HTML, CSS, Bootstrap
- JavaScript

---

## Estructura del repositorio

- `Pymes/BackendDDS/dds-backend` → Backend (Express + Sequelize + SQLite)
- `Pymes/FrontendDDS/dds-frontend` → Frontend (React + Vite)

---

## Funcionalidades / Alcance

### Artículos
- Listado paginado con filtros por **Nombre** y **Activo**
- Obtener artículo por id
- Alta / Modificación
- Eliminación con **baja lógica** (toggle Activo)

### Categorías
- Listado de categorías
- Obtener categoría por id
- Endpoint de categorías mock (sin BD)

### Seguridad
- Login con JWT (access token)
- Refresh token y logout
- Endpoint protegido `/api/usuarios` (solo rol `jefe`)

---

## Requisitos

- Node.js / npm instalados
- Recomendado: Node 18+ (o la versión que uses normalmente)

---

## Cómo ejecutar (desarrollo)

### 1) Backend (puerto 3000)
```bash
cd Pymes/BackendDDS/dds-backend
npm install
npm run dev
```

Endpoints útiles para verificar:
- `GET http://localhost:3000/` → "Backend inicial dds-backend!"
- `GET http://localhost:3000/_isalive`

### 2) Frontend (Vite, normalmente puerto 5173)
```bash
cd Pymes/FrontendDDS/dds-frontend
npm install
npm run dev
```

Abrir en el navegador la URL que informe Vite (por defecto suele ser):
- `http://localhost:5173`

> El frontend consume la API desde: `http://localhost:3000`

---

## Endpoints principales (Backend)

### Artículos
- `GET /api/articulos?Pagina=1&Nombre=<texto>&Activo=true|false`
- `GET /api/articulos/:id`
- `POST /api/articulos`
- `PUT /api/articulos/:id`
- `DELETE /api/articulos/:id` (baja lógica)

### Categorías
- `GET /api/categorias`
- `GET /api/categorias/:id`
- `GET /api/categoriasmock`
- `GET /api/categoriasmock/:id`

### Seguridad
- `POST /api/login`
- `POST /api/logout`
- `POST /api/refreshtoken`

### Usuarios (protegido)
- `GET /api/usuarios` (requiere JWT y rol `jefe`)

---

## Autenticación (JWT)

Usuarios de ejemplo (hardcodeados en el backend):
- `admin` / `123` → rol `jefe`
- `juan` / `123` → rol `empleado`

### Ejemplo de login
```bash
curl -X POST http://localhost:3000/api/login \
  -H "Content-Type: application/json" \
  -d '{"usuario":"admin","clave":"123"}'
```

El login devuelve un `accessToken` (y `refreshToken`). Para usar endpoints protegidos:

```bash
curl http://localhost:3000/api/usuarios \
  -H "Authorization: Bearer <ACCESS_TOKEN>"
```

---

## Base de datos (SQLite)

El backend usa SQLite y guarda el archivo en:
- `Pymes/BackendDDS/dds-backend/.data/pymes.db`

> La base se inicializa al arrancar el backend (ver `models/inicializarBase.js`).

---

## Tests (Backend)

Dentro del backend:
```bash
cd Pymes/BackendDDS/dds-backend
npm test
```

Incluye tests para:
- Artículos
- Categorías
- Seguridad (login + autorización por rol)

---

## Notas sobre Swagger

El proyecto incluye anotaciones para Swagger en algunas rutas.
Si se encuentra habilitado en el backend, debería exponerse en una ruta tipo `/api-docs` (dependiendo de la configuración).
