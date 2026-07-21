# La Roja 2026 — Página de exhibición

Página web de una sola pieza que presenta las equipaciones de la Selección Española
tras el Mundial 2026. Estética editorial oscura, vídeo de producto a pantalla completa
y un selector que cambia entre primera y segunda equipación retiñendo toda la página.

> **Proyecto conceptual sin afiliación oficial** con la RFEF, la FIFA, Adidas ni MARCA.
> Ver [Derechos y contenido de terceros](#derechos-y-contenido-de-terceros) antes de publicar.

---

## Qué incluye

- **Hero con vídeo** — la camiseta gira bajo un haz de luz que ilumina la segunda estrella.
  En escritorio el vídeo es fondo a sangre completa desplazado a la derecha; en móvil pasa
  a ser una banda superior con el texto debajo, para que nada tape el escudo.
- **Selector de equipación** — conmuta la foto, el texto descriptivo y la paleta de toda
  la página (carmesí + oro ⟷ granate + oro claro).
- **Palmarés, ficha técnica y paleta cromática** de la equipación.
- **Sin dependencias ni compilación.** Un único `index.html` con el CSS y el JS embebidos.
  Lo único externo son las fuentes de Google Fonts.

## Cómo verla en local

No hace falta servidor ni instalar nada:

```bash
# Basta con abrir el archivo
start index.html          # Windows
open index.html           # macOS
```

Si prefieres servirla por HTTP (recomendado, evita restricciones del protocolo `file://`):

```bash
python -m http.server 8000
# luego abre http://localhost:8000
```

## Publicar en GitHub Pages

1. Crea un repositorio y sube el contenido de esta carpeta **a la raíz** del repo
   (el `index.html` debe quedar en el nivel superior):

   ```bash
   git init
   git add .
   git commit -m "Página de exhibición La Roja 2026"
   git branch -M main
   git remote add origin https://github.com/<usuario>/<repo>.git
   git push -u origin main
   ```

2. En el repositorio: **Settings → Pages → Build and deployment**
   - Source: `Deploy from a branch`
   - Branch: `main` · carpeta `/ (root)`

3. En un par de minutos estará en `https://<usuario>.github.io/<repo>/`.

## Estructura

```
├── index.html          Página completa (HTML + CSS + JS embebidos)
├── camiseta.mp4        Vídeo del hero — 1920×1080, 10 s
├── poster.jpg          Primer fotograma, se muestra mientras carga el vídeo
├── escudo.png          Escudo RFEF original (1970×3332)
├── escudo-mark.png     Escudo reescalado a 72 px para la barra de navegación
├── cami1.jpg/.webp     Primera equipación — original y recorte con transparencia
└── cami2.jpg/.webp     Segunda equipación — original y recorte con transparencia
```

Los `.webp` son los que usa la página: mismo recorte pero con canal alfa, para que
las camisetas se integren en el fondo oscuro sin recuadro blanco.

> `cami3.webp` y `cami4.webp`, si están presentes, **no se usan** en la página.
> Puedes borrarlos o incorporarlos como vistas adicionales.

## Detalles de implementación

**Tipografía** — Playfair Display para titulares, Inter para texto. Escala fluida con
`clamp()`; en escritorio el tamaño del titular se reduce a propósito para que nunca
alcance a la camiseta del vídeo.

**Color** — todo se deriva de variables CSS (`--brand`, `--accent`…). El selector de
equipación reescribe esas variables en `:root`, así que el resplandor del hero, los
degradados de las tarjetas y el brillo de los botones cambian juntos sin tocar
un solo color a mano.

**Movimiento** — animaciones solo sobre `transform` y `opacity`. Las apariciones al
hacer scroll usan `IntersectionObserver` y se desconectan tras dispararse.

**Rendimiento** — el vídeo se pausa solo al salir de pantalla, las imágenes declaran
`width`/`height` para no provocar saltos de maquetación (CLS) y los recortes van en
WebP (56 KB y 33 KB).

## Accesibilidad

- Enlace de salto al contenido y foco visible en todos los elementos interactivos.
- Selector de equipación con patrón ARIA de pestañas y navegación por flechas.
- Botón de pausa del vídeo (WCAG 2.2.2: todo medio que se reproduce solo más de 5 s
  necesita control).
- `prefers-reduced-motion` desactiva el barrido de luz, la flotación y las apariciones;
  el vídeo se queda en el póster.
- Áreas táctiles de 44 px mínimo y respeto de las áreas seguras (`safe-area-inset`).
- Textos alternativos descriptivos en las camisetas.

Verificado a 375, 1024, 1440 y 1920 px, sin scroll horizontal.

## Derechos y contenido de terceros

**Esta página es un ejercicio de diseño. No la publiques como si fuera oficial.**

Ninguno de los elementos de marca es de creación propia:

| Recurso | Titular |
|---|---|
| `camiseta.mp4`, `poster.jpg` | Metraje de **MARCA** |
| `escudo.png`, `escudo-mark.png` | Escudo de la **RFEF**, marca registrada |
| `cami1.*`, `cami2.*` | Fotografías de producto — **Adidas / la tienda de origen** |
| Diseño de las equipaciones | **Adidas / RFEF** |

Subir el repositorio a GitHub **redistribuye estos archivos**. Para un repositorio
privado o un ejercicio de portfolio suele ser asumible; para una web pública deberías
sustituirlos por material propio o con licencia, o pedir permiso a los titulares.

El **código** (HTML, CSS y JavaScript) es original y puedes usarlo libremente. Si añades
un archivo `LICENSE` (MIT, por ejemplo), deja claro que la licencia cubre el código
**y no los recursos gráficos ni audiovisuales**.

## Créditos

Diseño y desarrollo de la página: código original.
Diseño de las camisetas, escudo y material audiovisual: sus respectivos titulares.
