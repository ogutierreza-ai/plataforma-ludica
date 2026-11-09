# AGENTS.md

Contexto del proyecto para los agentes de IA. Léalo antes de cualquier cambio.

## Proyecto

Plataforma web lúdica de cursos para niños (Ciencias con Estrellas). FastAPI + Jinja2.
Roles: docente y niño. El niño entra con código de clase y avatar, sin correo.
Datos del proyecto en `app/proyecto.py` (APRENDIZ, CURSO, MECANICA, VERSION).

## Stack

- Python 3.11+, FastAPI, plantillas Jinja2
- SQLModel (SQLite en desarrollo, PostgreSQL al publicar)
- pytest con TestClient, Ruff
- Despliegue en Render

## Comandos

- Instalar: `pip install -r requirements-dev.txt`
- Ejecutar: `uvicorn app.main:app --reload` (abrir http://127.0.0.1:8000)
- Pruebas: `pytest -v` (testpaths = `tests`, pythonpath = `.`)
- Estilo: `ruff check .` (line-length = 100, target py311, selecciona E, F, B, I)
- CI: `ruff check .` -> `pytest -v` -> arrancar app y consultar `/salud`
- Pre-commit: `pre-commit install` activa ruff y pytest antes de cada commit

## Estructura

- `app/main.py`: punto de entrada; monta `/static` e incluye routers `paginas` y `api`
- `app/rutas/`: reciben la petición y llaman a los servicios; sin regla de negocio
- `app/servicios/`: regla de negocio
- `app/modelos/`: tablas SQLModel
- `app/proyecto.py`: datos de identificación (leídos por página, API y `entregar.py`)
- `app/templates/` y `app/static/`: interfaz y PWA (sw.js se sirve desde la raíz)
- `tests/`: pruebas; `tests/aceptacion/`: criterios de aceptación (historia por archivo)
- `specs/`: especificación, plan, flujo de usuario, tareas y ADR
- `prompts/`: prompts versionados; `entregas/` + `entregar.py`: PDF de entrega

## Entorno

- Copie `.env.example` como `.env` y defina `SECRET_KEY` (`python -c "import secrets; print(secrets.token_urlsafe(32))"`)
- `.env` y `*.db` nunca se suben al repositorio (`.gitignore`)
- En Windows active el venv con `.venv\Scripts\activate`

## Convenciones

- Nombres en español, claros y completos.
- Cambios pequeños, una tarea por rama y prueba antes del código.
- No escribir claves ni contraseñas en el código: se leen de variables de entorno.
- No instalar dependencias sin verificar que existen en pypi.org.
- No modificar ni borrar pruebas para que pasen.
- No usar datos personales reales.
- No commitear `.env`, bases de datos locales ni PDFs de `entregas/`.

## Lecciones aprendidas

Cada vez que el agente se equivoque, escriba aquí la regla que evita repetir el error.
