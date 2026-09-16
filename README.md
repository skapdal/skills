# skapdal/skills

Skills compartidas del equipo de skapdal para Claude. Cada skill vive en su propia carpeta bajo `skills/`, con un `SKILL.md` que Claude lee para reproducir el proceso siempre de la misma forma.

## Estructura

```
skills/
  <nombre-skill>/
    SKILL.md
    references/   (opcional, contenido de apoyo)
```

## Skills disponibles

| Skill | Qué hace |
|---|---|
| [`propuesta-mejora-web`](skills/propuesta-mejora-web/SKILL.md) | Audita el diseño/UX de un sitio y entrega una propuesta de mejora lista para presentar a un cliente (Word/Markdown). |

## Instalación

Agregá este repositorio como fuente de plugins de Claude para tener disponibles todas las skills de acá. No hace falta invocarlas por nombre — cada una se dispara sola cuando el pedido calza con su descripción.

## Agregar una skill nueva

1. Crear `skills/<nombre-skill>/SKILL.md` con frontmatter `name` + `description` (la descripción es lo que decide cuándo se dispara — tiene que incluir frases concretas que alguien usaría para pedirla).
2. Sumarla a la tabla de este README.
3. Si el proceso es largo, dejar el `SKILL.md` liviano y mover el detalle a `skills/<nombre-skill>/references/`.
