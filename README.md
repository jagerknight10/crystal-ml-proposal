# Graph-Guided Crystal Screening

LaTeX Beamer slides describing a chemistry-aware reference graph for screening diffusion-generated crystals, building on Crys-JEPA, MatterGen, and MatterSim.

Edit the LaTeX source and compile it to produce a presentation PDF. The instructions below cover Windows and macOS, including automatic PDF rebuilding when you save in VS Code.

## Project files

| File | Purpose |
| --- | --- |
| [`idea-presentation.tex`](idea-presentation.tex) | Editable slide source, including diagrams and references |
| [`idea-presentation.pdf`](idea-presentation.pdf) | Rendered slides, ready to present or share |
| [`Makefile`](Makefile) | Optional shortcuts for building and watching the source |
| [`diagrams/slide-architectures.tex`](diagrams/slide-architectures.tex) | Slide-sized TikZ architectures included by the presentation |
| [`diagrams/`](diagrams/) | Standalone diagram sources, vector PDFs, and PNG previews |

The output is a widescreen Beamer PDF. This project does not currently generate an editable PowerPoint `.pptx` file.

## 1. Install the tools

You need VS Code, a LaTeX distribution, and the LaTeX Workshop extension. VS Code is the editor; the LaTeX distribution supplies the programs that compile `.tex` files into PDFs.

### Windows

