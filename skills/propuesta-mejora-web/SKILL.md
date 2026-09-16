---
name: propuesta-mejora-web
description: Usar cuando pidan auditar el diseño/UX de un sitio web y producir una "propuesta de mejora" para presentar a un cliente — estandariza tanto el relevamiento (qué revisar y cómo verificarlo) como el documento final (estructura, tono, tabla de prioridades). Disparadores: "análisis de diseño de una página", "auditoría UX", "propuesta de mejora para el cliente", "revisá el diseño de este sitio".
---

# Propuesta de mejora de diseño web

Estandariza dos cosas a la vez: **qué se revisa** de un sitio (siempre los mismos 5 ejes, más un eje de dirección estratégica cuando aplica) y **cómo se entrega** (mismo documento, misma tabla de prioridades, mismo tono). El objetivo es que dos auditorías hechas en momentos distintos se sientan del mismo estudio, y que el cliente reciba algo accionable, no una lista de opiniones sueltas.

## 0. Alcance y honestidad de los hallazgos

- Todo hallazgo que se presenta como hecho ("wp-admin expuesto", "el link de Instagram no funciona") tiene que haber sido **verificado en vivo** — navegando el sitio, no supuesto por el tipo de sitio o por experiencia previa con sitios similares. Si algo no se pudo verificar (p. ej. no se pudo tomar captura), decirlo explícitamente en vez de omitirlo o inventarlo.
- Si el cliente ya señaló un problema ("los links de contacto no andan"), igual conviene verificarlo directo antes de escribirlo en el documento — a veces el hallazgo real es más específico y más citable (ej.: no es que "no anden", es que apuntan a facebook.com genérico en vez del perfil real).
- Cuando el usuario pide una dirección de diseño puntual (minimalismo, migrar a X patrón, etc.), esa dirección va en su propia sección ("Dirección de diseño propuesta"), separada del diagnóstico — el diagnóstico son hechos observados, la dirección es la propuesta de hacia dónde ir.

## 1. Relevamiento

Preferir la versión **mobile** del sitio salvo que el cliente pida explícitamente desktop (suele ser el grueso del tráfico de e-commerce). Usar el navegador (built-in browser o Chrome, según corresponda) para:

1. Abrir la home, cerrar/registrar cualquier popup (newsletter, cookies, edad) y notar si el cierre funciona bien (un popup que no libera el scroll es en sí mismo un hallazgo).
2. Sacar capturas de las secciones principales (header, hero, navegación, footer) — son la base para hablar de paleta, tipografía y jerarquía con seguridad.
3. Usar `get_page_text` / `read_page` para relevar estructura, navegación y contenido sin depender solo de lo visual (útil también para detectar menús duplicados, mismatches de copy, etc. — cosas de accesibilidad/consistencia que no siempre se ven en una captura).

### Checklist de seguridad básica (siempre, no opcional)

- **Panel de administración expuesto**: probar `/wp-admin/`, `/admin/`, `/wp-login.php` u otra ruta estándar del CMS que use el sitio. Si carga un formulario de login sin fricción visible (sin CAPTCHA, sin 2FA aparente, sin bloqueo por intentos), es hallazgo de prioridad Alta.
- **Enlaces de contacto/redes**: abrir cada ícono de red social del footer/header y confirmar que apunte al perfil real del negocio (no a la home genérica de la plataforma). Confirmar que exista al menos un canal de contacto directo y clickeable (mail `mailto:`, WhatsApp, formulario) — un teléfono como texto suelto sin link no cuenta.
- Registrar cualquier otro enlace roto o que no lleve a ningún lado que se detecte de paso (footer, menú, CTAs).

Estas dos verificaciones no reemplazan un audit de seguridad completo — decirlo así en el documento ("se hicieron verificaciones puntuales, no un audit completo") para no sobrevender el alcance.

## 2. Los 5 ejes de diagnóstico (fijos)

Usar siempre estos títulos y este orden — es lo que hace comparables dos informes distintos:

