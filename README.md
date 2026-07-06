# LatexTemplates (Bragi; god of poetry, runes carved on his tongue)

`bragi` is the LaTeX writing layer of the [Asgard](https://github.com/AstroKriel/Asgard) ecosystem. It holds a reusable header template for MNRAS-style papers: useful packages and macros, organised in one place.

---

## Layout

```
header/
├── header.tex        single entry point; \subimport-s everything below
├── mnras.cls          official MNRAS class -- keep in sync with the journal's current version
├── mnras.bst          official MNRAS bibliography style
├── packages/          \usepackage lines only, one topic per file
└── aliases/           \newcommand macros only, one topic per file
```

`packages/` and `aliases/` are kept separate so each file has one clear purpose:
`packages/` is document/journal setup, `aliases/` is shorthand macros for writing content.

---

## Using this in a new paper

1. Copy `header/` into the new project.
2. In `main.tex`, right after `\documentclass{mnras}`:
   ```latex
   \usepackage{import}
   \subimport{header/}{header}
   ```
3. Any standalone note (e.g. `notes/some-derivation.tex`) that also needs the header
   loads it the same way, with a relative prefix:
   ```latex
   \usepackage{import}
   \subimport{../header/}{header}
   ```
   Do **not** replace `\subimport` with plain `\input` here -- `\input` resolves paths
   relative to the root document being compiled, not the file containing the `\input`
   call, so `header.tex`'s own internal includes would silently break when compiled
   from a subdirectory. `\subimport` (from the `import` package) resolves relative to
   the file it's called from, which is what makes `header.tex` reusable at any depth.
4. Add a `header/refs.bib` with the paper's bibliography.
5. Add any per-paper citation shortcuts to `header/aliases/ref-citealiases.tex`.
6. Add any per-paper macros (e.g. project-specific shorthand) to a new file under
   `header/aliases/`, or to an existing topic file if it fits.
