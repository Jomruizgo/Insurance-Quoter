# Sofka Insurance Quoter

Repositorio orquestador del cotizador de seguros de daños. Contiene la configuración de integración (`docker-compose.yml`), la documentación compartida del sistema y los tres servicios como submódulos de git.

## Repositorios

| Servicio | Repositorio | Puerto |
|----------|-------------|--------|
| Frontend (Angular 19) | https://github.com/Jomruizgo/Insurance-Quoter-Front | 4200 |
| Backend quoter | https://github.com/Jomruizgo/Insurance-Quoter-Back | 8080 |
| Backend core / catálogos | https://github.com/Jomruizgo/Insurance-Quoter-Core | 8081 |

## Levantar el stack completo

### 1. Clonar el repositorio

Los tres servicios son submódulos de git. Con un solo comando se clona este repo y todos sus submódulos:

```bash
git clone --recurse-submodules https://github.com/Jomruizgo/Insurance-Quoter.git
cd Insurance-Quoter
```

> Si ya clonaste el repo sin `--recurse-submodules`, ejecuta:
> ```bash
> git submodule update --init --recursive
> ```

### 2. Variables de entorno

Copiar y completar las variables de cada servicio:

```bash
cp Insurance-Quoter-Back/.env.example  Insurance-Quoter-Back/.env
cp Insurance-Quoter-Core/.env.example  Insurance-Quoter-Core/.env
```

### 3. Iniciar

```bash
docker compose up -d
```

El frontend queda disponible en `http://localhost:4200`.

### Detener

```bash
docker compose down
```

---

## Mantener los submódulos actualizados

Para traer los últimos cambios de los tres servicios:

```bash
git submodule update --remote --merge
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