1. **Identidad visual** — paleta, tipografía, logo, coherencia entre ellos.
2. **Estructura y jerarquía** — header, navegación, hero/carrusel, orden de la información.
3. **Usabilidad y primeras impresiones** — popups, fricciones de interacción, qué ve el usuario en los primeros segundos.
4. **Accesibilidad y consistencia de contenido** — marcado (menús duplicados, alt text), mismatches de copy/imagen, lectores de pantalla.
5. **Seguridad y enlaces de contacto** — el checklist de arriba, siempre presente aunque no haya hallazgos (en ese caso, decir que se verificó y no se encontraron problemas — no omitir la sección).

Si el cliente pidió una dirección de diseño concreta (minimalismo, un patrón de layout puntual, un rebranding), agregar una sección aparte **"Dirección de diseño propuesta"** después del diagnóstico, con un subtítulo por cada eje de esa dirección (ej.: "Minimalismo", "Home organizada por categorías en pictogramas"). Esta sección es prescriptiva (a dónde ir), a diferencia del diagnóstico que es descriptivo (qué se observó).

## 3. Tabla de recomendaciones priorizadas

Siempre en este formato, ordenada de mayor a menor prioridad:

| Prioridad | Hallazgo | Recomendación | Esfuerzo est. |

 Reglas de prioridad:
- **Alta**: todo lo de seguridad (sin excepción), y cualquier cosa que bloquee o confunda al usuario en el flujo principal (popup que traba el scroll, navegación rota).
- **Media**: inconsistencias visuales o de contenido que no bloquean pero sí restan profesionalismo.
- **Baja**: oportunidades de optimización sin fricción asociada (jerarquía de producto, mejoras incrementales).

Esfuerzo: Bajo / Medio / Medio-Alto / Alto — estimación gruesa, no un presupuesto.

## 4. Estructura del documento (en este orden)

1. Portada — título, subtítulo ("Diseño y experiencia web"), dominio, "Preparado para" / "Preparado por" + contacto, fecha.
2. Resumen ejecutivo (2 párrafos: qué se hizo, diagnóstico general en una frase).
3. Alcance del análisis (bullets de qué se revisó — incluir siempre el ítem de seguridad/contacto).
4. Diagnóstico (los 5 ejes fijos, sección 5 con callouts destacados en rojo para cada hallazgo de seguridad).
5. [Opcional] Dirección de diseño propuesta.
6. Recomendaciones priorizadas (tabla).
7. Qué funciona bien y conviene conservar (bullets — nunca omitir, todo sitio tiene algo rescatable y humaniza el informe).
8. Próximos pasos sugeridos (bullets accionables, orden lógico: primero lo urgente/bajo esfuerzo).
9. Pie de página con autoría.

## 5. Generar el .docx

Usar el skill `docx` (leer su SKILL.md primero: `/mnt/skills/public/docx/SKILL.md`) con este script como plantilla base — parametrizar `CONFIG` y las listas de datos, no reescribir la maquetación:

