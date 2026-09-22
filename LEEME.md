# Generador de Campaña Inmobiliaria — herramienta KVA Agency

Página estática de un solo archivo (`index.html`, autocontenida: CSS + JS inline,
solo depende de Google Fonts vía CDN). Clon funcional — mismo motor de reglas,
misma estructura de 6 bloques de output — de la herramienta gratuita de YAXmedia
(`https://yax-media.agency/herramienta`), reskineada 100% con la identidad real
de KVA Agency (Ruta A "Papel y savia": crema/savia/terracota, Fraunces + IBM Plex,
ver `../../landing/styles.css`) y adaptada de voseo (UY/AR) a tuteo (MX/CO).

## Origen

2026-09-21: el usuario pidió clonar `yax-media.agency/herramienta` con la marca de
KVA Agency, "que haga exactamente lo mismo y dé exactamente los mismos resultados".
Se extrajo el motor de reglas completo (JS embebido en la página original, servida
desde un Funnel de GoHighLevel/`leadconnectorhq.com`) inspeccionando el HTML fuente
con Playwright — no hay backend ni IA real del lado del generador: todo es un árbol
de condicionales sobre los selects del formulario (precio → segmento de comprador,
intención → ángulos de copy, zona → intereses de Meta, etc.).

## Qué cambió vs. el original (y por qué)

- **Marca:** YAXmedia → KVA Agency. Logo real (isotipo SVG inline), paleta real
  (savia `#2F5D50` + terracota `#C8663D` sobre crema, no navy/teal — la ficha
  resumen de `Vault KVA/Entidades/KVA Agency.md` está desactualizada frente al
  sistema visual ya shippeado en `07-identidad-visual/`), tipografías Fraunces +
  IBM Plex Sans/Mono.
- **Moneda:** UYU/ARS → MXN/COP (USD se mantiene). Tasas de conversión aprox. para
  segmentar: MXN/18.5, COP/4200 (referenciales, igual que el original).
- **Geografía:** "Montevideo, Buenos Aires" → "Ciudad de México, Guadalajara,
  Monterrey" (decisión canónica 4: México principal + Colombia secundario, nada
  más). "Punta del Este" → "Los Cabos" como interés de Meta para zona costera.
- **Vocabulario:** piscina→alberca, parrillero→asador, dormitorio→recámara,
  alquiler→renta — términos reales de México, no Rioplatense.
- **Voz:** voseo ("Cargá", "vos", "Guardate") → tuteo ("Carga", "ti", "Guarda"),
  igual que la landing real de KVA (`../../landing/index.html` usa "tu", "te",
  "hablas" en todo su copy).
- **WhatsApp:** `wa.me/50685986742` (mismo número que ya usa la landing real de
  KVA, confirmado por el usuario) en vez del número de YAXmedia.
- **Banner "guardate este link":** ajustado — el original depende del mecanismo de
  link único de GHL Funnels; acá es una página estática sin esa lógica, así que
  el texto se cambió a "guarda esta página en tus favoritos" para no prometer algo
  que la página no hace.

## Qué se mantuvo idéntico (a propósito)

Toda la lógica de negocio: los 5 tramos de segmento por precio (USD <80k / <180k /
<350k / <700k / resto), el árbol de intención (auto-detección), los 3 ángulos de
copy por intención, las preguntas de prefiltro, la secuencia de seguimiento de 5
pasos y el checklist de lanzamiento de 7 pasos — todo byte-a-byte igual que el
original, solo con los textos reskineados.

## Verificado (Playwright, 2026-09-21)

- Flujo completo probado con 2 escenarios (USD/inversión en Polanco CDMX,
  MXN/vivienda en Zapopan) — 0 errores de consola.
- Segmentación por precio funciona correctamente en las 3 monedas (conversión a
  USD interna antes de clasificar).
- Visualmente consistente con la landing real de KVA Agency (misma paleta,
  mismas fuentes, mismo isotipo).

## Correcciones del usuario (2026-09-21, tras la primera preview)

1. **Copy del anuncio:** antes solo se mostraba 1 copy (ángulo principal). Ahora
   `construirCreativos()` arma un `cuerpo` común (PAS + datos + CTA) y genera un
   `copies[]` — un copy completo por cada uno de los 3 ángulos, cada uno con su
   propio botón "Copiar" (mismo cuerpo/oferta, gancho de apertura distinto, para
   test A/B real de hooks).
2. **CTA final — alcance real de KVA:** el párrafo decía que el equipo de KVA
   implementa "...formularios de prefiltro **y seguimiento**". Fuera de alcance
   según `Vault KVA/Entidades/KVA Agency.md` (KVA nunca responde leads ni hace
   seguimiento — eso es del equipo del cliente). Se quitó "y seguimiento" de la
   lista y se ajustó el cierre a "para que tu equipo se dedique a responder leads
   y cerrar ventas". La sección 5 (paso a paso de seguimiento) no se tocó: es
   consejo para que el cliente lo haga, no una promesa de servicio de KVA.
   **Corrección 2026-09-21 (mismo día, segunda pasada):** el usuario pidió sacar
   la palabra "leads" del cierre — quedó "para que tu equipo se dedique a
   **contactar a personas interesadas en comprar** y cerrar ventas".

## Hosting

- **2026-09-21 — preview rápida en vivo:** publicada con
  `_recursos/web/publicar-en-github.sh` (mismo script que documenta
  `../../landing/LEEME.md`) para verla funcionando sin editar nada primero.
  - Repo: `github.com/KVAAgency/kva-generador-campana` (público, GitHub Pages).
  - URL: **https://kvaagency.github.io/kva-generador-campana/**
  - Verificado con Playwright: HTTP 200, 0 errores de consola, título correcto.
  - Esto es **solo la vista previa**, no el hosting final.
- **Hosting final (pendiente, decisión del usuario):** el usuario planea
  publicarlo igual que `go.agencykva.com` (la landing real) — Cloudflare Pages
  conectado al repo de GitHub + subdominio propio vía CNAME en GoDaddy, sin
  tocar DNS/correo/GoHighLevel (ver `ESTADO.md` de la marca, sección "🟢 EN
  VIVO", y skill `publicar-sitio`). Falta decidir el subdominio (ej.
  `go2.agencykva.com` o similar, ya que `go.` está tomado por la landing) y
  correr el flujo de Cloudflare Pages sobre este mismo repo.
- Antes de conectar el dominio real, pasar por el checklist de
  `Vault KVA/Referencia — Qué se puede publicar.md` (decisión canónica 17) — a
  simple vista no revela precios ni el método exacto de KVA, pero no se auditó
  formalmente contra esa checklist.
