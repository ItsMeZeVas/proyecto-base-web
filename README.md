# proyecto-base-web

Plantilla base para proyectos web con **Vue 3** (frontend), **Node.js + Express** (backend) y **PostgreSQL** (base de datos), todo corriendo con **Docker** para que cualquier integrante del equipo tenga el mismo entorno con un solo comando.

## Stack

- **Frontend:** Vue 3 + Vite + Vue Router + Pinia
- **Backend:** Node.js + Express + Prisma (ORM)
- **Base de datos:** PostgreSQL (SQL)
- **Infraestructura:** Docker + Docker Compose

---

## 🚀 Cómo usar esta plantilla para un proyecto nuevo

Si estás clonando esto para empezar un proyecto distinto, cambia estos nombres:

| Archivo | Qué cambiar |
|---|---|
| `package.json` (raíz) | `"name": "nombre-de-tu-proyecto"` |
| `backend/package.json` | `"name": "nombre-de-tu-proyecto-backend"` |
| `frontend/package.json` | `"name": "nombre-de-tu-proyecto-frontend"` |
| `docker-compose.yml` | `POSTGRES_DB: nombre_de_tu_proyecto` (con guion bajo, no medio) |
| `backend/.env` y `backend/.env.example` | Actualizar `DATABASE_URL` con el mismo nombre de base de datos |
| Este `README.md` | Cambiar el título y esta sección |
| Repositorio en GitHub | Renombrar desde **Settings → Repository name** |

Después de cambiar el nombre de la base de datos, reconstruye con `npm run dev:build` para que tome el nuevo nombre.

---

## 📋 Requisitos previos

Cada integrante del equipo necesita instalado:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (con WSL2 si es Windows)
- [Node.js LTS](https://nodejs.org/) (solo para tener `npm`)
- [Git](https://git-scm.com/)
- [GitHub Desktop](https://desktop.github.com/) (opcional, pero recomendado)

## 🛠️ Cómo levantar el proyecto

1. Clonar el repositorio.
2. Copiar el archivo de variables de entorno:
   - Windows (PowerShell): `Copy-Item backend/.env.example backend/.env`
   - Mac/Linux: `cp backend/.env.example backend/.env`
3. **Primera vez** (construye las imágenes, tarda varios minutos):
   ```
   npm run dev:build
   ```
4. **Las siguientes veces:**
   ```
   npm run dev
   ```
5. Abrir:
   - Frontend: http://localhost:5173
   - Backend: http://localhost:3000/health

## 📦 Comandos disponibles

| Comando | Qué hace |
|---|---|
| `npm run dev` | Levanta el proyecto (uso normal) |
| `npm run dev:build` | Levanta reconstruyendo (usar tras instalar dependencias nuevas o cambiar un Dockerfile) |
| `npm run dev:bg` | Levanta en segundo plano |
| `npm run stop` | Apaga todo |
| `npm run logs` | Ver logs en vivo (si se usó `dev:bg`) |
| `npm run reset-db` | ⚠️ Apaga y borra los datos de la base de datos |

## 🗄️ Base de datos y migraciones

Este proyecto usa **PostgreSQL** como base de datos y **Prisma Migrate** para versionar los cambios (las tablas, columnas, relaciones, etc.) de forma ordenada entre todo el equipo:

1. Editar el modelo en `backend/prisma/schema.prisma`.
2. Generar la migración:
   ```
   docker compose exec backend npx prisma migrate dev --name nombre_descriptivo
   ```
3. Esto crea un archivo SQL en `backend/prisma/migrations/` que se sube a Git.
4. El resto del equipo, tras hacer `git pull`, solo corre:
   ```
   docker compose exec backend npx prisma migrate dev
   ```
   y su base de datos queda igual, sin tocar SQL a mano.

## 📁 Estructura del proyecto

```
proyecto-base/
├── backend/          # APdI en Node.js + Express + Prisma
│   ├── src/
│   ├── prisma/
│   └── Dockerfile
├── frontend/          # Vue 3 + Vite
│   ├── src/
│   └── Dockerfile
├── docker-compose.yml
└── package.json       # Atajos de comandos (npm run dev, etc.)
```