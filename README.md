# Synta

Synta [^1] è uno strumento a linea di comando per svolgere svariate operazioni
sul formato di definizione regolare usato internamente a [csunibo](https://github.com/csunibo)
per vincolare le nomenclature dei file. Con Synta è possibile:
- Verificare la validità di un file `.synta`
- Convertire un file `.synta` in una espressione regolare
- Convertire un file `.synta` in un file JSON che descrivre i contenuti dei file.
    Il file JSON generato è descritto dallo schema JSON disponibile [qui](TODO).

# Definizione del linguaggio

Il linguaggio Synta è definito dalla seguente BNF:
```bnf
<upperletter> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" |
                  "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T" |
                  "U" | "V" | "W" | "X" | "Y" | "Z"
<lowerletter> ::= "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" |
                  "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" |
                  "u" | "v" | "w" | "x" | "y" | "z"
<letter>      ::= <lowerletter> | <upperletter> | ":" | ";" | "," | "." | "-" | "_"
<word>        ::= <letter> <word> | <letter>

<digit>       ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
<number>      ::= <digit> <number> | <digit>
<alphanum>    ::= <lowerletter> | <digit>
<text>        ::= <alphanum> <text> | " " <text> | <alphanum>

<comment>     ::= ";" <text> "\n"
<id>          ::= <lowerletter> <id> | <lowerletter>
<def>         ::= <id> " = " <regexp> "\n"
<commdef>     ::= <comment> <commdef> | <comment> <def>
<commdefs>    ::= <commdef> <commdefs> | <commdef>

<segment>     ::= "-" <id> | "(-" <id> ")?"
<join>        ::= <segment> <join> | <segment>
<main>        ::= "> " <join> "." <id> "\n"

<language>    ::= <commdefs> <main>
```
dove `<regexp>` è una valida sintassi che esprime una espressione regolare.

## Esempi

```
; La tipologia della prova
tipo = scritto|orale
; Una qualunque parola alfanumerica
data = \d{4}-\d{2}-\d{2}
; Una qualunque parola alfanumerica
extra = (\w|\d)+
; Estensione del file. Possibili valori:
; - txt, tex, md, pdf, doc, docx
ext = txt|tex|md|pdf|doc|docx

> tipo-data(-fila)?-extra.ext
```

# Origine del nome

[^1] è uno dei top-10 nomi suggeriti da chat gpt. Abbiamo usato il prompt:
> suggest me a short name for a tiny parser command line utility. the name must not be made up of two or more words