```js
const {
  Document, Packer, Paragraph, TextRun, HeadingLevel, AlignmentType,
  Table, TableRow, TableCell, WidthType, ShadingType, BorderStyle,
  Header, Footer, PageNumber, LevelFormat, VerticalAlign,
} = require("docx");
const fs = require("fs");

// ============ EDITAR POR CLIENTE ============
const CONFIG = {
  clienteNombre: "Nombre del cliente",
  dominio: "ejemplo.com",
  preparadoPor: "Adrian Freisinger",
  contacto: "afreisinger@gmail.com",
  fecha: "Mes 2026",
  archivoSalida: "propuesta-mejora-<cliente>.docx",
};

const resumenEjecutivo = [
  "Párrafo 1: qué se hizo y con qué objetivo.",
  "Párrafo 2: diagnóstico general en una frase, en qué se concentran las oportunidades.",
];

const alcance = [
  "Revisión de la página de inicio en formato mobile.",
  "Evaluación de identidad visual: paleta de color, tipografía, logo.",
  "Evaluación de estructura y jerarquía.",
  "Revisión de usabilidad y accesibilidad.",
  "Verificación básica de seguridad (accesos administrativos) y de los enlaces de contacto/redes.",
];

// cada eje: { titulo, texto } — texto plano, o { texto, callouts: [{titulo, lineas:[...]}] } para el eje de seguridad
const diagnostico = [
  { titulo: "1. Identidad visual", texto: "..." },
  { titulo: "2. Estructura y jerarquía", texto: "..." },
  { titulo: "3. Usabilidad y primeras impresiones", texto: "..." },
  { titulo: "4. Accesibilidad y consistencia de contenido", texto: "..." },
  {
    titulo: "5. Seguridad y enlaces de contacto",
    texto: "Se hicieron verificaciones puntuales, no un audit de seguridad completo:",
    callouts: [
      { titulo: "Panel de administración expuesto", lineas: ["..."] },
      { titulo: "Enlaces de contacto que no llevan a ningún lado", lineas: ["..."] },
    ],
  },
];

// opcional — omitir el h1 completo si no aplica
const direccionDiseno = {
  intro: "Más allá de las correcciones puntuales, se propone...",
  ejes: [
    { titulo: "Minimalismo", texto: "..." },
    { titulo: "Home organizada por categorías, en pictogramas", texto: "..." },
  ],
};

const recomendaciones = [
  { priority: "Alta", finding: "...", recommendation: "...", effort: "Bajo" },
  // seguridad siempre primero y siempre Alta
];

const queFunciona = ["...", "..."];
const proximosPasos = ["...", "..."];
// ============ FIN CONFIG ============

const DARK = "2E3B2E", ACCENT = "4C6B4F", TEXT = "2B2B2B", MUTED = "6B6B6B";
const RED = "B3413A", AMBER = "B8862B", GREEN = "3F7D4C", FONT = "Calibri";

const h1 = (text) => new Paragraph({ heading: HeadingLevel.HEADING_1, spacing: { before: 360, after: 160 },
  border: { bottom: { color: ACCENT, space: 4, style: BorderStyle.SINGLE, size: 6 } },
  children: [new TextRun({ text, bold: true, color: DARK, size: 30, font: FONT })] });
const h2 = (text) => new Paragraph({ heading: HeadingLevel.HEADING_2, spacing: { before: 280, after: 120 },
  children: [new TextRun({ text, bold: true, color: ACCENT, size: 24, font: FONT })] });
const body = (text) => new Paragraph({ spacing: { after: 160, line: 276 },
  children: [new TextRun({ text, color: TEXT, size: 21, font: FONT })] });
const bullet = (text) => new Paragraph({ numbering: { reference: "bullets", level: 0 }, spacing: { after: 90, line: 276 },
  children: [new TextRun({ text, color: TEXT, size: 21, font: FONT })] });
const priorityColor = (l) => ({ Alta: RED, Media: AMBER, Baja: GREEN }[l] || MUTED);

function callout(titulo, lineas) {
  return new Table({ width: { size: 9000, type: WidthType.DXA }, columnWidths: [9000], rows: [new TableRow({ children: [
    new TableCell({ width: { size: 9000, type: WidthType.DXA }, shading: { fill: "FBEEEC", type: ShadingType.CLEAR, color: "auto" },
      borders: { top: { style: BorderStyle.SINGLE, size: 4, color: RED }, bottom: { style: BorderStyle.SINGLE, size: 4, color: RED },
        left: { style: BorderStyle.SINGLE, size: 16, color: RED }, right: { style: BorderStyle.SINGLE, size: 4, color: "FBEEEC" } },
      margins: { top: 160, bottom: 160, left: 220, right: 220 },
      children: [new Paragraph({ spacing: { after: 80 }, children: [new TextRun({ text: titulo, bold: true, color: RED, size: 20, font: FONT })] }),
        ...lineas.map((t) => new Paragraph({ numbering: { reference: "bullets", level: 0 }, spacing: { after: 60, line: 264 },
          children: [new TextRun({ text: t, color: TEXT, size: 19, font: FONT })] }))] }) ] }) ] });
}

function recTable(rows) {
  const headerCells = ["Prioridad", "Hallazgo", "Recomendación", "Esfuerzo est."].map((t) =>
    new TableCell({ shading: { fill: DARK, type: ShadingType.CLEAR, color: "auto" }, verticalAlign: VerticalAlign.CENTER,
      margins: { top: 100, bottom: 100, left: 120, right: 120 },
      children: [new Paragraph({ children: [new TextRun({ text: t, bold: true, color: "FFFFFF", size: 18, font: FONT })] })] }));
  const widths = [1300, 3600, 4600, 1500];
  const dataRows = rows.map((r, i) => {
    const fill = i % 2 === 0 ? "FFFFFF" : "F5F7F4";
    const cell = (text, color = TEXT, bold = false, valign = false) => new TableCell({
      width: { size: widths[0], type: WidthType.DXA }, shading: { fill, type: ShadingType.CLEAR, color: "auto" },
      verticalAlign: valign ? VerticalAlign.CENTER : undefined, margins: { top: 100, bottom: 100, left: 120, right: 120 },
      children: [new Paragraph({ children: [new TextRun({ text, bold, color, size: 19, font: FONT })] })] });
    return new TableRow({ children: [
      cell(r.priority, priorityColor(r.priority), true, true),
      cell(r.finding), cell(r.recommendation), cell(r.effort, MUTED, false, true),
    ] });
  });
  return new Table({ width: { size: widths.reduce((a,b)=>a+b,0), type: WidthType.DXA }, columnWidths: widths,
    rows: [new TableRow({ tableHeader: true, children: headerCells }), ...dataRows] });
}

const bodyChildren = [
  h1("Resumen ejecutivo"), ...resumenEjecutivo.map(body),
  h1("Alcance del análisis"), ...alcance.map(bullet), body(""),
  h1("Diagnóstico"),
  ...diagnostico.flatMap((d) => [
    h2(d.titulo), body(d.texto),
    ...(d.callouts ? d.callouts.flatMap((c) => [callout(c.titulo, c.lineas), body("")]) : []),
  ]),
];

if (direccionDiseno) {
  bodyChildren.push(h1("Dirección de diseño propuesta"), body(direccionDiseno.intro));
  direccionDiseno.ejes.forEach((e) => bodyChildren.push(h2(e.titulo), body(e.texto)));
  bodyChildren.push(body(""));
}

bodyChildren.push(
  h1("Recomendaciones priorizadas"),
  body("Ordenadas de mayor a menor impacto esperado sobre la experiencia y la conversión."),
  recTable(recomendaciones), body(""),
  h1("Qué funciona bien y conviene conservar"), ...queFunciona.map(bullet), body(""),
  h1("Próximos pasos sugeridos"), ...proximosPasos.map(bullet), body(""),
  new Paragraph({ spacing: { before: 300 }, border: { top: { color: "D9DED7", space: 8, style: BorderStyle.SINGLE, size: 4 } },
    children: [new TextRun({ text: `Documento preparado por ${CONFIG.preparadoPor} (${CONFIG.contacto}) a partir de un relevamiento de diseño de ${CONFIG.dominio}.`,
      italics: true, color: MUTED, size: 18, font: FONT })] }),
);

const doc = new Document({
  numbering: { config: [{ reference: "bullets", levels: [{ level: 0, format: LevelFormat.BULLET, text: "•", alignment: AlignmentType.LEFT,
    style: { paragraph: { indent: { left: 360, hanging: 260 } } } }] }] },
  styles: { default: { document: { run: { font: FONT, size: 21, color: TEXT } } } },
  sections: [
    { properties: { page: { size: { width: 11906, height: 16838 }, margin: { top: 1700, bottom: 1400, left: 1300, right: 1300 } } },
      children: [
        new Paragraph({ spacing: { before: 1600 }, children: [] }),
        new Paragraph({ children: [new TextRun({ text: "PROPUESTA DE MEJORA", bold: true, color: MUTED, size: 22, font: FONT, characterSpacing: 20 })] }),
        new Paragraph({ spacing: { before: 120 }, children: [new TextRun({ text: "Diseño y experiencia web", bold: true, color: DARK, size: 52, font: FONT })] }),
        new Paragraph({ spacing: { before: 40, after: 800 }, children: [new TextRun({ text: CONFIG.dominio, color: ACCENT, size: 30, font: FONT })] }),
        new Paragraph({ border: { top: { color: ACCENT, space: 8, style: BorderStyle.SINGLE, size: 6 } }, spacing: { before: 200 }, children: [] }),
        new Paragraph({ spacing: { before: 300 }, children: [new TextRun({ text: "Preparado para", color: MUTED, size: 20, font: FONT })] }),
        new Paragraph({ spacing: { after: 200 }, children: [new TextRun({ text: CONFIG.clienteNombre, bold: true, color: TEXT, size: 24, font: FONT })] }),
        new Paragraph({ spacing: { before: 200 }, children: [new TextRun({ text: "Preparado por", color: MUTED, size: 20, font: FONT })] }),
        new Paragraph({ spacing: { after: 60 }, children: [new TextRun({ text: CONFIG.preparadoPor, bold: true, color: TEXT, size: 24, font: FONT })] }),
        new Paragraph({ children: [new TextRun({ text: CONFIG.contacto, color: MUTED, size: 20, font: FONT })] }),
        new Paragraph({ spacing: { before: 200 }, children: [new TextRun({ text: CONFIG.fecha, color: MUTED, size: 20, font: FONT })] }),
      ] },
    { properties: { page: { size: { width: 11906, height: 16838 }, margin: { top: 1300, bottom: 1300, left: 1300, right: 1300 } } },
      headers: { default: new Header({ children: [new Paragraph({ alignment: AlignmentType.RIGHT,
        border: { bottom: { color: "D9DED7", space: 4, style: BorderStyle.SINGLE, size: 4 } },
        children: [new TextRun({ text: `${CONFIG.clienteNombre} — Propuesta de mejora de diseño`, color: MUTED, size: 16, font: FONT })] })] }) },
      footers: { default: new Footer({ children: [new Paragraph({ alignment: AlignmentType.CENTER, children: [
        new TextRun({ children: [PageNumber.CURRENT], color: MUTED, size: 16, font: FONT }),
        new TextRun({ text: "  /  ", color: MUTED, size: 16, font: FONT }),
        new TextRun({ children: [PageNumber.TOTAL_PAGES], color: MUTED, size: 16, font: FONT }) ] })] }) },
      children: bodyChildren },
  ],
});

Packer.toBuffer(doc).then((buffer) => { fs.writeFileSync(CONFIG.archivoSalida, buffer); console.log("done"); });
```

