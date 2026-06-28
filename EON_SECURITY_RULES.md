# EON_SECURITY_RULES.md — Reglas de seguridad v0.2

## Facturas: PROHIBIDO

EON no puede crear, convertir, modificar, enviar, eliminar, descargar ni automatizar facturas. Ningún modelo, herramienta ni flujo de EON debe tocar facturas.

## Secretos: PROHIBIDO mostrar

- Nunca mostrar claves API, tokens, contraseñas ni secretos en respuestas.
- Si se detectan secretos en archivos de configuración, avisar al usuario sin mostrar el contenido.
- No incluir secretos en logs, resúmenes ni borradores.

## Holded: REQUIERE aprobación humana

- No crear, modificar ni eliminar recursos reales en Holded sin aprobación humana explícita.
- Preparar borradores y payloads es permitido; ejecutar la sincronización no.
- Si `can_sync_holded` es `false` o faltan IDs reales validados, la salida debe quedar como borrador.

## Datos: NO inventar

EON no debe inventar ni asumir:

- CIF/NIF
- Emails
- Direcciones
- Precios o cantidades
- Impuestos o porcentajes de IGIC
- IDs de Holded (series, métodos de pago, impuestos, contactos)
- Descuentos
- Métodos de pago

Si un dato no está disponible: marcarlo como `PENDIENTE` o preguntar.

## Clientes: NO duplicar

- Antes de crear un cliente nuevo, buscar posibles duplicados.
- Si hay coincidencia parcial, avisar al usuario y pedir confirmación.
- No crear clientes automáticamente sin validación.

## Cálculos: NO manuales

- Todo cálculo de presupuestos (totales, subtotales, márgenes, impuestos) debe pasar por `index_quote_engine`.
- EON orquesta; el motor calcula.
- Si el motor no está disponible, informar al usuario. No calcular a mano como alternativa.

## Errores HTTP de Holded

| Código | Acción |
|--------|--------|
| 401 | Parar. Posible problema de autenticación. Informar al usuario. |
| 403 | Parar. Permiso denegado. Informar al usuario. |
| 422 | Parar. Datos inválidos en el payload. Revisar campos y explicar. |
| 500 | Parar. Error del servidor de Holded. Dejar en revisión. |

En cualquier error HTTP de Holded: no reintentar automáticamente. Explicar el error al usuario y dejar la operación en estado de revisión.

## Documentos de proveedor

- Si un documento de proveedor no está claro, no asumir valores.
- Marcar como pendiente de revisión y pedir al usuario que confirme.

## Vault de Obsidian

- EON lee el vault como fuente de conocimiento.
- EON no modifica, mueve, copia ni borra notas del vault salvo petición explícita de Samuel.

## Modelos y routing

- Qwen/local: solo tareas simples, internas y reversibles.
- Qwen/local no debe decidir importes, configuración de Holded ni datos críticos.
- Claude/OpenAI pueden razonar, pero las operaciones deben pasar por herramientas controladas.
