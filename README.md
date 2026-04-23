# Sofka Insurance Quoter

Repositorio orquestador del cotizador de seguros de daños. Agrupa la configuración de integración (`docker-compose.yml`) y la documentación compartida del sistema. Cada servicio vive en su propio repositorio independiente.

## Repositorios

| Servicio | Repositorio | Puerto |
|----------|-------------|--------|
| Frontend (Angular 19) | https://github.com/Jomruizgo/Insurance-Quoter-Front | 4200 |
| Backend quoter | https://github.com/Jomruizgo/Insurance-Quoter-Back | 8080 |
| Backend core / catálogos | https://github.com/Jomruizgo/Insurance-Quoter-Core | 8081 |

## Levantar el stack completo

### Requisitos previos

- Docker y Docker Compose
- Clonar los tres repos dentro de este directorio:

```bash
git clone https://github.com/Jomruizgo/Insurance-Quoter-Back.git
git clone https://github.com/Jomruizgo/Insurance-Quoter-Core.git
git clone https://github.com/Jomruizgo/Insurance-Quoter-Front.git
```

### Variables de entorno

1. Copiar el `.env.example` raíz y completar con las credenciales de cada backend:

```bash
cp .env.example .env
```

2. En `Insurance-Quoter-Back/`, copiar `.env.example` → `.env` y rellenar las credenciales.
3. En `Insurance-Quoter-Core/`, copiar `.env.example` → `.env` y rellenar las credenciales.

### Iniciar

```bash
docker compose up -d
```

El frontend queda disponible en `http://localhost:4200`.

### Detener

```bash
docker compose down
```

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
