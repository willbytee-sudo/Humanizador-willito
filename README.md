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
| [QuillBot](https://quillbot.com/es/detector-de-ia) | 100% IA | **0% IA — 100% humano** |
| [Originality.ai](https://originality.ai/) | 100% IA | **95–100% Confident That's Original** *(95–100% humano)* |

### Modelos que ejecutan la skill

Probado **a ciegas** (el modelo solo recibe la skill y el texto, sin ayuda extra) y en **una sola pasada**:

| Familia | Modelo | Resultado |
|---|---|---|
| DeepSeek | DeepSeek | **4 / 4** en 0% |
| Google | Gemini 3.1 Pro | **3 / 3** en 0% |
| Anthropic | Claude Sonnet · Claude Haiku | **14 / 16** en 0% |

**26 mediciones en 0% exacto**, sobre **5 dominios** distintos (derecho, salud, economía, educación, seguridad industrial). Los 2 fallos fueron de Claude en un mismo dominio.

Una de las pruebas se hizo sobre un texto **escrito y medido el mismo día**, ausente de la skill y nunca visto por ningún modelo: baseline 100% IA → **0%** de una pasada. No hay memorización posible.

## Cómo se construyó

No salió de una idea feliz. Salió de **matar hipótesis, una tras otra, hasta que quedó la que aguantó**.

### Siete días, nueve fases

Esto no fue una sesión. Fue una investigación de una semana, con cuaderno de laboratorio, hipótesis pre-registradas y cientos de mediciones. Cada fase **mató** algo.

| # | Fase | Escala | Qué costó |
|---|---|---|---|
| 1 | **Primer experimento** contra un detector comercial | 16 mediciones, 14 sondas | Murieron **6 mecanismos** (perplejidad, fluidez, rugosidad, largo de frase, palabras función, densidad de datos). Descubrimiento incómodo: **las explicaciones del detector se inventan los valores** del texto que tienen delante. |
| 2 | **Modelado con acceso autónomo** al navegador | ~56 mediciones | Cayeron **8 modelos explicativos** más. Apareció el primer par mínimo brutal: un párrafo enciclopédico al 99% → **0% cambiando una coma por un punto**. |
| 3 | **Dos clases de detector** | comparativas en 3 sitios | Se descubrió que los detectores **de forma** y los **de perplejidad** son bestias distintas: el truco que vence a uno **sube** el score del otro. |
| 4 | **Prueba de frontera** | 25 reformulaciones del mismo texto | Ninguna bajó de 39%. Declaré que había una **frontera infranqueable**. |
| 5 | **La frontera era falsa** | par mínimo decisivo | **El usuario la refutó con un texto suyo: 0%.** La variable que yo nunca había movido era la *corrección gramatical* — porque un modelo que escribe bien no descubre solo que escribir bien es el problema. |
| 6 | **Los dos Grammarly** | **72 mediciones** (36 textos × 2 endpoints) | Acuerdo entre endpoints: **58%**, correlación **r = 0,21**. Conclusión: *"pasar el detector"* no es propiedad del texto, sino del par **(texto, detector)**. |
| 7 | **Campaña de frontera** | ~67 escaneos | Se aisló la variable real: **densidad intra-oración**. Espolvorear una marca por oración = inerte. Apilarlas dentro = 0%. |
| 8 | **La contaminación** | re-verificación completa | Se descubrió que **con la sesión iniciada el detector regala 0% falsos**. Invalidó mediciones previas y obligó al protocolo de anclas. |
| 9 | **Validación final** *(este repo)* | **96 escaneos · 83 agentes · 5,12 M tokens** | **10 hipótesis falsadas**, incluida mi propia explicación del método. Sobrevivió **1**. |

**Total: más de 400 mediciones documentadas**, 175 archivos de datos, 44 textos de sonda y un cuaderno de laboratorio de **2.216 líneas** donde está registrado cada callejón sin salida.

### El costo real

Cifras recuperadas de los registros de uso de las **17 sesiones de trabajo** (16–22 de julio):

| | |
|---|---|
| Sesiones de trabajo | **17** |
| Mensajes con uso registrado | **15.515** |
| **Tokens generados** | **16,8 millones** |
| Contexto escrito en caché | 178,7 millones |
| Contexto releído | 4.490 millones |
| **Total procesado** | **4,69 mil millones de tokens** |

Los 16,8 millones de tokens generados equivalen a unos **12,6 millones de palabras** — alrededor de **140 libros** de extensión media. Escritos, medidos y en su enorme mayoría **descartados**, porque casi todo lo que se probó no funcionó.

Ese es el precio real de las **1.400 palabras** que quedaron en `SKILL.md`.

### Los métodos

**No usamos estadística inferencial.** No hay regresiones, ni p-valores, ni intervalos de confianza — y sería deshonesto pretenderlo. Lo que usamos fue **método experimental clásico**, que para este problema resultó más filoso:

- **Pares mínimos.** Dos versiones del mismo texto que difieren en **una sola variable**. Es lo que aisló la palanca real: *"solo traducción-ese" = 62%* vs *"traducción-ese + densidad" = 0%*, mismo contenido.
- **Controles positivos obligatorios.** Cada tanda incluyó un texto de IA cruda que **debía** dar alto. Si el ancla no se comportaba, la tanda entera se descartaba. Los 96 escaneos están anclados.
- **Pruebas a ciegas.** Los modelos recibían la skill y el texto, sin contexto, sin pistas, sin saber qué se esperaba de ellos.
- **Validación cruzada entre familias.** No basta que funcione en el modelo que la escribió. Se probó en Anthropic, Google y DeepSeek.
- **Pre-registro de predicciones.** Escribir qué se espera **antes** de medir. Fue lo que mató la última hipótesis: predijimos 4 resultados y fallamos 3.
- **Conteo lingüístico.** Cuantificación de marcadores por familia gramatical (pronombre sujeto redundante, relativos, muletillas, perífrasis) y su densidad **dentro** de cada oración.

### Lo que se descubrió por el camino

**El detector mentía según la sesión.** Con la cuenta iniciada, Grammarly era notablemente más indulgente y devolvía **0% falsos**. Ese hallazgo invalidó una campaña entera de mediciones previas y obligó a rehacerlas en anónimo. Desde entonces, ninguna medición vale sin su ancla.

**La receta correcta era casi lo opuesto a lo intuitivo.** Durante buena parte del trabajo se probó lo que "suena" a humanizar: frases cortas, tono hablado, imitar textos humanos reales, encarnar a un estudiante cansado. **Todo eso falló** — algunos incluso empeoraron el score. Lo que funciona es exactamente lo contrario: **oraciones largas, cargadas por dentro de torpeza estructural**.

**Y lo más incómodo:** la explicación de *por qué* funciona el método también murió. Se identificó un umbral que predecía perfectamente 24 observaciones pasadas — y al usarlo para predecir de verdad, **falló 3 de 4 veces**. El método sigue funcionando; la teoría con que lo explicábamos, no.

> Esa distinción es el corazón de este proyecto: **una instrucción puede funcionar aunque la teoría que la justifica sea falsa.** La skill se dejó congelada, con hash verificable, precisamente para no romper lo que funciona intentando arreglar una explicación equivocada.

### El cementerio de hipótesis

Las **10** que murieron solo en la fase final. Cada una parecía razonable, cada una se midió, cada una se cayó:

| Hipótesis | Cómo murió |
|---|---|
| Un documento de **reglas** transfiere a otros modelos | 75% y 71% |
| Encapsular mi **proceso mental** transfiere | disperso, 25–100% |
| Una **segunda pasada** de auto-auditoría lo resuelve | funcionó en 1 texto, falló en 3 |
| Darle **ejemplos humanos reales** (few-shot) lo rescata | **empeoró** — de 63% subió a 100% |
| Transferir el **discriminador** + autopuntuación | inconsistente |
| **Encarnar la persona** en vez de aplicar reglas | empeoró |
| El contenido **abstracto** resiste todo, piso 24% | cayó a **0%** con la receta correcta |
| El **modelo tonto gana** porque escribe peor | artefacto: con la receta buena ganan todos |
| **Más instrucciones empeoran** el resultado | eran instrucciones *equivocadas*, no exceso |
| El umbral **"relativos ≥ 3"** predice el fallo | predijo 4, **falló 3** |

**La lección que más caro costó:** durante buena parte del trabajo reemplacé una receta ya validada por teorías propias — llegué a **prohibir explícitamente la palanca más potente** porque no encajaba con mi explicación. Veintiún mediciones después, una sola pregunta del usuario —*"¿y no probaste el método que ya funcionaba?"*— destapó el error. La receta correcta produjo 0% al primer intento en el texto que había resistido todo lo demás.

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
