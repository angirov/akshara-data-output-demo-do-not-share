# east-data-output

Rendered static HTML site generated from the [east-data](https://github.com/angirov/east-data)
DB root by the [AksharaDB](https://github.com/angirov/aksharadb) toolkit (`make deploy`).

This repo holds only pipeline *output* — a browsable, static rendering of the `_bibs`,
`_persons`, and `_works` collections (bibliography/author/work data) and their cross-references.
It is regenerated wholesale on each deploy; nothing here is hand-edited.

## Layout

- `index.html`, `site.css`, `bibs/`, `persons/`, `works/` — the deployable static site (what
  `make deploy` leaves behind after cleaning up intermediates). This is what actually gets
  served, e.g. via GitHub Pages.
- `INTERMEDIATE_OUTPUT/` — normally deleted by `make deploy`; kept here once, renamed from its
  build-time name `OUTPUT/`, purely to show what the pipeline produces along the way before the
  final HTML render:
  - `collections.json` — the validated collections map: every item in every collection, keyed
    by its filename stem, as parsed from the YAML docs.
  - `db.sqlite` — the same data materialized into a relational SQLite DB, with foreign-key
    relations inferred from the schemas' `_`-prefixed properties.
  - `db_dump.sql` — a plain-text SQL dump of that database, for inspecting the schema/data
    without opening the `.sqlite` file.
  - `fk_rows.json` — the resolved foreign-key map (`parent_table -> parent_value -> child_table
    -> [other_fk_values]`) that `render_site.py` uses to build cross-reference links on each
    page.
