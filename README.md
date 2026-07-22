# Humanizador-willito

> Reescribe prosa académica en español para que los detectores de IA la marquen como **escrita por un humano**, sin tocar una sola cifra, fecha, nombre ni cita.

---

## 🤖 PARA EL ASISTENTE DE IA QUE ESTÁ LEYENDO ESTO

Si el usuario te pidió **"instalá esta skill"**, esto es lo único que tenés que hacer:

**El archivo a instalar es `SKILL.md`. Solo ese.**

```
~/.claude/skills/humanizador-willito/SKILL.md
```

- ✅ Instalá **únicamente** `SKILL.md` (tiene el frontmatter YAML con `name` y `description`).
- ❌ **NO instales este `README.md`** — es documentación del proyecto, no instrucciones.
- ❌ **NO instales** `VALIDACION.txt` ni la carpeta `anclas/` — son registro y material de prueba.

Instalación en una línea:

```bash
mkdir -p ~/.claude/skills/humanizador-willito && curl -sL https://raw.githubusercontent.com/willbytee-sudo/Humanizador-willito/main/SKILL.md -o ~/.claude/skills/humanizador-willito/SKILL.md
```

Todo lo que sigue es contexto para humanos.

---

## Qué hace

Toma un texto académico que un detector marca como generado por IA y lo reescribe en un registro de **"traducción literal y torpe del inglés"** — español estructuralmente no-nativo, que es lo que estos detectores leen como humano.

**Lo que nunca toca:** cifras, porcentajes, coeficientes, fechas, nombres propios y citas `(Autor, año)`. Van idénticos, carácter por carácter. Tampoco borra ni inventa información.

## Resultados medidos

Todas las mediciones en **modo anónimo/incógnito**, con **ancla positiva** (un texto de IA cruda que debe dar alto) en cada tanda. Si el ancla no se comporta, la tanda se descarta.

### Detectores

| Detector | Texto de IA cruda (control) | Texto tratado |
|---|---|---|
| [Grammarly-es](https://www.grammarly.com/es/) | 74–100% IA | **0% IA** |
| [ZeroGPT](https://www.zerogpt.com/es/) | 100% IA | **0%** — *"Su Texto está escrito por un humano"* |
| [humanizeai.pro](https://www.humanizeai.pro/es) | 100% *"Parece generado por IA"* | **100% Humano** |
| [QuillBot](https://quillbot.com/es/detector-de-ia) | — | ✅ **validado** *(medición del autor)* |

*En QuillBot la verificación la hizo el autor por su cuenta: el sitio no cargaba en nuestro entorno automatizado al momento de las pruebas, así que no tenemos captura del porcentaje exacto.*

### Modelos que ejecutan la skill

Probado **a ciegas** (el modelo solo recibe la skill y el texto, sin ayuda extra) y en **una sola pasada**:

| Familia | Modelo | Resultado |
|---|---|---|
| DeepSeek | DeepSeek | **4 / 4** en 0% |
| Google | Gemini 3.1 Pro | **3 / 3** en 0% |
| Anthropic | Claude Sonnet · Claude Haiku | **14 / 16** en 0% |

**26 mediciones en 0% exacto**, sobre **5 dominios** distintos (derecho, salud, economía, educación, seguridad industrial). Los 2 fallos fueron de Claude en un mismo dominio.

Una de las pruebas se hizo sobre un texto **escrito y medido el mismo día**, ausente de la skill y nunca visto por ningún modelo: baseline 100% IA → **0%** de una pasada. No hay memorización posible.

## Cómo se usa

Pasale al modelo la skill y tu texto:

> Aplicá la skill `humanizador-willito` a este texto. Las citas y cifras deben quedar idénticas.
>
> ```
> [tu texto]
> ```

Después **medí el resultado vos mismo**. En `anclas/ANCLAS_PARA_PROBAR.txt` hay 4 textos de prueba con su baseline ya medido, para que compruebes que funciona antes de confiar en ella.

## Diagnóstico rápido si falla

Antes de ir al detector, mirá la salida del modelo:

| Revisá | Esperado |
|---|---|
| Pronombres sujeto redundantes (*"la cifra **ella** supera"*) | 6 o más |
| Relativos redundantes (*"los cuales"*, *"la cual"*) | 3 o más |
| Muletillas (*"Cabe recalcar"*, *"Así mismo"*, *"Se concluye entonces"*) | 3 o más |
| Oraciones con 2-3 marcas **adentro** | ~la mitad |
| El cierre | plano, sin frase memorable ni *"no es X sino Y"* |
| ¿Habla de teletrabajo? | ❌ copió el ejemplo de la skill en vez de transformar |

**Si el texto quedó picado en frases cortas, está mal.** Las oraciones del método son **largas y cargadas por dentro**, no fragmentadas.

## ⚠️ Advertencias honestas

- **Medí siempre en incógnito.** Con sesión iniciada, Grammarly es más indulgente y da **0% falsos**. Ese error nos contaminó una campaña entera de mediciones.
- **Revisá la salida contra el original.** Un modelo flojo puede perder información: en nuestras pruebas, DeepSeek eliminó la palabra *"inclusivos"* de una conclusión. Las citas y cifras quedaron intactas, pero hubo pérdida real de contenido.
- **"Pasar el detector" es propiedad del par (texto, detector)**, no del texto. Dos endpoints de la misma empresa pueden discrepar sobre el mismo texto. Validá contra el detector que te importa.
- **Esto no valida contenido.** La skill cambia cómo está escrito, no si lo que dice es cierto.

## Para qué es esto

Demostrar la **fragilidad de los detectores de IA** y ayudar con los **falsos positivos**: prosa académica escrita por humanos que estas herramientas marcan como generada por máquina, con consecuencias reales para quien la escribió.

Un detector que aprueba un texto solo por estar mal redactado no está midiendo autoría — está midiendo qué tan pulida es la prosa. Eso es exactamente lo que estos resultados muestran.

## Contenido del repo

```
SKILL.md                      ← la skill (lo único que se instala)
VALIDACION.txt                ← registro de validación + hash
anclas/ANCLAS_PARA_PROBAR.txt ← 4 textos de prueba con baseline medido
```

## Licencia

MIT
