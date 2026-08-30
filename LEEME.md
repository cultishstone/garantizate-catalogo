# Catálogo de ventanas de cobertura de Garantízate

Un solo fichero: [`catalogo.json`](catalogo.json). Lo lee la app
**Garantízate** para saber cuánto tiempo tiene alguien para contratar una
cobertura ampliada —AppleCare+, DJI Care Refresh y las que se vayan
comprobando— y avisar antes de que se cierre el plazo.

Vive en un repositorio aparte, y público, por un motivo concreto: **el de la app
es privado, y un fichero en un repo privado exigiría meter un token dentro del
binario.** Eso no se hace.

## Qué lleva y qué no

Solo ventanas **comprobadas en la web del propio fabricante**. Nada aquí es
información que no esté ya publicada por ellos.

**Una ventana equivocada es peor que ninguna**: hace perder el plazo justo a
quien se fió. Ante la duda, no entra.

## El formato

```json
{
  "id": "apple-applecare",
  "marca": "Apple",
  "nombre": "AppleCare+",
  "ventana": { "dias": 60 },
  "antelacion": { "dias": 15 },
  "desdeLaActivacion": false,
  "nota": "60 días desde la compra para contratarlo."
}
```

| | |
|---|---|
| `id` | No cambia nunca. Es lo que ata una ventana a los artículos ya guardados |
| `ventana` | `dias`, `horas` o `meses`. **Horas no es un capricho**: DJI da 48 |
| `antelacion` | Cuánto antes se avisa. **Es un dato, no una fórmula**: «un tercio» daría 20 días en una ventana de 60 y 16 horas en una de 48, inútil en los dos casos |
| `desdeLaActivacion` | `true` cuando la marca cuenta desde que se activa el aparato y no desde la compra. La app entonces **pregunta** esa fecha, porque no puede saberla |
| `nota` | Una línea que se le enseña a quien elige esa cobertura |

**`version` sube cada vez que se cambia algo.** La app ignora un fichero con
`version` menor que la del catálogo que lleva dentro: así una caché rancia o un
fichero mal subido no pueden dejarla peor de lo que ya venía.

## Cómo se cambia

Se edita el JSON, se **sube `version`** y se sube. **No hay que publicar una
versión de la app**: eso es todo el motivo de que este repositorio exista.

Cuánto tarda en llegar, medido el 31-08-2026:

| | |
|---|---|
| `raw.githubusercontent.com` sirve lo nuevo | **hasta 5 minutos** (`cache-control: max-age=300`), y **cada nodo caduca por su cuenta**: un rato después de que `curl` devuelva lo nuevo, todavía hay quien recibe lo viejo |
| La app vuelve a preguntar | **una vez por semana** como mucho |
| Un aparato sin red | se queda con la última copia que pudo bajar, indefinidamente |

O sea: **esto no sirve para urgencias.** Un cambio tarda días en llegar a todo
el mundo, y así está pensado — el catálogo cambia dos veces al año, y preguntar
más a menudo sería gastar batería ajena para nada.

Y si alguien tiene una marca que no está aquí, **puede escribirla él en la app**.
Lo que escriba gana siempre, incluso a lo que diga este fichero.