1. Install [Visual Studio Code](https://code.visualstudio.com/download).
2. Download and install [MiKTeX](https://miktex.org/download). The [official Windows installation guide](https://miktex.org/howto/install-miktex) explains the installer options.
3. Open **MiKTeX Console**, check for updates, and install available updates. In its settings, enable automatic installation of missing packages. If you leave this set to ask first, approve package installation prompts during the initial build.
4. Install [Strawberry Perl](https://strawberryperl.com/). MiKTeX's `latexmk` needs a Perl interpreter. The [LaTeX Workshop installation guide](https://github.com/James-Yu/LaTeX-Workshop/wiki/Install) documents this requirement.
5. Close and reopen VS Code and any terminals so they pick up the updated `PATH`.
6. Open a new PowerShell terminal and check:

   ```powershell
   pdflatex --version
   latexmk -v
   perl -v
   ```

Each command should print version information. If `latexmk` is missing, install the `latexmk` package through **MiKTeX Console > Packages**, then restart VS Code.

### macOS

1. Install [Visual Studio Code](https://code.visualstudio.com/download).
2. Download the full [MacTeX distribution](https://tug.org/mactex/mactex-download.html) and run its installer. Full MacTeX includes the packages needed by this deck and is simpler for beginners than the smaller BasicTeX distribution. Check the download page for supported macOS versions.
3. Close and reopen VS Code and Terminal after installation.
4. In a new Terminal window, check:

   ```sh
   pdflatex --version
   latexmk -v
   perl -v
   ```

If you already have a working MacTeX or TeX Live installation, you can use it without installing another distribution.

If the commands cannot be found, check the standard MacTeX executable location:

```sh
/Library/TeX/texbin/pdflatex --version
/Library/TeX/texbin/latexmk -v
```

If those work, ensure `/Library/TeX/texbin` is on your `PATH`. For the default zsh shell, add this line to `~/.zprofile`, then reopen Terminal and VS Code:

```sh
export PATH="/Library/TeX/texbin:$PATH"
```

### Install the VS Code extension on either platform

1. Open **Extensions** in VS Code.
2. Search for **LaTeX Workshop** by **James Yu**, extension ID `James-Yu.latex-workshop`.
3. Install it. [Extension page](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop).

The extension does not install MiKTeX or MacTeX for you.

## 2. Open the project

Download and extract the project folder, or clone it if it is available on GitHub. Git is optional when downloading a ZIP.

In VS Code, select **File > Open Folder** and choose the folder containing `idea-presentation.tex`. Open that file in the editor. Opening the whole folder makes workspace settings available.

## 3. Enable rendering on save

Create a `.vscode` folder inside the project, then create `.vscode/settings.json` with the following content. If that file already exists, merge these settings into its existing JSON object.

```json
{
  "latex-workshop.latex.autoBuild.run": "onSave",
  "latex-workshop.latex.recipe.default": "first",
  "latex-workshop.latex.outDir": "%DIR%",
  "latex-workshop.view.pdf.viewer": "tab",
  "latex-workshop.latex.recipes": [
    {
      "name": "latexmk (PDF)",
      "tools": ["latexmk-pdf"]
    }
  ],
  "latex-workshop.latex.tools": [
    {
      "name": "latexmk-pdf",
      "command": "latexmk",
      "args": [
        "-pdf",
        "-synctex=1",
        "-interaction=nonstopmode",
        "-halt-on-error",
        "-file-line-error",
        "-outdir=%OUTDIR%",
        "%DOC%"
      ]
    }
  ]
}
```

Saving a `.tex` file now starts the PDF build. `latexmk` runs the necessary compilation passes to resolve citations and slide numbering. The PDF is written beside the source. These settings follow LaTeX Workshop's [compilation documentation](https://github.com/James-Yu/LaTeX-Workshop/wiki/Compile).

This README provides the settings to create; the `.vscode/settings.json` file is not included in the current project.

## 4. Build and preview the slides

1. Open `idea-presentation.tex`.
2. Open the Command Palette with **Ctrl+Shift+P** on Windows or **Cmd+Shift+P** on macOS.
3. Run **LaTeX Workshop: Build LaTeX project** for the first build.
4. Run **LaTeX Workshop: View LaTeX PDF file** to open the PDF beside the source.
5. Edit a slide and save with **Ctrl+S** or **Cmd+S**. After a successful build, the integrated PDF preview refreshes.

See the [PDF viewer documentation](https://github.com/James-Yu/LaTeX-Workshop/wiki/View) for preview and source-navigation options.

The first build can take longer while MiKTeX downloads missing packages. Later builds generally reuse installed packages. To present, open `idea-presentation.pdf` in a PDF reader and use its full-screen or presentation mode. Share that PDF when the audience only needs to view the slides.

## 5. Build from a terminal

Open a terminal in the project folder. These commands work in macOS Terminal and Windows PowerShell when the LaTeX tools are on `PATH`.

Build once:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error -synctex=1 idea-presentation.tex
```

Rebuild whenever the source changes:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error -synctex=1 -pvc -view=none idea-presentation.tex
```

Leave the watcher running while editing and stop it with **Ctrl+C**. It updates the PDF on disk without launching a viewer. External viewers may require a refresh or reopening the file.

Use either VS Code's automatic build or the terminal watcher. Running both can start competing builds of the same files.

If `make` is installed, the supplied Makefile provides equivalent shortcuts:

```sh
make pdf
make watch
```

`make` is optional and is not normally available in Windows PowerShell. Use the direct `latexmk` commands there; you do not need to install `make` just for this project.

## Editing slides and references

Each slide is a `frame` environment in `idea-presentation.tex`. Diagrams use TikZ, and equations use standard LaTeX math syntax.

The references are self-contained in the `thebibliography` environment at the end of the source. Cite them using these existing keys:

```latex
\cite{liu2026crysjepa}
\cite{MatterGen2025}
\cite{yang2024mattersim}
```

No external `.bib` file or BibTeX step is required for this deck. Keep `diagrams/slide-architectures.tex` alongside the main source at its existing relative path when sharing or building the presentation. When adding a reference, create a matching `\bibitem{key}` in the bibliography and use `\cite{key}` on the relevant slide.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| `latexmk` or `pdflatex` not found, or `spawn latexmk ENOENT` | Run the version checks in a new terminal. Install the missing tool, check `PATH`, and fully restart VS Code. |
| MiKTeX reports that Perl is missing | Install Strawberry Perl, restart VS Code, and confirm `perl -v` works. |
| A `.sty` or `.cls` file is missing | On Windows, install the required package through MiKTeX Console. On macOS, use TeX Live Utility to install missing packages. This deck uses Beamer, PGF/TikZ, booktabs, AMS math packages, and xcolor. |
| The PDF does not change after saving | Check that the workspace settings exist, the source was saved, and the build succeeded. Open **View > Output** and select LaTeX Workshop to inspect the build output. |
| A PDF write or permission error occurs on Windows | Close external PDF readers that may lock the output file and retry, or use the integrated preview. |
| References show `?` | Check that each citation key matches a `\bibitem` key exactly, then rebuild with `latexmk`. A single manual pdfLaTeX pass may leave citations unresolved. |
| The build stops after an edit | Read the first error in `idea-presentation.log`, fix it, and save again. A failed build may leave an older PDF visible. |
| `make` is not recognized | Use the direct `latexmk` command from the terminal section. |

To remove auxiliary files and rebuild after stale build state, stop any terminal watcher first, then run:

```sh
latexmk -c idea-presentation.tex
latexmk -pdf -interaction=nonstopmode -halt-on-error -synctex=1 idea-presentation.tex
```

The lowercase `-c` removes temporary compilation files while retaining the PDF. Avoid uppercase `-C` if you want to keep the rendered PDF.

## Files to keep when sharing

Keep the `.tex` source, README, Makefile, and any figures or other source assets you add. Include `.vscode/settings.json` if you want collaborators to inherit your build settings. The PDF can also be included so readers can view the slides without installing LaTeX.

Generated files such as `.aux`, `.log`, `.nav`, `.snm`, `.toc`, `.out`, `.fls`, `.fdb_latexmk`, and `.synctex.gz` are build artifacts and usually do not need to be committed. The `tmp/` directory contains local rendering checks and is not needed to build the slides.
