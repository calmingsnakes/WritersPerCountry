# Escritores por País

Guía cultural, construida con [Jekyll](https://jekyllrb.com/) y el tema [Just the Docs](https://just-the-docs.com/), con hasta 3 escritores/obras célebres por país y por época (clásico, siglo XIX, XX y XXI) — organizada por continente y país — más un CSV descargable que funciona como tracker de lectura.

## Desarrollo local

```bash
bundle install
bundle exec jekyll serve
```

Luego abre `http://localhost:4000`.

## Estructura

- `_data/writers.yml` — fuente de datos única: país, continente y hasta 3 obras {author, work, year, enlace (opcional)} por época (clasico/siglo19/siglo20/siglo21). El campo `enlace` solo está presente en obras verificadas como de dominio público, y apunta a una fuente legal de descarga (Project Gutenberg, Internet Archive, Wikisource, etc.).
- `docs/europa.md`, `docs/america-del-norte.md`, `docs/america-central-caribe.md`, `docs/america-del-sur.md`, `docs/asia.md`, `docs/africa.md`, `docs/oceania.md` — páginas (layout `default` de Just the Docs) generadas a partir de `_data/writers.yml` vía el include `_includes/country_section.html`; cada país es una sección con sus 4 épocas.
- `escritores-por-pais.csv` — export CSV (continente, país, autor, obra, siglo, leido, dominio_publico, enlace_descarga) generado desde `_data/writers.yml`, una fila por obra, pensado como tracker de lectura descargable desde la página de inicio.
- `dominio-publico/` — investigación de origen de los enlaces de dominio público (no enlazada al sitio); `_data/writers.yml` es ahora la fuente canónica de esos enlaces tras la fusión.
- `index.md` — página de inicio.
