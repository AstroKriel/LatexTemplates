# LatexTemplates (Bragi; god of poetry, runes carved on his tongue)

`bragi` is the LaTeX writing layer of the [Asgard](https://github.com/AstroKriel/Asgard) ecosystem: an organised header covering common capabilities, ready to copy into any document.

## Layout

```
header/
├── header.tex  # single entry point; \subimport-s everything below
├── packages/   # \usepackage lines, one topic per file
└── aliases/    # \newcommand macros, one topic per file
```

## Usage

Copy `header/` into a project, then in the main document:

```latex
\usepackage{import}
\subimport{header/}{header}
```

From a file one level deeper (e.g. `notes/`), use `\subimport{../header/}{header}` instead: `\subimport` resolves relative to the file that calls it, so this works at any depth.
