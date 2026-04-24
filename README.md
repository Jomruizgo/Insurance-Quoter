# Sofka Insurance Quoter

Repositorio orquestador del cotizador de seguros de daños. Contiene la configuración de integración (`docker-compose.yml`), los Dockerfiles de cada servicio y la documentación compartida del sistema.

Cada servicio vive en su propio repositorio independiente y se clona automáticamente durante el build de Docker — no es necesario clonarlos manualmente.

## Repositorios

| Servicio | Repositorio | Puerto |
|----------|-------------|--------|
| Frontend (Angular 19) | https://github.com/Jomruizgo/Insurance-Quoter-Front | 4200 |
| Backend quoter | https://github.com/Jomruizgo/Insurance-Quoter-Back | 8080 |
| Backend core / catálogos | https://github.com/Jomruizgo/Insurance-Quoter-Core | 8081 |
| Pruebas automatizadas UI (Serenity BDD) | https://github.com/Jomruizgo/Insurance_Quoter_Auto_Front_Screenplay | — |

## Levantar el stack completo

### Requisitos previos

- Docker y Docker Compose
- Acceso a internet (Docker clona los repos durante el build)

### 1. Clonar este repositorio

```bash
git clone https://github.com/Jomruizgo/Insurance-Quoter.git
cd Insurance-Quoter
```

### 2. Variables de entorno

```bash
cp .env.example .env
```

Editar `.env` y reemplazar los valores `change_me` con credenciales reales para las bases de datos.

### 3. Iniciar

```bash
docker compose up -d
```

Docker construye las imágenes clonando cada repo desde GitHub, compila el código y levanta todos los contenedores. El frontend queda disponible en `http://localhost:4200`.

> En el primer `up` la construcción toma varios minutos (clona + compila 2 backends Java y 1 frontend Angular). Las ejecuciones siguientes usan el caché de Docker y son inmediatas.

### Detener

```bash
docker compose down
```

### Reconstruir con el código más reciente

```bash
docker compose build --no-cache
docker compose up -d
```

---

## Arquitectura

```
┌─────────────────────────────────────────────┐
│  nginx :4200                                │
│  ├── /api/*       → backend :8080           │
│  └── /api-core/*  → core :8081             │
└─────────────────────────────────────────────┘
         │                    │
┌────────────────┐   ┌────────────────┐
│ backend :8080  │──▶│  core :8081    │
│ insurance_     │   │ insurance_     │
│ quoter_db :5432│   │ core_db :5433  │
└────────────────┘   └────────────────┘
```

## Documentación

| Documento | Descripción |
|-----------|-------------|
| [docs/api-contracts.md](docs/api-contracts.md) | Contratos HTTP front↔back y back→core |
| [docs/Reto.md](docs/Reto.md) | Especificación del reto técnico |
