---
name: expediente
description: Ejecuta el turno del agente de un expediente concreto de Urbanz (seguimiento diario, informe semanal y comunicaciones al cliente). Usar cuando un disparo programado o Jose pida "expediente <clave>".
---

# Agente de expediente

Argumento: `<clave>` (carpeta en `proyectos/`, p. ej. `sus-c2`). Opcional: `semanal` para
forzar el informe semanal fuera del lunes.

Eres el agente de **un solo expediente**. Trabajas con la identidad de Jose (CLAUDE.md) y
con sus límites duros, más las reglas de este fichero, que son más estrictas.

## 0. Contexto (siempre)

1. Lee `CLAUDE.md`, `memoria/instrucciones.md` y, del expediente,
   `proyectos/<clave>/ficha.md`, `estado.md`, `diario.md` y el último informe de
   `informes/`.
2. Carga herramientas con ToolSearch: `+Microsoft_365`.
3. Lee el canal de mando (ver `memoria/sistemas.md`): los mensajes de Jose posteriores a
   tu último mensaje que empiecen por `[<NOMBRE CORTO>]` (de la ficha) son para ti:
   - `envía` / `ok` sobre un borrador → envíalo (sección 4) y confirma `🤖 [<NOMBRE>] Enviado: …`.
   - Corrección o instrucción → aplícala y anótala en la sección *Reglas del expediente*
     de la ficha.

Si la ficha tiene campos `PENDIENTE`, haz primero el **alta** (sección 1) y nada más.

## 1. Alta (solo si la ficha está incompleta)

Busca en Outlook (bandeja y enviados, palabras clave de la ficha, últimos 6 meses),
Teams y SharePoint para completar: cliente y su interlocutor designado, equipo interno,
agentes externos (DF, contratista, ingenierías, Administración), carpeta de SharePoint,
hitos y fechas, documentos clave y asuntos abiertos. Lo que no puedas confirmar déjalo
como `PENDIENTE (motivo)`. Vuelca los asuntos abiertos en `estado.md` y manda a Jose:

```
🤖 [<NOMBRE>] Alta del agente del expediente — revisa la ficha: <enlace al commit o resumen de 5 líneas>
Dudas: <lo que no he podido confirmar>
```

## 2. Recogida diaria de información

Desde la última ejecución (fecha en `diario.md`):

- **Correo**: hilos que casen con las palabras clave o los contactos de la ficha.
  Responde tú lo técnico/operativo **a internos y profesionales externos** (DF,
  contratista, ingenierías), igual que la skill `jornada`. Lo que sea **del cliente** o
  para el cliente → sección 4 (borrador).
- **Agenda**: reuniones del expediente en los próximos 7 días → prepara orden del día con
  los pendientes de `estado.md`.
- **Reuniones pasadas**: transcripciones/resúmenes de Teams → acta con acuerdos,
  responsables y fechas en `estado.md`; envía el acta a los asistentes internos.
- **SharePoint**: documentos nuevos o modificados en la carpeta del expediente → si son
  técnicos (mediciones, presupuesto, certificación, oferta, modificado), revisión técnica
  según la sección 3 de la skill `jornada`.
- **Estado**: lo vencido o a 2 días de vencer → recordatorio al responsable (máx. 2
  recordatorios; el tercero se escala a Jose).

Anota en `diario.md` una entrada por ejecución (fecha, novedades, acciones) y en
`registro/AAAA-MM-DD.md` cada acción exterior con el prefijo `[<clave>]`.

## 3. Informe semanal (lunes, o con `semanal`)

Genera dos documentos en `informes/AAAA-MM-DD-*.md` y súbelos a la carpeta de SharePoint
del expediente (subcarpeta `Informes agente`), si existe:

**a) Informe interno** (`-interno.md`) → envíalo por correo a Jose y al equipo interno de
la ficha:

1. Situación general (🟢/🟡/🔴) en una línea y por qué.
2. Avance: hitos cumplidos y próximos, con fecha prevista vs. real.
3. Economía: presupuesto aprobado, certificado a origen, modificados (aprobados / en
   trámite), desviación. Solo cifras con fuente; si no hay, dilo.
4. Asuntos abiertos por responsable, con fecha.
5. Riesgos y propuesta de mitigación.
6. Decisiones que necesita Jose.
7. Comunicaciones con el cliente de la semana.

**b) Informe al cliente** (`-cliente.md`) → **nunca se envía solo**: prepara el borrador
en Outlook dirigido al interlocutor designado de la ficha (sección 4). Contenido:
avance de la semana, próximos pasos, lo que necesitamos de ellos y para cuándo. **Sin**
datos económicos internos, márgenes, problemas con subcontratas, ni nada jurídico.

## 4. Comunicaciones al cliente — siempre en borrador

Regla del piloto: **todo** lo que vaya al cliente (consultas, respuestas, informe
semanal, convocatorias) queda en borrador para que Jose lo apruebe:

1. Crea el borrador en Outlook (respuesta en el hilo si existe; si no, correo nuevo) con
   la firma de Jose. Destinatario: el interlocutor designado de la ficha; nunca el
   conjunto de socios.
2. Avisa a Jose en el canal de mando:
   `🤖 [<NOMBRE>] Borrador para cliente — <asunto> — <qué dice en una línea> — responde "[<NOMBRE>] envía" para mandarlo`.
3. Anótalo en `estado.md` (sección *Borradores pendientes de aprobación*).
4. Al recibir la aprobación: `outlook_send_draft`; si falla por imágenes en línea, usa el
   método alternativo de `memoria/sistemas.md`. Muévelo a *Enviados* en `estado.md`.

Un correo del cliente sin respuesta más de un día laborable → recuérdaselo a Jose en el
canal de mando.

## 5. Persistir

```
git add proyectos/<clave> registro && git commit -m "Expediente <clave> <fecha>" \
  && git pull --rebase origin master && git push origin HEAD:master
```

Si el push falla por red, reintenta hasta 4 veces (2, 4, 8, 16 s).
