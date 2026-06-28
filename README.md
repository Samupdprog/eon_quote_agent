# EON — Orquestador de Presupuestos Index Clima

EON es el orquestador inteligente de Index Clima. Recibe peticiones del usuario en lenguaje natural, interpreta la intención y delega toda la lógica de presupuestos al motor `index_quote_engine`.

**EON no calcula precios. EON no toca JSONs directamente. EON orquesta.**

---

## Qué hace EON v0.1

- Parsea intenciones en español (crear, buscar, resumir, informar, exportar, archivar)
- Detecta IDs de presupuesto tipo `PRE-2026-0001`
- Llama a `index_quote_engine` vía HTTP
- Devuelve resúmenes claros
- Pide los datos que falten antes de actuar

## Qué NO hace todavía

- Lectura real de PDF o audio
- Conexión real a Holded
- Base de datos propia
- Frontend o login
- WhatsApp
- IA generativa (el parser es por reglas)
- Agentes complejos

---

## Conexión a `index_quote_engine`

EON espera que `index_quote_engine` esté corriendo en la URL definida en `.env`.

Endpoints usados:

| Acción | Endpoint |
|--------|----------|
| Herramientas disponibles | `GET /eon/tools` |
| Buscar presupuestos | `GET /eon/search` |
| Obtener presupuesto | `GET /eon/quotes/{id}` |
| Resumen | `GET /eon/quotes/{id}/summary` |
| Calcular | `POST /eon/quotes/{id}/calculate` |
| Workflow completo | `POST /workflow/quote` |
| Informe | `GET /eon/quotes/{id}/report` |
| Informe HTML | `GET /eon/quotes/{id}/report/html` |
| Export Holded | `GET /eon/quotes/{id}/export/holded` |
| Archivar | `POST /eon/quotes/{id}/archive` |

---

## Variables de entorno

Copia `.env.example` a `.env` y ajusta los valores:

```env
QUOTE_ENGINE_API_URL=http://127.0.0.1:8000
EON_DEFAULT_CREATED_BY=EON
EON_DEFAULT_SOURCE=eon
```

---

## Instalación

```bash
pip install -e ".[dev]"
```

---

## CLI

```bash
# Buscar presupuestos
python -m eon.cli "busca presupuestos de Citanias"

# Resumir un presupuesto
python -m eon.cli "resúmeme PRE-2026-0001"

# Crear presupuesto con archivo JSON
python -m eon.cli "hazme un presupuesto para Citanias" --file data/input.json

# Salida JSON
python -m eon.cli "genera informe de PRE-2026-0001" --json
```

---

## Tests

```bash
pytest
```

---

## Próximos pasos

- Parser con LLM (Claude) para intención más robusta
- Lectura de PDFs de proveedores
- Conexión real a Holded con flujo de confirmación humana
- Integración de voz/audio
- Panel de seguimiento de presupuestos abiertos
