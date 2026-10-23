# UMR-Chan Aniyomi Desktop – extensiones HTML

Repositorio de extensiones para **AniyomiDesktopKotlin**. La app descarga el índice raw:

```
https://raw.githubusercontent.com/UMR-Chan/UMR-Chan-aniyomi-desktop/main/index.json
```

y cada extensión listada en `extensions/`. No hay código ejecutable: cada extensión es un JSON con
selectores CSS (Jsoup) y expresiones regulares.

## Formato de una extensión

| Campo | Uso |
|---|---|
| `id`, `name`, `version`, `lang`, `description` | Metadatos. Sube `version` para que la app reemplace la copia en caché. |
| `baseUrl` | Dominio de la fuente. Se usa para saber a qué extensión pertenece una URL. |
| `codec` | `av1` / `hevc` si el vídeo no lo puede decodificar el reproductor interno (JavaFX). La app usa VLC o el navegador. |
| `headers` | Cabeceras para todas las peticiones (p. ej. `User-Agent`). |
| `search.url` | Plantilla con `{baseUrl}` y `{query}`. |
| `search.item` / `title` / `link` / `thumbnail` | Selectores CSS de cada resultado. |
| `episodes.linkRegex` | Regex sobre el `href` absoluto de los enlaces de episodio; el grupo `numberGroup` es el número. |
| `episodes.dataRegex` + `dataTemplate` | Opcional: regex sobre el HTML/JSON embebido para generar todos los episodios (`{animeUrl}/{1}`). |
| `video.servers[]` | Servidores en orden de preferencia. Cada `step` descarga la URL actual, aplica `regex` y construye la siguiente URL con `template` (`{1}` = grupo 1). El último resultado es el stream. |
| `video.servers[].type` | `mp4` o `hls`. |
| `video.servers[].streamHeaders` | Cabeceras que exige el host del vídeo; la app las envía a través de su proxy local. |

## Añadir una extensión

1. Crea `extensions/<id>.json`.
2. Añádela a `index.json`.
3. Pruébala desde el proyecto: `gradlew checkExtension -PextQ="nombre" -PextSpec=ruta/al/<id>.json`.
