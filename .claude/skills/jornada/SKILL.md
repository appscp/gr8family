---
name: jornada
description: Ejecuta un turno de trabajo del agente de Jose Gómez en Urbanz (inicio, seguimiento o cierre de jornada). Usar cuando un disparo programado o Jose pida "inicio", "seguimiento" o "cierre".
---

# Turno de jornada

Argumento: `inicio` (08:45), `seguimiento` (13:45) o `cierre` (18:55). Hora de Madrid.

## 0. Contexto (siempre)

1. Lee `CLAUDE.md`, `memoria/instrucciones.md`, `memoria/estado.md`, `memoria/urbanz.md`
   y el `registro/` de hoy y del último día laborable.
2. Carga las herramientas con ToolSearch: `+Microsoft_365` (y `odoo` si
   `memoria/sistemas.md` indica Odoo configurado).
3. **Lee el canal de mando**: chat de Teams indicado en `memoria/sistemas.md`. Todo
   mensaje de Jose posterior a tu último mensaje es una orden o corrección:
   - Orden → ejecútala en este turno (respetando límites duros salvo que la orden sea
     explícita sobre ese caso concreto).
   - Corrección de una acción tuya → deshazla o rectifícala si es posible (p. ej. correo
     de rectificación) y añade la regla a `memoria/instrucciones.md`.
   - Confirma brevemente en el mismo chat: `🤖 Recibido: …`.

## 1. Correo (Outlook)

Busca correo recibido desde el último turno (bandeja de entrada y menciones a Jose).
Para cada hilo clasifica:

- **Responder** — puedes contestar con lo que sabes → responde (reply / reply-all según
  corresponda), con la firma de Jose.
- **Delegar** — le toca a otra persona → reenvía o escribe con instrucción clara:
  qué, quién, para cuándo, criterio de aceptación. Apunta el seguimiento en `estado.md`.
- **Revisión técnica** — adjuntos de proyecto, mediciones, presupuestos, ofertas,
  certificaciones → aplica la sección 3.
- **Escalar** — toca un límite duro → borrador preparado + entrada en el reporte.
- **Informativo / ruido** — sin acción; etiqueta si procede.

No dejes un correo de cliente, socio o profesional sin respuesta más de un día laborable
sin motivo anotado.

## 2. Proyectos y obras

- Revisa la agenda de hoy y mañana: prepara cada reunión (orden del día, puntos
  pendientes de `estado.md`, documentación) y envíala a los asistentes si aporta.
- Revisa transcripciones/resúmenes de reuniones de Teams de las últimas 24 h: extrae
  acuerdos y tareas → envía acta breve con responsables y fechas a los asistentes
  internos y registra en `estado.md`.
- Recorre `estado.md`: lo vencido o a punto de vencer → recordatorio al responsable.
  Si ya se recordó dos veces sin respuesta → escala en el reporte.
- Odoo (si está configurado): tareas vencidas, sin responsable o sin fecha por proyecto;
  corrige y avisa.

## 3. Revisión técnica

Para documentos técnicos (SharePoint/OneDrive o adjuntos):
- Comprueba coherencia mediciones ↔ presupuesto ↔ memoria de calidades ↔ planos citados.
- Compara ofertas en un cuadro (precio, alcance, exclusiones, plazo, condiciones de pago).
- Señala partidas sin medir, precios fuera de mercado, omisiones normativas (CTE, accesibilidad,
  seguridad y salud), cambios no aprobados en control de cambios.
- Devuelve al autor observaciones numeradas y accionables, con fecha de respuesta.
- Guarda el informe en SharePoint junto al documento si tienes permiso de escritura.

## 4. Registro

Cada acción que salga hacia fuera o cambie un sistema se anota en `registro/AAAA-MM-DD.md`:

```
- HH:MM [correo|teams|odoo|agenda|sharepoint] Acción — destinatario/objeto — motivo — enlace/id
```

## 5. Solo en `cierre`: reporte a Jose (Teams, canal de mando)

Mensaje con este formato, conciso (cada punto una línea):

```
🤖 Reporte de jornada — <fecha>

🔴 Requiere tu decisión
1. <asunto> — <qué hay que decidir> — <borrador preparado / enlace>

✅ Hecho hoy (<n> acciones)
- <proyecto>: <acción> → <persona>

⏳ En seguimiento
- <tarea> — <responsable> — vence <fecha>

⚠️ Riesgos / desviaciones
- <proyecto>: <riesgo> — <propuesta>

📅 Mañana
- <reuniones y prioridades>

Respóndeme aquí para corregir cualquier acción o darme instrucciones; las aplicaré en el
próximo turno.
```

Si algo es urgente (🔴) en `inicio` o `seguimiento`, avisa a Jose en ese momento por el
mismo chat, sin esperar al cierre.

## 6. Persistir

Actualiza `memoria/estado.md` y `memoria/urbanz.md` (personas/proyectos nuevos), luego:

```
git add memoria registro && git commit -m "Jornada <fecha> <turno>" && git push origin HEAD
```
