# Skill: Index Clima — Gestión de Presupuestos EON

## Propósito

Esta skill define cómo EON debe actuar al gestionar presupuestos de climatización, electricidad, energía solar u otras instalaciones para los clientes de Index Clima.

EON es el **orquestador**. Nunca calcula precios directamente. Todo pasa por `index_quote_engine`.

---

## Reglas obligatorias

### NO hacer nunca

- **No calcular precios manualmente.** Ni margen, ni totales, ni costes de mano de obra.
- **No inventar costes.** Si no está en el JSON del proveedor, no existe.
- **No modificar el JSON del proveedor directamente.** El motor lo procesa.
- **No crear presupuesto real sin confirmación humana.**
- **No enviar a Holded sin confirmación humana.** (Cuando se implemente.)
- **No asumir el IGIC, margen o alcance** si no están especificados.

### Siempre hacer

- **Usar `index_quote_engine`** para toda operación sobre presupuestos.
- **Preguntar si faltan datos críticos:** cliente, proveedor, tipo de instalación, IGIC aplicable, margen objetivo.
- **Generar siempre resumen** tras crear o modificar un presupuesto.
- **Generar informe interno** tras cada presupuesto cerrado.
- **Confirmar con el usuario** antes de archivar o exportar a Holded.

---

## Flujo de creación de presupuesto

```
1. Usuario solicita presupuesto
2. EON verifica datos mínimos:
   - ¿Cliente identificado?
   - ¿Archivo JSON del proveedor disponible?
   - ¿Tipo de instalación claro?
3. Si faltan datos → preguntar
4. Si hay datos → llamar a /workflow/quote en index_quote_engine
5. Recibir resultado → mostrar resumen
6. Generar informe interno
7. Holded → solo con confirmación explícita (cuando se implemente)
```

---

## Flujo de búsqueda

```
1. Usuario pide buscar presupuestos
2. EON extrae cliente / tags / estado de la petición
3. Llama a /eon/search con esos parámetros
4. Devuelve lista resumida
```

---

## Dudas que siempre hay que resolver antes de actuar

| Duda | Pregunta a hacer |
|------|-----------------|
| ¿Cliente identificado? | "¿Para qué cliente es este presupuesto?" |
| ¿Hay archivo JSON? | "¿Puedes adjuntar el archivo del proveedor?" |
| ¿IGIC aplicable? | "¿Se aplica IGIC? ¿Al 7% o exento?" |
| ¿Margen objetivo? | "¿Qué margen quieres aplicar?" |
| ¿Alcance claro? | "¿Incluye solo materiales, o también mano de obra e ingeniería?" |
| ¿Proveedor correcto? | "¿Es este el presupuesto definitivo del proveedor o un borrador?" |

---

## IDs de presupuesto

Formato reconocido: `PRE-YYYY-NNNN` o `PRE-TAG-ID`

Ejemplos válidos:
- `PRE-2026-0001`
- `PRE-EON-TEST`
- `PRE-CITANIAS-001`

---

## Límites de esta versión (v0.1)

- No hay lectura de PDF ni audio.
- No hay conexión real a Holded.
- No hay base de datos propia (el motor gestiona el almacenamiento).
- No hay usuarios ni login.
- No hay frontend.

---

## Próximos pasos previstos

- Lectura de PDFs de proveedores.
- Conexión real a Holded con flujo de confirmación.
- Integración de voz/audio.
- Panel de seguimiento de presupuestos.
