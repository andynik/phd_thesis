# PhD Thesis Description

[![CC BY 4.0][cc-by-shield]][cc-by]

This is the dissertation for my Ph.D. at KNU, submitted in 2025. The Ph.D. research investigates data generation methods for mathematical combinatorial problems using large language models.

I wrote the dissertation in Overleaf using the [vakthesis](https://imath.kiev.ua/~baranovskyi/tex/vakthesis/) and [mon2017dev](https://imath.kiev.ua/~baranovskyi/tex/vakthesis/support/mon2017dev/) templates, which follow the recommendations of VAK Ukraine on the guidelines for thesis preparation.

The repository contains the dissertation files in compliance with the PDF/A format required for submission by the university.

You can find the template (**not** a PDF/A standard) of my thesis on Overleaf [here](https://www.overleaf.com/read/kwdpjymbrsxw#3fbfc7). 

## PDF/A File Preparation

To prepare the PDF/A file, one can use online converters. However, it might degrade the quality of the output file ([Source](https://www.mathstat.dal.ca/~selinger/pdfa/)).

I tried various ways of compiling the file via Overleaf but, unfortunately, it wasn't generating a valid PDF/A file (switching to `XeLaTeX` or `LuaLaTeX` didn't help due to usage of Ukrainian text in the file). At this point I decided to do futher conversion locally.

To create a valid PDF/A copy of the dissertation, please, follow the steps:

1. Download Overleaf project and opened it via VS Code with pdflatex compiler installed from [MacTex](https://www.tug.org/mactex/).

2. To create a PDF/A file install the [pdfx](https://ctan.org/pkg/pdfx) package and include the package in preambule.

There are several PDF/A options [available](https://mirror.math.princeton.edu/pub/CTAN/macros/latex/contrib/pdfx/pdfx.pdf). I decided to go with the most common one: **a-1b**.

To compile the tex file from the terminal run:

``` bash
pdflatex Dissertation.tex
```

This will generate an output PDF file. To check whether it claims compliance with PDF/A standard, please, read futher.

### Images Compliance

In my Overleaf template, I had images in several formats including `.svg`, `.png`, and `.pdf`. I had to keep images aligned with the guidelines of a valid PDF/A-1b format, which requires non-transparent images.

> **Note:** Google Slides is a convinient way to prepare bais images. You can export them in several formats, including SVG.

First, I converted everything into a `.pdf` format.

#### `.svg` -> `.pdf`: ([inkscape](https://inkscape.org/release/inkscape-1.4.2/))

``` bash
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

It will make a `.pdf` copy of the correspondent files.

#### `.png` -> `.pdf`: ([magick](https://imagemagick.org/script/download.php))

``` bash
cd /path/to/images/folder
for f in *.png; do
  magick "$f" -background white -alpha remove -alpha off \
    "flat/${f%.png}_flat.pdf"
done
```

It will make a `.pdf` copy of the correspondent files.

Then, clean the files with

``` bash
cd /path/to/images/folder
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

To attach metadata to your output PDF file, insert the following `filecontents*` in the preamble:

``` latex
\pdfminorversion=4 % for compliance with 1a-b

\begin{filecontents*}[overwrite]{\jobname.xmpdata}
  \Title{Dissertation}
  \Author{Andrii Nikolaiev}
  \Language{uk-UA}
  \Subject{Development of Data Synthesis and Mathematical Combinatorial Problem Generation Methods Using Large Language Models}
  \Keywords{Artificial intelligence\sep automated theorem proving\sep natural language processing\sep large language models\sep machine learning\sep mathematical problems
\end{filecontents*}
% it's important to use \sep for Keywords

\documentclass[type=phd]{mon2017dev}
\usepackage[a-1b]{pdfx}

% the rest of the file goes here
```

After successful compilation, there will be a file `Dissertation.xmpdata` generated with your details.

Additionally, one can check the metadata of the file via Adobe Reader (File > Document Properties > Description) or online (e.g. [here](https://apitemplate.io/pdf-tools/extract-pdf-metadata/)).

### Validation

There are several ways to validate the file.

It can be done with the [verapdf](https://verapdf.org/software/) LaTeX lib or via online validators ([option 1](https://www.pdf-online.com/osa/validate.aspx), [option 2](https://www.pdfforge.org/online/en/validate-pdfa), [option 3](https://avepdf.com/pdfa-validation)).

``` bash
verapdf Dissertation.pdf
```

The result should show that the validation is successful and recognise the compliance format, e.g. PDF/A-1B.

If you open the PDF in Adobe Reader you will see the banner at the top saying:

> This file claims compliance with the PDF/A standard and has been opened read-only to prevent modification.

You can find minimal working example and more on working with PDF/A for LaTeX [here](https://webpages.tuni.fi/latex/pdfa-guide.pdf) (Sep 2023).

## Todo
[+] Add links for libs and guides used.  
[+] Add license.  
[ ] Publish LaTeX template to the Overleaf Gallery.  

## License

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
