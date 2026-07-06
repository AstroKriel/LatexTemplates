# LatexTemplates (Bragi; god of poetry, runes carved on his tongue)

`bragi` is the LaTeX writing layer of the [Asgard](https://github.com/AstroKriel/Asgard) ecosystem. It holds a reusable header template for MNRAS-style papers, factored out of `kriel-phd-thesis`'s `header/` organisation and hardened while building `kriel-beattie-schober-curvature`.

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

`packages/` and `aliases/` are kept separate on purpose: an unused `\usepackage` is a real
submission risk (it's one more thing that can fail to compile in arXiv's or the journal's
build environment), while an unused `\newcommand` is completely inert. So `packages/`
should be trimmed to what a given paper actually uses before submission; `aliases/` can
stay comprehensive.

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

---

## Before submitting

- Trim `header/packages/*.tex` down to packages the paper actually uses -- an unused
  `\usepackage` is pure risk for zero benefit.
- Strip `\TODO{...}` / `\mhl{...}` usages (draft highlighting aids, see
  `aliases/text-tools.tex`) -- fine for working copies, not for a submitted manuscript.
- Confirm the bibliography is built with classic `bibtex` (`\bibliographystyle` +
  `\bibliography`), not `biblatex`/`biber` -- arXiv and most journal pipelines expect a
  bibtex-generated `.bbl`.
- If anything lives in a subdirectory beyond `header/` and `notes/`, do a fresh
  test-compile (ideally in a clean directory or via arXiv's own compiler) before relying
  on it for the actual submission -- `\subimport`/subdirectories are supported but are
  newer, less-travelled machinery than a flat `\input` structure.

---

## Known-unused packages worth reconsidering

`header/packages/` currently includes some packages already known to be unused in
practice (e.g. `pstricks` was deliberately dropped from `packages/figures.tex` for this
reason -- it predates native `pdflatex` support and needs extra compatibility packages to
work without the dvips pipeline). Don't add packages "just in case" -- add them when a
document actually needs them.
