# Resume

This is the actual resume I share with HR and talent acquisition staff. As I mention in it, it is done entirely in LaTeX.

## Disclaimer

Updating this resume does not mean I am actively looking for a new job. I maintain it as a living document so it stays accurate and ready when needed.

## Generating the PDF

You must use XeLaTeX or LuaTeX as the compiler. I personally use XeLaTeX, and it is also used by this repo to build it automatically.

The Ubuntu font family has to be installed in order for the build to work. The CI job downloads the static Ubuntu 0.83 faces from `assets.ubuntu.com` into `fonts/`; locally, `fonts/` is a symlink to the installed copy.

```sh
xelatex -interaction=nonstopmode Resume.tex
xelatex -interaction=nonstopmode Resume.tex   # run it twice: LaTeX needs a second pass
```

An example of how it's built is available in the Github Actions workflow file (`.github/workflows/main.yml`).

## Download a prebuilt version

You can download the latest prebuilt version on the Releases section of the repository. Every time I push a new version, Github Actions will build it automatically.

## Design decisions

These are deliberate. Please do not "simplify" them without checking the reason.

- **No photo.** A photo adds no signal for international and remote applications: anti-bias screening policies penalise it, and some ATS parsers auto-reject CVs that carry one. The image file was removed from the repo, along with the `\showphoto` conditional that used to place it.
- **Two pages, 10pt.** 11pt pushes the content onto a third page with only a few lines on it. If a change overflows to three pages, cut content instead of shrinking the margin, the font or the spacing.
- **Ragged right, hyphenation off.** Justified text broke words mid-way ("con-tact", "ser-vices") and split the exact keywords an ATS may be matching on.
- **Fonts are loaded by filename, not by family name.** Distros ship Ubuntu as a variable font. Selecting it by family name silently degrades every `\textbf` to regular weight, and `xdvipdfmx` cannot embed it at all. `\IfFileExists{fonts/Ubuntu-R.ttf}` also works where fontconfig cannot see a project-local directory.
- **Section headings keep a vertical gap above them.** The gap is not cosmetic: without it, `pdftotext` merges a heading into the paragraph before it, which is how some ATS parsers read the file. The value is tuned to the smallest one that still extracts cleanly.
- **Every number is real.** No invented metrics and no hedging words ("various", "numerous") standing in for one. If a figure is unknown, the bullet stands on scale or scope instead.

## Checking the result

The PDF text layer is the view an ATS gets. Check it after any layout change:

```sh
pdftotext -layout Resume.pdf -
```

Confirm two things: the section headings land on their own lines, and the page count is still two.

## Flags

- `\showbirthdate` (default off) — adds a date of birth to the contact line. Off by default: no signal, and a liability abroad.
- `\reviewdate` — the "Last reviewed" footer. Update it on real content edits.