Notas sobre el script:
- No forzar `PageBreak` manual antes de secciones largas (como la tabla) — deja páginas en blanco cuando el contenido previo ya cae justo al borde. Dejar que fluya solo.
- El callout rojo es solo para hallazgos de seguridad — no reusar ese color para otra cosa, así el cliente lo reconoce de un vistazo como "esto es urgente".

## 6. Verificar antes de entregar

Siempre, sin excepción:

```bash
python /mnt/skills/public/docx/scripts/office/soffice.py --headless --convert-to pdf <archivo>.docx
pdftoppm -jpeg -r 100 <archivo>.pdf page
```

Leer **todas** las páginas generadas (`Read` sobre cada `page-N.jpg`) antes de mandar el archivo. Buscar específicamente: páginas en blanco (señal de un salto de página mal puesto), tablas cortadas raro, texto que se sale del margen. Corregir y regenerar antes de entregar — no entregar sin haber mirado el render.

## 7. Versión Markdown (si la piden)

Misma estructura y mismo contenido, en `.md` plano: `#`/`##` para los títulos, tabla Markdown para las recomendaciones (usar 🔴/🟡/🟢 antes de Alta/Media/Baja para que la prioridad se distinga a simple vista), `>` para los callouts de seguridad con **⚠** al inicio de la primera línea.

## 8. Entrega

Copiar el archivo final a `/mnt/user-data/outputs/` y entregarlo con `SendUserFile`. No hace falta narrar los pasos del proceso al usuario — el resumen final alcanza con qué se encontró (1–2 líneas) y el archivo.
