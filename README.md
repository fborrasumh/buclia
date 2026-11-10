# BuclIA

De una pregunta a una decisión con evidencia trazable. Aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/buclia/

**Idiomas:** español (por defecto), inglés, portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

## Qué hace

Guía un bucle de investigación acotado en seis pasos y deja una carpeta con el formato de [Research Loop](https://github.com/wrencanfly/research-loop-skills):

1. **Pregunta**: decisión que depende de la respuesta, alcance, alternativas, estándar de evidencia, regla de parada y presupuesto de iteraciones (3 por defecto).
2. **Fuentes**: material propio (Word, PDF, PowerPoint, texto) y búsquedas en PubMed, OpenAlex y Crossref. Cada fuente lleva su acceso real: completo, parcial (solo resumen) o no disponible.
3. **Afirmaciones**: la IA propone afirmaciones con la cita literal que las apoya o contradice; el código busca cada cita en el texto de su fuente y calcula el estado (apoyada, refutada, disputada, sin resolver).
4. **Prueba con datos** (opcional): plan con regla de decisión fijado antes de ver resultados (hora y huella SHA-256); código Python ejecutado en el navegador con Pyodide 0.26.4, con semilla, versiones de paquetes y huella de los datos registradas. El resultado pasa a ser una fuente citable.
5. **Verificación**: comprobador del registro (traducción a JavaScript de `research_run.py check`), tabla de comprobaciones hechas por el código, revisión crítica con IA y revisión humana.
6. **Cierre**: respuesta, limitaciones, siguiente paso y motivo de parada. Descargas: carpeta del run (ZIP), Word y proyecto JSON.

### Qué comprueba el código (la IA solo propone)

- Cada cita literal se busca en el texto de su fuente; si no aparece, o la fuente no está disponible, no cuenta como evidencia y no se exporta.
- El estado de cada afirmación se calcula a partir de la evidencia verificada; si la IA proponía otro, se indica.
- Las cifras de cada afirmación se buscan en sus fuentes verificadas; las del cierre, en las afirmaciones y fuentes.
- Los DOI se consultan en Crossref y se compara el título.
- Se marcan las fuentes que contienen instrucciones dirigidas a una IA; las fuentes se envían a la IA como datos.
- Cada búsqueda o ejecución exige declarar la iteración y su motivo; ampliar el presupuesto queda registrado.
- Cambiar el plan de la prueba después de ver resultados la convierte en exploratoria y queda registrado.
- La carpeta exportada pasa el comprobador original: `python3 scripts/research_run.py check carpeta`.

## Cómo se usa la IA

Con la propia clave de OpenAI, Google Gemini o Anthropic Claude. La clave se guarda solo en el navegador. No hace falta servidor.

## Privacidad

- **En el navegador** (IndexedDB): el proyecto, las fuentes, los datos y los resultados. La clave de IA, en `localStorage`.
- **Hacia el proveedor de IA** (OpenAI, Gemini o Claude, con la clave de cada persona): la pregunta, el encuadre y el texto de las fuentes, con correos, DNI/NIE y teléfonos enmascarados. De los datos, solo nombres de columnas, tipos y resúmenes; nunca las filas. Antes del primer envío de la sesión se muestra lo que sale y se pide confirmación.
- **Hacia PubMed, OpenAlex y Crossref**: los términos de búsqueda y los DOI que se comprueban.
- **Desde jsDelivr**: Pyodide se descarga la primera vez que se usa el análisis.
- El ZIP incluye el texto de las fuentes propias, pero no el fichero de datos (solo su huella SHA-256).

## Límites

- El comprobador revisa la estructura del registro, no la verdad científica ni la fidelidad de la prosa. Que una cita exista no garantiza que se haya interpretado bien.
- El nivel de acceso a cada fuente lo declara la persona o la búsqueda; el código no puede confirmarlo.
- Una búsqueda informal no es una revisión sistemática. No se detecta si varias fuentes derivan del mismo estudio (solo duplicados por DOI o título).
- La detección de instrucciones incrustadas se basa en patrones y no es exhaustiva.
- La revisión crítica con IA es una autorrevisión asistida, no una revisión independiente.
- El análisis acepta CSV y TSV (hasta 50 MB); Excel debe guardarse antes como CSV. Solo están disponibles los paquetes de Pyodide (numpy, pandas, scipy, statsmodels…) y el código se ejecuta en el hilo principal: un cálculo largo bloquea la página mientras dura.
- La persona debe leer el código antes de ejecutarlo; la app bloquea el uso de red e instalación de paquetes, pero no revisa la lógica estadística.

## Origen de la idea

Basada en [Research Loop](https://github.com/wrencanfly/research-loop-skills) (wrencanfly y colaboradores, licencia MIT), siete instrucciones (*skills*) para que un agente de IA investigue con evidencia trazable, y en su formato de registro `run.json` v1. BuclIA traslada ese método a una aplicación web guiada con IA en la nube; es una adaptación independiente, no un producto de los autores originales. El ejemplo sin clave adapta, en tres idiomas, el escenario ficticio de su demostración de síntesis de evidencia.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche).

Basada en Research Loop (wrencanfly y colaboradores, licencia MIT): siete instrucciones para que un agente de IA investigue con evidencia trazable.

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

## Cómo citar

Borrás Rocher, F. (2026). *BuclIA* (v1.0.0) [Software]. (DOI en trámite)

## Licencia

MIT. Véase [LICENSE](LICENSE).

El comprobador del registro (`compruebaRun` en el código) es una traducción a JavaScript de `scripts/research_run.py` de Research Loop, y el ejemplo adapta su demostración de síntesis de evidencia. Aviso de la obra original: *Copyright (c) 2026 Research Loop contributors*, publicada bajo licencia MIT (https://github.com/wrencanfly/research-loop-skills/blob/main/LICENSE).
