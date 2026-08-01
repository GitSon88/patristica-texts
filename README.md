# patristica-texts

Public domain texts for **Patristica** — an Early Church Reference Guide.

This repository stores clean, public-domain English translations of early Christian writings, apocrypha, pseudepigrapha, creeds, councils, pagan/Jewish sources, and related texts so they can be fetched via raw GitHub URLs by the Patristica web application.

## Structure

```
patristica-texts/
├── church-fathers/
├── apocrypha/
├── pseudepigrapha/
├── nt-apocrypha/
├── creeds-and-councils/
├── pagan-sources/
├── gnostic-texts/
├── manifest.json
└── README.md
```

## File Format

Every text file follows:

1. Title (line 1)
2. Translation credit (line 2) — e.g. `Translation: Roberts-Donaldson 1885, Public Domain`
3. Source URL (line 3)
4. Blank line
5. Full text beginning on line 5

Chapter/section numbers and paragraph breaks are preserved. No HTML or site commentary.

## Sources Used

- Primary: [Early Christian Writings](https://www.earlychristianwritings.com) (Roberts-Donaldson, Lightfoot, Kirsopp Lake, etc. — public domain)
- Apocrypha PDFs from scriptural-truth.com and Internet Archive where confirmed public domain (KJV 1611, R.H. Charles 1917, etc.)
- Additional public-domain sources via Internet Archive when needed

**Only confirmed public-domain material is included.** Copyrighted modern translations and site-specific commentary are excluded.

## Raw URL Format

```
https://raw.githubusercontent.com/GitSon88/patristica-texts/main/[folder]/[filename].txt
```

Example:
```
https://raw.githubusercontent.com/GitSon88/patristica-texts/main/church-fathers/didache.txt
```

## Manifest

See `manifest.json` for the complete inventory (path, title, category, translation, source, word count, public-domain confirmation).

## Status

This repository is being populated systematically. Short foundational texts are prioritized first. Longer multi-book works (e.g. Irenaeus *Against Heresies*, Augustine *City of God*, Origen, etc.) will be added in stages after careful cleaning.

Contributions of verified public-domain clean texts are welcome via pull request.

---

Built for Patristica. Soli Deo Gloria.
