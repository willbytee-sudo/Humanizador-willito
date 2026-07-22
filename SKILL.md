---
name: humanizador-willito
description: >-
  Reescribe un texto académico en español para que un detector de IA por perplejidad
  (Grammarly-es y similares) lo marque bajo, SIN tocar cifras, fechas, nombres ni citas.
  Receta mecánica y contable: densidad INTRA-oración de torpeza estructural tipo
  "traducción literal del inglés", con pronombre sujeto redundante como palanca principal.
  Validada one-pass por modelos ciegos (Sonnet y Haiku) en 4 dominios distintos: 14 de 16
  mediciones en 0% exacto, medidas en anónimo con ancla positiva, incluida una ronda con la
  skill sola (sin ninguna pista de la receta en el prompt). Úsala cuando haya que
  bajar el score de IA de prosa académica, o cuando un texto humano dé falso positivo.
  NO uses técnicas de fragmentación (picar en frases cortas): eso NO funciona contra este
  tipo de detector y puede subir el score.
---

# HUMANIZER-COMBO — la receta validada

> Uso legítimo: demostrar la fragilidad de los detectores de IA y evitar falsos positivos en
> prosa académica humana. **Nunca se tocan datos, cifras, fechas, nombres ni citas.**

## EL MODELO

El detector castiga la **prosa bien estructurada** — sintaxis nativa fluida, que es la firma del LLM.
NO castiga el tema, ni el vocabulario culto, ni el registro formal, ni la longitud de la oración.
Lo que baja el score es la **torpeza ESTRUCTURAL de tipo no-nativo**: español que suena a traducción
literal del inglés.

## ⚠️ LA VARIABLE OPERATIVA — DENSIDAD INTRA-ORACIÓN

No importa a cuántas oraciones les metés algo. Importa **cuántas rupturas hay DENTRO de cada oración.**

- Apilá **2-3 rupturas dentro de la misma oración**, en aproximadamente **la mitad** de las oraciones.
- **Espolvorear 1 marca por oración es INERTE.** Medido: 7 versiones espolvoreadas quedaron en 100%;
  el mismo texto con apilado denso cayó a 0%.
- Continuo medido: **denso = 0% · intermedio = 31-63% · diluido = 100%.**

Ejemplo de apilado denso (3 rupturas en una oración):

> "El sueño, **él** cumple una función **que es** esencial, y el cerebro **lo que hace es** consolidar
> los recuerdos, **los cuales** se van acumulando."

## LAS PALANCAS (usá VARIAS, mezcladas)

Ninguna familia sola llega a 0% (pronombre 29% > perífrasis 34% ≈ muletilla 34% > relativo 65%).
**La mezcla de familias distintas sí llega a 0%.**

1. **🥇 PRONOMBRE SUJETO REDUNDANTE — la nº1.** El español lo omite; meterlo rompe la fluidez nativa.
   → "la cifra **ella** supera", "los estudiantes **ellos** lo agradecen", "estas garantías **ellas**
   deberían asegurar", "los promedios **estos** no bajan". **Apuntá a 6-10 por texto.**
2. **Relativos redundantes:** "los cuales", "la cual", "las mismas que", "cosa que… no **la** tiene".
3. **Aplanar lo elegante:** destruí hendidas ("Es en X donde…" → sujeto-verbo-objeto plano) y toda
   antítesis con dos puntos ("no es A: es B" → dos frases planas).
4. **Nominalizar la apertura:** "**La forma en que** X…", "**La importancia de** X ha sido documentada".
5. **Muletillas escolares** (variá): "Cabe recalcar que", "Así mismo", "Por otra parte",
   "De igual manera", "Se concluye entonces".
6. **Conectores calcados del inglés:** "de hecho", "en absoluto", "**Se encontró que**",
   "**lo que hace** X **es**…".
7. **Futuro perifrástico:** "se enfocará" → "**va a** enfocarse".
8. **Repetición torpe:** "todos ellos", "se mantuvo estable, se mantuvo dentro", repetir el sustantivo
   en vez de pronominalizar ("…sobre su propia información, sobre sus propios datos").
