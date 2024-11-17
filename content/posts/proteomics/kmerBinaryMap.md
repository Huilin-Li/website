+++
title = "Encoding Protein Sequences into Integer Representations Using K-mer Binary Mapping"
bookToc=true
+++

- `seq="MKTLGEFIVEKQH"`, `k=4`.
- Kmers: `['MKTL', 'KTLG', 'TLGE', 'LGEF', 'GEFI', 'EFIV', 'FIVE', 'IVEK', 'VEKQ', 'EKQH']`
- Encodings: `[5057, 15385, 49556, 6470, 37986, 17954, 25124, 8771, 9268, 17226]`
- Check:
| Kmer   | K-mer Binary Mapping  | Encoding    | Check    | 
| ---    | -----------           | ----------- | ----------- |
| "MKTL" | 0001 0011 1100 0001   | 5057        | ✔️|
| "KTLG" | 0011 1100 0001 1001   | 15385       | ✔️|
| "TLGE" | 1100 0001 1001 0100   | 49556       | ✔️|
| "LGEF" | 0001 1001 0100 0110   | 6470        | ✔️ |
| "GEFI" | 1001 0100 0110 0010   | 37986       | ✔️|
| "EFIV" | 0100 0110 0010 0010   | 17954       |✔️ |
| "FIVE" | 0110 0010 0010 0100   | 25124       | ✔️|
| "IVEK" | 0010 0010 0100 0011   | 8771        | ✔️|
| "VEKQ" | 0010 0100 0011 0100   | 9268        |✔️|
| "EKQH" | 0100 0011 0100 1010   | 17226       | ✔️|

- Code:
<script src="https://gist.github.com/Huilin-Li/da371c7da55c1d504fddf854c116887b.js"></script>