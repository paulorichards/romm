# La rama `casa`

Fork de [rommapp/romm](https://github.com/rommapp/romm) para el NAS de la casa.
`casa` corre nuestros parches encima de un release de upstream.

⚠️ **AGPL-3.0.** Distribuir la imagen obliga a publicar el fuente modificado,
y por eso este repo es **público**. No lo pases a privado mientras el NAS corra
una imagen de acá.

A diferencia del fork de `wardrowbe`, acá los parches **sí se piensan para
upstream**: están escritos contra `master`, con tests, en inglés y siguiendo el
`AGENTS.md` del proyecto. La rama `casa` es sólo el atajo para correrlos ya.

| Rama | Qué es | Quién la mueve |
|---|---|---|
| `master` | espejo de `upstream/master` | nosotros, al sincronizar |
| `casa` | release de upstream + nuestros parches + `casa-build` | nosotros al agregar un parche |
| `feat/*`, `fix/*` | un parche, nacido de `master`, PR-able a upstream | nosotros |

`.casa/base` dice sobre qué release de upstream está `casa`.

## El ciclo de un parche

1. Rama desde `master`, conventional commits, test incluido, `trunk fmt`.
2. PR a upstream desde esa rama (con la declaración de uso de IA, que el
   `AGENTS.md` de RomM exige y es no negociable).
3. El mismo commit, `cherry-pick` a `casa` → `casa-build` publica imagen.
4. Si upstream lo mergea, al sincronizar el parche desaparece solo del rebase.

⚠️ El `cherry-pick` a `casa` **choca** cuando el archivo se movió entre el
release y `master`: `useCoverArt` vive en `frontend/src/v2/composables/` en
5.4.1 y en `frontend/src/composables/` en `master`. Es un conflicto de ruta,
no de contenido.

## Parches que corren hoy

| Parche | Por qué |
|---|---|
| `aspect_ratio` por plataforma | la columna existía desde la migración 0027 y nunca llegó a la API, así que las carátulas apaisadas de SNES, las cuadradas de PSX y GBA y las altas de Super Famicom se recortaban todas en la misma caja 2/3. Medido en la biblioteca de casa: sólo el 41 % de 16.522 carátulas se recortaba 15 % o menos |

## Imágenes

`ghcr.io/paulorichards/romm:5.4.1-casa.N` — 5.4.1 es el release de upstream
debajo, N la corrida de `casa-build`. Sólo amd64. El despliegue es a mano desde
`nas-gitops`.

## Workflows de upstream

Los de `pull_request` no molestan (no hay PRs acá salvo los nuestros), pero
`trunk-upgrade` y `trunk-check-all` corren por `schedule`: conviene apagarlos
si empiezan a abrir ruido.
