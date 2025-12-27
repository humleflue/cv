# Repository Guidelines

## Project Structure & Module Organization
- Root contains the LaTeX sources for the Danish and English CVs: `CV_Lasse_D_Skaalum.tex` and `CV_English_Lasse_D_Skaalum.tex`.
- Generated artifacts (PDF, .aux, .log, .out, .synctex.gz, .fls, .fdb_latexmk) are also stored at the repo root.
- Assets are currently minimal; `pb.jpg` is referenced by the LaTeX sources.
- CI lives in `.github/workflows/ci.yml` and compiles both PDFs on push to `main`.

## Build, Test, and Development Commands
- Build Danish CV: `latexmk -pdf CV_Lasse_D_Skaalum.tex` (produces `CV_Lasse_D_Skaalum.pdf`).
- Build English CV: `latexmk -pdf CV_English_Lasse_D_Skaalum.tex` (produces `CV_English_Lasse_D_Skaalum.pdf`).
- Clean LaTeX artifacts: `latexmk -c` (removes auxiliary files, keeps PDFs).
- CI build mirrors this with `xu-cheng/latex-action@v2` in GitHub Actions.

## Coding Style & Naming Conventions
- Indentation: tabs are used in the LaTeX sources; keep existing indentation style.
- Keep section and command names consistent with existing `\cv*` macros in the `.tex` files.
- Prefer Danish text in `CV_Lasse_D_Skaalum.tex` and English text in `CV_English_Lasse_D_Skaalum.tex`.

## Testing Guidelines
- No automated tests are defined beyond LaTeX compilation.
- Validate changes by compiling the relevant `.tex` file and reviewing the resulting PDF.

## Commit & Pull Request Guidelines
- Commit messages are short, direct, and in present tense (e.g., “Add Trifork København”, “Update CV”).
- If opening a PR, include a short summary and attach or link the updated PDF(s).
- Note any visual/layout changes and which CVs were rebuilt.

## Configuration & Release Notes
- Releases are created by CI on pushes to `main` and include both PDF assets.
- Keep file names stable so the workflow can upload the expected artifacts.
