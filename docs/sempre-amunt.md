# SEMPRE AMUNT

[Ver SEMPRE AMUNT](https://area-mestalla.lamunecadesal.chatgpt.site/)

Diario independiente del Valencia CF desarrollado por Sergio Devece (SerxDev) como proyecto de Lenguajes de Marcas del ciclo DAM. El proyecto aplica el diseño editorial de un diario deportivo a una web adaptable, con rutas HTML independientes y un área privada de demostración.

## Funcionalidades

| Sección | Qué permite consultar |
| --- | --- |
| [Portada](https://area-mestalla.lamunecadesal.chatgpt.site/) | Crónica destacada, noticias, siguiente partido y resumen de clasificación. |
| [Actualidad](https://area-mestalla.lamunecadesal.chatgpt.site/actualidad/) | Artículos propios con fecha, autoría editorial y enlace a las fuentes. |
| [Fichajes](https://area-mestalla.lamunecadesal.chatgpt.site/fichajes/) | Incorporaciones confirmadas y filtro por tipo de operación. |
| [Entrenamientos](https://area-mestalla.lamunecadesal.chatgpt.site/entrenamientos/) | Partes de trabajo del primer equipo con acceso a la fuente oficial. |
| [Próximos partidos](https://area-mestalla.lamunecadesal.chatgpt.site/partidos/) | Calendario filtrable por localía y descarga iCalendar cuando se conoce la hora. |
| [Clasificación](https://area-mestalla.lamunecadesal.chatgpt.site/clasificacion/) | Tabla de LaLiga y acceso directo a la fila del Valencia CF. |
| [Estadísticas](https://area-mestalla.lamunecadesal.chatgpt.site/estadisticas/) | Filtros por nombre y posición y orden por minutos, goles o dorsal. |
| [Abonados](https://area-mestalla.lamunecadesal.chatgpt.site/abonados/) | Entrevista de ejemplo, sorteo de prueba y propuestas de eventos y viajes. |

La portada incluye un buscador de secciones y noticias. Las ilustraciones se generaron para el proyecto y los espacios publicitarios son ejemplos. Los datos deportivos corresponden al corte del **12 de noviembre de 2026**. Se contrastaron con [Valencia CF](https://www.valenciacf.com/) y [LaLiga](https://www.laliga.com/clubes/valencia-cf/proximos-partidos). La estructura editorial toma como referencia la [sección del Valencia CF de Superdeporte](https://www.superdeporte.es/valencia-cf/).

## Arquitectura

| Archivo del proyecto | Responsabilidad |
| --- | --- |
| `contenido.py` | Noticias originales, calendario, clasificación, estadísticas y URLs de las fuentes. |
| `generar.py` | Plantillas HTML, metadatos, datos estructurados, índice de búsqueda, sitemap y calendario. |
| `public/assets/estilos.css` | Diseño adaptable, tipografía, tablas, navegación y componentes editoriales. |
| `public/assets/sitio.js` | Búsqueda, filtros, orden de tablas y preferencias de demostración en el navegador. |
| `worker/index.js` | Servicio de las rutas HTTP, redirecciones canónicas, respuestas 404 y acceso de abonados. |

El generador crea HTML por URL antes del despliegue. El servidor entrega ese HTML y JavaScript añade las interacciones. La comunidad de abonados exige un código de revisión y una sesión firmada en el servidor. Las selecciones de la demostración se guardan en el navegador.

Para ejecutar una copia del código fuente, usa Python 3.9+ y Node.js 22+. El comando `npm run dev` inicia la versión local en `http://localhost:3000/`. `npm run build` genera el HTML y el paquete de despliegue.

## SEO técnico

- 13 URLs editoriales con título, descripción, encabezado principal y enlace canónico por página.
- Enlaces HTML entre secciones y artículos y metadatos Open Graph para las vistas previas.
- Datos estructurados `WebSite`, `BreadcrumbList` y `NewsArticle`.
- [Sitemap XML](https://area-mestalla.lamunecadesal.chatgpt.site/sitemap.xml) con URLs canónicas y [robots.txt](https://area-mestalla.lamunecadesal.chatgpt.site/robots.txt) con su referencia.
- Imágenes en WebP, carga prioritaria de la portada y carga diferida de las secundarias.
- Las páginas editoriales se pueden indexar en producción. El área de abonados mantiene `noindex`.

Este es el fragmento de metadatos generado para el calendario en producción.

```html
<title>Próximos partidos del Valencia CF | SEMPRE AMUNT</title>
<meta name="description" content="Calendario del Valencia CF en LaLiga 2026/27 con horarios, estadios y televisión.">
<meta name="robots" content="index,follow,max-image-preview:large">
<link rel="canonical" href="https://area-mestalla.lamunecadesal.chatgpt.site/partidos/">
```

## Validación

- Navegación, búsqueda y filtros probados en escritorio y a ancho móvil durante la revisión.
- Compilación y validación de las 13 rutas del sitemap, URLs canónicas, redirección de rutas sin barra final y respuesta 404.
- Comprobación del acceso a abonados y del bloqueo de la ruta de comunidad sin sesión.
- Archivo HTML de Google desplegado en la raíz de la web y comprobado por HTTP 200 directo. El Worker entrega los bytes originales en la URL `.html` solicitada.
- Propiedad de prefijo de URL verificada en Google Search Console el 12 de noviembre de 2026.
- Sitemap enviado y leído por Google el 12 de noviembre de 2026. El informe de Sitemaps muestra el estado **Correcto**, con **13 páginas descubiertas** y **0 vídeos descubiertos**.

## Qué demuestra el proyecto

HTML semántico con `article`, `nav`, `time` y tablas, separación entre contenido y presentación, generación de XML y calendarios, JavaScript para interacciones y control de acceso desde el servidor. El proyecto reúne esos elementos en una publicación que se puede usar y revisar desde cualquier navegador.

## Edición automática y hemeroteca

La [hemeroteca](https://area-mestalla.lamunecadesal.chatgpt.site/hemeroteca/) reúne las piezas anteriores y las nuevas con filtros por texto, tipo y mes. Las URLs originales se conservan. Las nuevas publicaciones se incorporan al sitemap y al índice del buscador.

La edición diaria usa un Worker y una base D1 para guardar piezas y datos deportivos. El servicio valida las fuentes y los bloques completos, deduplica por fuente y tipo de publicación y conserva la última versión de las secciones que no reciben datos. La edición del 12 de noviembre de 2026 incluyó calendario, resultados, clasificación y estadísticas, además de una noticia, una entrevista atribuida y un artículo de opinión. La prueba inmediata añadió una crónica del filial. Se verificaron los enlaces públicos, la persistencia tras un despliegue y el sitemap de **18 URLs**.

La tarea está activa a las **09:00, hora de València**. La primera publicación se comprobó en producción y la prueba inmediata se ejecutó en la conversación mediante el acceso autenticado del servicio. La primera ejecución programada en segundo plano queda pendiente de comprobar.
