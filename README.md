# lsm

To split the LSM PDF, we did:

```shell
alias pdf2htmlEX='docker run -ti --rm -v `pwd`:/pdf bwits/pdf2htmlex pdf2htmlEX'

pdf2htmlEX --embed cfijo --split-pages 1 --dest-dir lsm --page-filename page-%d.html lsm.pdf
```