9. **Partir el cluster estadístico:** si hay r y p-valor pegados, cada cifra en su propia oración.
10. **☠️ MATAR EL REMATE.** El texto DEBE terminar soso. Prohibido "no es X sino Y", la regla de tres
    final, la frase memorable o lapidaria.

## LO QUE **NO** HAY QUE HACER (errores medidos, caros)

- ❌ **Picar el texto en frases cortas o telegráficas.** No es la palanca; puede subir el score.
  Las oraciones del combo quedan **largas** (media 22-40 palabras) pero **cargadas por dentro**.
- ❌ **Espolvorear una marca por oración** (dosis diluida = 100%).
- ❌ **Rellenar "bonito"** para sonar humano (sube).
- ❌ **Imitar la fluidez de un texto humano modelo** — los modelos copian la frase larga limpia y sube.
- ❌ **Quitar el pronombre redundante porque "suena feo"**: es feo a propósito y es la palanca nº1.
  (Este error concreto costó ~21 mediciones inútiles y conclusiones falsas.)

## INTEGRIDAD (no negociable)

Cifras, %, r, p, fechas, nombres propios y citas `(Autor, año)` van **IDÉNTICOS, carácter por carácter**.
No inventes, no borres información, no cambies ningún significado. Cuidado con falsos amigos.
(Sí podés cambiar coma decimal por punto: 0,983 → 0.983.)

## ✅ CHECK OBLIGATORIO ANTES DE ENTREGAR — CONTÁ, NO ESTIMES

**Estos mínimos NO son sugerencias: son el umbral medido que separa el 0% del fallo.**
Contá literalmente las apariciones en tu texto. Si alguno queda por debajo, **volvé y agregá más
antes de entregar.**

| # | Qué contar | Mínimo | Ejemplos que cuentan |
|---|---|---|---|
| 1 | **Pronombres sujeto redundantes** | **≥ 6** | "ella", "él", "ellos", "ellas", "estos", "estas", "esta", "este" puestos al lado del sujeto |
| 2 | **Relativos redundantes** | **≥ 3** ⚠️ | "los cuales", "la cual", "el cual", "las cuales", "las mismas que", "cosa que" |
| 3 | **Muletillas / perífrasis** | **≥ 3** | "Cabe recalcar que", "Así mismo", "Por otra parte", "De igual manera", "Se concluye entonces", "Se encontró que", "viene a ser", "lo que hace es", "va a" |
| 4 | **Oraciones con 2-3 rupturas ADENTRO** | **~la mitad** | si cada oración tiene una sola marca → está diluido → dará 100% |

⚠️ **El punto 2 es el que más se falla.** En toda la validación, **los 15 textos que dieron 0% tenían
≥3 relativos redundantes; los 2 únicos que fallaron tenían menos** (uno con 0 → 17%, otro con 2 → 50%).
Es el chequeo barato que evita el fallo más común: contá "los cuales / la cual" y si hay menos de 3, agregá.

