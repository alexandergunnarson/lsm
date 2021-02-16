# lsm

We tried a few approaches here.

## Approach 1 — `pdfseparate` + HTML index page (current approach)

```shell
mkdir lsm && pdfseparate lsm.pdf ./lsm/page-%d.pdf
```

## Approach 2 — `pdf2htmlEX`

To split the LSM PDF, we did:

```shell
alias pdf2htmlEX='docker run -ti --rm -v `pwd`:/pdf bwits/pdf2htmlex pdf2htmlEX'

pdf2htmlEX --embed cfijo --split-pages 1 --dest-dir lsm --page-filename page-%d.html lsm.pdf
```
