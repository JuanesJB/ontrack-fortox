# Tablero de resultados — Piloto FORTOX

Sitio estático de una sola página que muestra qué midió OnTrack en el piloto de FORTOX.
Sin dependencias externas: HTML + CSS + SVG dibujado en JS. Funciona abriendo `index.html`.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Todo el tablero: estructura, estilos y gráficos |
| `data.js` | El dataset ya agregado (`window.DATA`). **Generado desde los XLSX — no editar a mano** |
| `assets/`, `fonts/` | Logo, favicon y las tipografías de marca (Montserrat + Anybody) |

## Fuentes de los datos

1. `school_temp-report_1787231076-9952.xlsx` — hoja *Rutas ejecutadas*: 30 recorridos,
   21/07/2026 – 19/08/2026.
2. `school_temp-report_1787231252-2130.xlsx` — hoja *Velocidad excedida*: 129 eventos,
   01/06/2026 – 23/07/2026.
3. Ranking de conductores: módulo de calificación de la plataforma, periodo del piloto
   (transcrito a mano en `data.js`, clave `ranking`).

Los dos XLSX **no** están en este repositorio a propósito (ver la nota de privacidad).
Para regenerar `data.js` hay que volver a exportarlos de la plataforma.

## Publicar en GitHub Pages

```bash
git init && git add -A && git commit -m "feat: tablero de resultados del piloto FORTOX"
gh repo create <nombre> --private --source=. --push       # privado primero
gh api -X POST repos/:owner/<nombre>/pages -f source.branch=main -f source.path=/
```

> Pages **solo sirve sitios de repositorios públicos** en las cuentas Free. Si el repo es
> privado hace falta plan Pro/Team, o se publica con el modo anónimo activado.

## ⚠ Privacidad antes de publicar

El tablero muestra **nombres completos de conductores, placas y coordenadas GPS** de una
operación real. Una URL de GitHub Pages es pública e indexable.

Opciones, en orden de preferencia:

1. Compartir el archivo local o un PDF (botón *Imprimir / PDF*) en lugar de publicarlo.
2. Publicarlo con autorización explícita de FORTOX.

Nunca se incluyeron los números de documento de los conductores, que sí venían en el XLSX.

Cada rótulo con nombre o placa guarda además una variante enmascarada en `data-anon`
(`Robinson Rojas Balbin` → `R. RB`, `BKG90D` → `BK•••D`). Hoy no se usa — no hay botón —
pero si algún día hace falta publicar sin nombres, basta recorrer `[data-real]` y cambiar
el `textContent` por el `data-anon`.

## Cartografía

`assets/mapa-corredor.png` (Autopista Norte, Chía – Cajicá, zoom 13) y
`assets/mapa-contexto.png` (zoom 10) son imágenes **estáticas** armadas una sola vez con
teselas de CARTO *light_all* @2x; van embebidas para que el tablero no dependa de internet
ni de una librería de mapas. Los puntos se dibujan encima en SVG con la misma proyección
Web Mercator, con los parámetros `CORR` y `CTX` que están en el script de `index.html`
(nivel de zoom + origen en píxeles). Si se cambia el recorte hay que actualizar esos dos
objetos, o los puntos dejan de caer sobre la vía.

La atribución **© OpenStreetMap · CARTO** debe permanecer visible bajo los mapas: es
condición de uso de las teselas.
