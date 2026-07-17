# Escritores por País

Guía cultural, construida con [Jekyll](https://jekyllrb.com/) y el tema [Just the Docs](https://just-the-docs.com/), con los escritores y libros más celebrados de cada país del mundo — organizados en clásicos, siglo XIX, siglo XX y siglo XXI — más un mapa mundial interactivo.

## Desarrollo local

```bash
bundle install
bundle exec jekyll serve
```

Luego abre `http://localhost:4000`.

## Estructura

- `_data/writers.yml` — fuente de datos única: país, continente, coordenadas y escritor/obra por época.
- `docs/clasicos.md`, `docs/siglo-19.md`, `docs/siglo-20.md`, `docs/siglo-21.md` — páginas generadas a partir de `_data/writers.yml` vía el include `_includes/century_table.html`.
- `mapa.md` — mapa mundial interactivo (Leaflet) con los escritores de cada país.
- `index.md` — página de inicio.
