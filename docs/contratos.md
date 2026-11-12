 GET /v1/telemetry/
 # Contrato: Telemetría de misión

- **Productor:** servidor de hábitat (o mock)
- **Consumidores:** HUD (P6), motor de rutas (P3)
- **Endpoint:** `GET /v1/telemetry`
- **Versión:** 1.0 · **Última modificación:** 2026-11-12

## Campos de la respuesta

| Campo | Tipo | Unidad | Rango | Obligatorio | Significado |
|---|---|---|---|---|---|
| `version` | texto | — | — | sí | Versión del contrato |
| `timestampUnixMs` | entero | milisegundos (UTC) | — | sí | Momento de la medición |
| `oxygen.percent` | número | % | 0 a 100 | sí | Oxígeno restante en el tanque |
| `oxygen.minutesLeft` | número | minutos | ≥ 0 | no | Autonomía estimada al consumo actual |
| `radiation.rate_mSv_per_h` | número | mSv/h | ≥ 0 | sí | Tasa de dosis en este momento |
| `radiation.dose_mSv` | número | mSv | ≥ 0 | sí | Dosis acumulada en la misión |
| `weather.temperatureC` | número | °C | -150 a 40 | no | Temperatura exterior |
| `weather.pressurePa` | número | pascales | ≥ 0 | no | Presión atmosférica |
| `weather.windSpeedMs` | número | m/s | ≥ 0 | no | Velocidad del viento |
| `weather.dustStormWarning` | booleano | — | true/false | no | Aviso de tormenta de polvo |

## Convenciones
- Todos los números usan punto decimal.
- Un campo opcional que no se conoce **se omite** (no se envía `0`, porque `0` es un valor válido).
- El tiempo es UTC.

## Errores
| Código | Cuándo | Cuerpo |
|---|---|---|
| 200 | Todo bien | JSON de abajo |
| 503 | La fuente de datos no responde | `{"error": "source_unavailable"}` |

## Ejemplo real
```json
{
  "version": "1.0",
  "timestampUnixMs": 1762905600000,
  "oxygen": { "percent": 80.0, "minutesLeft": 312 },
  "radiation": { "rate_mSv_per_h": 0.025, "dose_mSv": 0.30 },
  "weather": { "temperatureC": -60.0, "pressurePa": 610,
               "windSpeedMs": 8.5, "dustStormWarning": false }
}
```

## Cambios
- 1.0: versión inicial.