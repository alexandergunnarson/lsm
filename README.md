# lsm

We tried a few approaches here.

# Favored approach — `pdfseparate` + Java Aspose PDF + HTML index page (current approach)

Code contained elsewhere.

## Discarded approaches

### `pdf2htmlEX`

This results in great performance and formatting, but the HTML comes out garbled so it's hard for
Google Translate to make sense of it.

```shell
echo '
#!/bin/bash
docker run -ti --rm -v `pwd`:/pdf bwits/pdf2htmlex pdf2htmlEX "$@"
' > /usr/local/bin/pdf2htmlEX && chmod +x /usr/local/bin/pdf2htmlEX

# All-in-one — High quality visual, and medium-low translatability
pdf2htmlEX --embed cfijo --split-pages 1 --dest-dir lsm --page-filename page-%d.html lsm.pdf

# Individual page — High quality visual, but low translatability
pdf2htmlEX page-1.pdf page-1.html
pdf2htmlEX --correct-text-visibility 1 page-1.pdf page-1.html

# Individual page — High quality visual, and medium-low translatability
pdf2htmlEX --embed cfijo --dest-dir page-1 page-1.pdf page-1.html
pdf2htmlEX --embed cfijo --dest-dir page-9 page-9.pdf page-9.html
pdf2htmlEX --correct-text-visibility 1 --embed cfijo --dest-dir page-9 page-9.pdf page-9.html
```

### `mupdf`

Images aren't preserved.

```shell
brew install mupdf
mutool convert -o page-1.html page-1.pdf
```

### `pdftohtml`

Low quality visual, but good translatability.

```shell
pdftohtml page-1.pdf page-1.html
```

### Unilex transcript

This is a very immature tool that doesn't do a good job.

```shell
git clone https://github.com/fmalina/unilex-transcript
cd unilex-transcript
pip3 install -r requirements.txt

echo
<<EOF
DATA_DIR = '/Users/alexandergunnarson/Creating/Code/alexandergunnarson/lsm/pdfs/test/'
PDF_DIR  = DATA_DIR+'PDF'
HTML_DIR = DATA_DIR+'HTML'
# used by ttf.py to access full original fonts to compare with the broken ones
FULL_FONTS_PATH  = '/Users/f/SITES/etc/ttf'
BULLETS =  ('', '')
REMOVE_BEFORE = ()
REPLACE_AFTER = ()
EOF | tee config.py

# Adapted from https://github.com/fmalina/unilex-transcript/blob/51a14361a16b4d9c6e565e1ed5f742ac2be93dba/pdf2html.py
docker run -ti --rm -v /Users/alexandergunnarson/Creating/Code/alexandergunnarson/lsm/pdfs/test:/pdf bwits/pdf2htmlex pdf2htmlEX \
  --embed-external-font 0 \
  --external-hint-tool ttfautohint \
  --process-nontext 0 \
  --embed cfijo \
  --dest-dir ./HTML/page-9 \
  PDF/page-9.pdf \
   page-9.html

./transcript.py
```
