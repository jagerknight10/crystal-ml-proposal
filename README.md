# Crystal screening slides

Edit `idea-presentation.tex`. The deck focuses on the overall research idea
and includes self-contained references for Crys-JEPA, MatterGen and MatterSim.
No separate `.bib` file or BibTeX run is needed.

Build the slide PDF once:

```sh
make pdf
```

Rebuild automatically whenever the source is saved:

```sh
make watch
```

Leave the watcher running in a terminal and open `idea-presentation.pdf` in
a PDF viewer that reloads changed files. Stop the watcher with Ctrl+C.
The watcher updates the PDF on disk; viewer refresh behavior depends on the viewer.
If a save contains a LaTeX error, fix it and save again to resume compilation.

Requires `latexmk` and pdfLaTeX with Beamer and TikZ, already available on this machine.
The editable slide source is LaTeX Beamer; this folder does not contain a PowerPoint export.
