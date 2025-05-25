# PhD Thesis Description

Shield: [![CC BY 4.0][cc-by-shield]][cc-by]

This is the dissertation file for my Ph.D. at KNU, submitted in 2025. The dissertation investigates data generation methods for combinatorial problems using large language models.

I wrote the dissertation purely in Overleaf using the [vakthesis](https://imath.kiev.ua/~baranovskyi/tex/vakthesis/) and [mon2017dev](https://imath.kiev.ua/~baranovskyi/tex/vakthesis/support/mon2017dev/) templates, which follow the recommendations of VAK Ukraine on the guidelines for thesis preparation.

The repository contains the dissertation in compliance with the PDF/A format required for submission by the university.

You can find the working template of my thesis on Overleaf [here](https://www.overleaf.com/read/kwdpjymbrsxw#3fbfc7).

## PDF/A File Preparation

To prepare the PDF/A file, one can use online converters. However, it might degrade the quality of the output file ([Source](https://www.mathstat.dal.ca/~selinger/pdfa/)).

If you prepared your dissertation in LaTeX, you can do this with the help of the [pdfx](https://ctan.org/pkg/pdfx) package.

At this point, I downloaded my thesis and opened it via VS Code with the [MacTex](https://www.tug.org/mactex/) package for compilation installed. I tried various ways of compiling the file via Overleaf but, unfortunately, it wasn't compiling things right (switching to `XeLaTeX` or `LuaLaTeX` didn't help either due to usage of Ukrainian in the file).

### Preparing Images for PDF/A Compliance

In my Overleaf template, I had images in several formats including `.svg`, `.png`, and `.pdf` (a convenient way to create images through Google Slides, where you export them in several formats, including SVG), and the trickiest part was to keep images aligned with the guidelines of a valid PDF/A format (non-transparent, etc.).

First, I converted everything into a `.pdf` format.

#### `.svg` -> `.pdf`: ([inkscape](https://inkscape.org/release/inkscape-1.4.2/))

```
cd /path/to/images/folder
for f in *.svg; do
  base="${f%.svg}"
  inkscape "$f" \
    --export-area-drawing \
    --export-background=white \
    --export-type=pdf \
    --export-filename="${base}.pdf"
done
```

#### `.png` -> `.pdf`: ([magick](https://imagemagick.org/script/download.php))

```
cd /path/to/images/folder
for f in *.png; do
  magick "$f" -background white -alpha remove -alpha off \
    "flat/${f%.png}_flat.pdf"
done
```

Then, cleaned with

```
cd /path/to/svg/folder
mkdir cleaned_pdfs
for file in *.pdf; do
  gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite \
     -dPDFA=1 -dPDFACompatibilityPolicy=1 \
     -dPDFSETTINGS=/prepress \
     -sColorConversionStrategy=RGB \
     -sProcessColorModel=DeviceRGB \
     -dEmbedAllFonts=true \
     -dDownsampleColorImages=false \
     -sOutputFile=cleaned_pdfs/"$file" "$file"
done
```


This resulted in having a valid set of images (took a bit more space than the original ones).

### Metadata

To attach metadata to your output PDF file, insert the following code in the preamble:

```
\begin{filecontents*}[overwrite]{\jobname.xmpdata}
  \Title{Dissertation}
  \Author{Andrii Nikolaiev}
  \Language{uk-UA}
  \Subject{Development of Data Synthesis and Mathematical Combinatorial Problem Generation Methods Using Large Language Models}
  \Keywords{Artificial intelligence, automated theorem proving, natural language
processing, large language models, machine learning, mathematical problems}
\end{filecontents*}
```


After successful compilation, there will be a file `Dissertation.xmpdata` generated with your details.

## PDF Validation

There are several ways to validate the file.

It can be done with the [verapdf](https://verapdf.org/software/) LaTeX lib or via online validators ([option 1](https://www.pdf-online.com/osa/validate.aspx), [option 2](https://www.pdfforge.org/online/en/validate-pdfa), [option 3](https://avepdf.com/pdfa-validation)).

The result should show that the validation is successful and mention the compliance, e.g. "pdfa-1b".

Additionally, one can check the metadata of the file via Adobe Reader (File > Document Properties > Description) or online (e.g. [here](https://apitemplate.io/pdf-tools/extract-pdf-metadata/)).

## Todo

[ ] Publish LaTeX template to the Overleaf Gallery.  
[ ] Add link to images on Google Slides.

## License

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