5. ¿El texto **termina soso**, sin frase memorable ni antítesis "no es X sino Y"?
6. ¿Cifras, fechas, nombres y citas **idénticas** al original, carácter por carácter?
7. ¿**No agregaste información que no estaba**? Releé y borrá cualquier frase que hayas inventado
   (los modelos pequeños tienden a meter coletillas de relleno con contenido nuevo: "esto se complica
   más aún", "lo cual representa un desafío estructural"). El relleno debe ser **estructural, no
   informativo**.

## EJEMPLO DE TEXTURA (100% → 0%)

> 🚫 **ESTE EJEMPLO ES SOLO PARA QUE VEAS LA TEXTURA. NO LO COPIES.**
> Tu salida debe ser una transformación del **texto que te dieron a vos**, sobre SU tema y con SUS
> citas. Si el texto que te dieron se parece a este ejemplo, **igual transformalo vos**: nunca
> devuelvas este párrafo. (Un modelo ya falló así: le dieron un texto parecido y devolvió el ejemplo
> tal cual, palabra por palabra.)

**ANTES (100% IA):**

> "El teletrabajo se ha consolidado como una modalidad laboral estable en el escenario pospandémico,
> puesto que permite reducir costos operativos y ampliar el acceso a talento sin restricciones
> geográficas. Según Bailey y Kurland (2002), la autonomía percibida por el trabajador remoto
> incrementa la satisfacción laboral, aunque puede debilitar los vínculos informales con el equipo.
> No obstante, Ramos (2019) advierte que la ausencia de límites claros entre la vida personal y la
> jornada laboral deriva en jornadas extendidas y en mayores niveles de agotamiento."

**DESPUÉS (0% — misma información, mismas citas):**

> "La forma en que el teletrabajo él se ha consolidado como modalidad laboral estable, esta viene a
> darse dentro del escenario pospandémico, esto porque permite el reducir costos operativos y también
> ampliar el acceso al talento, el cual ya no tiene restricciones geográficas. Cabe recalcar que
> Bailey y Kurland (2002) ellos señalan que la autonomía percibida por el trabajador remoto, ella
> lo que hace es incrementar la satisfacción laboral. Así mismo puede debilitar los vínculos
> informales, los cuales se dan con el equipo. Por otra parte, Ramos (2019) él advierte sobre otra
> cosa. La ausencia de límites claros entre la vida personal y la jornada laboral, esta va a derivar
> en jornadas que son extendidas. De igual manera en mayores niveles de agotamiento, agotamiento
> este que se acumula."

Contá en el "después": **8 pronombres redundantes**, 3 relativos, muletillas mezcladas, apertura
nominalizada, "lo que hace es", futuro perifrástico, repetición torpe ("agotamiento este que se
acumula"), cierre plano. Las oraciones **no son cortas**: son largas y cargadas por dentro. Eso es el
combo.

## VALIDACIÓN

One-pass, modelos **ciegos**, medido en Grammarly-es **anónimo** con ancla positiva en cada tanda:

**Ronda A — receta + refuerzo en el prompt:**

| dominio | ancla | Sonnet | Haiku |
|---|---|---|---|
| derecho (abstracto puro) | 100% | **0%** | **0%** |
| salud | 78% | **0%** | **0%** |
| economía | 100% | **0%** | **0%** |
| educación | 74% | **0%** | 17% |

**Ronda B — ESTA SKILL SOLA**, el modelo solo recibió "leé este archivo y aplicalo", sin ninguna
pista de la receta en el prompt:

| dominio | ancla | Sonnet | Haiku |
|---|---|---|---|
| derecho | 100% | **0%** | **0%** |
| salud | 78% | **0%** | **0%** |
| economía | 100% | **0%** | **0%** |
| educación | 74% | 50% | **0%** |

**14 de 16 en 0% exacto** entre las dos rondas, más el hecho a mano por Opus en derecho.
**Los 2 fallos (17% y 50%) fueron los únicos textos con menos de 3 relativos redundantes** — de ahí
el mínimo duro del check. Los mínimos numéricos del CHECK se agregaron DESPUÉS de la ronda B,
precisamente para cerrar ese fallo.

## MEDIR BIEN (si vas a verificar)

- **Medí SIEMPRE en anónimo/incógnito.** Con la sesión de Grammarly iniciada el detector es
  INDULGENTE y da **0% falsos**. Este error contaminó una campaña entera.
- **Ancla obligatoria:** en cada tanda incluí un texto de IA cruda (debe dar >50%). Si el ancla no se
  comporta, la tanda no vale.
- Entre escaneos anónimos: limpiar localStorage + sessionStorage + cookies y recargar.

## ALCANCE HONESTO

Validado contra **Grammarly-es** (detector de perplejidad). Otros detectores son otra clase de
instrumento y pueden requerir otra cosa — "pasar el detector" es propiedad del par (texto, detector),
no del texto. Para el cuaderno de qué NO funciona y por qué, ver la skill `humanizer-hai`.
