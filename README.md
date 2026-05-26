# Genova 1507 — La Rivolta contro la Francia

One-shot Forged in the Dark bilingue (IT/EN) · 4 tavoli · 16–20 giocatori
Genova, 1507. Le quattro casate decidono il destino della Superba sotto il
governatore francese Gian Giacomo Trivulzio.

## Struttura

```
genova-1507/
├── main.tex                    ← versione italiana (lualatex)
├── titlepage.tex
├── preamble/
│   ├── packages.tex            font, pacchetti, comandi custom
│   └── indexstyle.ist          stile per makeindex
├── sections/
│   ├── 01_intro.tex            contesto storico + regole globali
│   ├── 02_playbooks.tex        i 4 playbook di casata
│   ├── 03_characters.tex       schede PG Casa Doria (complete)
│   ├── 04_appendice.tex        reference GM
│   └── 05_storia_it.tex        contesto storico esteso
├── characters/
│   ├── pg_doria.tex            5 PG Casa Doria
│   ├── pg_spinola.tex          5 PG Casa Spinola
│   ├── pg_fieschi.tex          5 PG Casa Fieschi
│   ├── pg_grimaldi.tex         5 PG Casa Grimaldi
│   └── pg_*_extra.tex          schede aggiuntive
├── shared/
│   └── 06_ohm_appendix.tex     dati OpenHistoryMap (RDF/Turtle)
├── en/                         ← versione inglese
│   ├── main_en.tex             → lualatex main_en.tex (dalla cartella en/)
│   ├── preamble_en.tex
│   ├── sections/01_intro_en.tex … 05_history_en.tex
│   └── characters/pg_*_en.tex
└── .github/workflows/build-pdf.yml   CI: build PDF su ogni push e release
```

## Compilazione locale

```bash
# Versione italiana
lualatex main.tex
makeindex -s preamble/indexstyle.ist main.idx
lualatex main.tex
lualatex main.tex                # 3 passate per indice + riferimenti

# Versione inglese
cd en
lualatex main_en.tex
makeindex -s ../preamble/indexstyle.ist main_en.idx
lualatex main_en.tex
lualatex main_en.tex
```

## Requisiti

- **LuaLaTeX** (TeX Live 2022+ o MiKTeX 22+)
- Font in `texlive-fonts-extra`: Linux Libertine O, Linux Biolinum O, DejaVu Sans Mono
- Pacchetti: geometry, xcolor, tcolorbox, fontawesome5, titlesec, booktabs,
  tabularx, fancyhdr, hyperref, imakeidx, multicol, pgffor, enumitem, babel,
  microtype, amssymb, dashrule

## Build automatica

Una GitHub Actions sotto `.github/workflows/build-pdf.yml`:
- Compila entrambe le edizioni (`main.tex` + `en/main_en.tex`) su `texlive/texlive:latest`
- Pubblica `genova_1507_it.pdf` e `genova_1507_en.pdf` come artefatti su ogni push
- Su `release: published`, li allega al GitHub release

## Coordinatore bot

Il bot Telegram [coordinatore](https://github.com/sirmmo/coordinatore) gestisce
la parte condivisa multi-tavolo (clock globale, Momenti di Congiunzione,
Colpo Globale). Genova 1507 ha la sua game config sotto
[games/genova-1507.yaml](https://github.com/sirmmo/coordinatore/blob/main/games/genova-1507.yaml)
in quel repo.

In Telegram:
```
/open genova-1507 full
/join doria              (o spinola / fieschi / grimaldi)
/begin
/score doria 2           ← Casa Doria completa il Colpo Riservato (+2 clock)
/moment vespri_genovesi  ← invia il segnale «campane di San Lorenzo»
```

## Annotazioni rpg-schema

I commenti `% @rpg-schema: ...` nei file `.tex` sono triple RDF/Turtle inline,
invisibili nel PDF. Per estrarle:

```bash
grep -rh "@rpg-schema:" --include="*.tex" . | sed 's/.*@rpg-schema: //'
```

## Crediti

- **Sistema**: Forged in the Dark — John Harper / One Seven Design (CC BY 3.0)
- **Dati storici**: figure di dominio pubblico (Andrea Doria, Gian Giacomo
  Trivulzio, Luigi XII). PG e PNG sono finzionali.
- **Ontologia RPG**: [rpg-schema.org](https://rpg-schema.org)
- **Dati geografici**: [OpenHistoryMap](https://openhistorymap.org)
- **Licenza**: CC BY 3.0
