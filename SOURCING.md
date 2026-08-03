# Sourcing notes — how texts get into this repository

Everything here was extracted from real files on disk, never fetched through a
web tool. That is deliberate. The fetch path available to these sessions routes
pages through a summarising model, which silently drops verses and renumbers
chapters. It was tried once, on 1 Enoch, and produced a text with three chapters
missing verses and one renumbered before the attempt was abandoned. The rule
since: **download the file, then extract from bytes.**

The working pattern is: get a plain-text or EPUB file onto disk → session reads
it directly → extract, verify the first and last line of every file, register in
`manifest.json` → push.

## Provenance screen — read this before adding anything

Safe: Ante-Nicene Fathers (Roberts–Donaldson, 1885), Nicene and Post-Nicene
Fathers (Schaff, 1886–1900), the Oxford Library of the Fathers (1838–1885),
Project Gutenberg, Wikisource, and any scan published before 1930.

**Not safe, and easy to confuse with the above:**

- *The Fathers of the Church* (Catholic University of America Press, 1947– ) —
  in copyright. The name is very close to the 19th-century series.
- *Ancient Christian Writers* (Newman/Paulist, 1946– ) — in copyright.
- *Luther's Works*, American Edition (Concordia/Fortress, 1955–86) — in
  copyright. Distinct from the Holman/Philadelphia edition (Jacobs and Spaeth,
  1915–32), which is public domain and is what this repo carries.
- Loeb, Penguin, Oxford World's Classics, Hendrickson reissues carrying new
  introductions.

A title alone never establishes provenance. Check the named translator and the
edition year every time. Two real near-misses this project has already had: a
Luther volume that turned out to be the public-domain Holman edition rather than
the copyrighted Concordia one (checked via its Jacobs/Spaeth attribution), and a
scan volume carrying a **modern article appended by the electronic edition's
editor**, which had to be excluded — see the note in the commit history.

## Known gaps, and what would close them

**Augustine, *Confessions*.** Absent from the 38-volume scan this repo was built
from. The classic public-domain English translation is **E. B. Pusey's, 1838**,
in the Oxford Library of the Fathers. Widely available on Project Gutenberg and
the Internet Archive. This is the most valuable single gap remaining and the
easiest to close — a Gutenberg plain-text download would do it.

**Justin Martyr, *Second Apology* and *Dialogue with Trypho*.** Present in the
scan but interleaved with the *Discourse to the Greeks*, with chapter numbering
that jumps between works. No clean seam exists. Both are in ANF vol. 1 on CCEL
and Wikisource as separate single-work pages, which would extract cleanly.

**Reformation confessional documents.** Most are now in. Still missing: Cole's
1823 *Bondage of the Will* and Wace and Buchheim's *First Principles of the
Reformation* (1883).

**Schleitheim Confession (1527).** Still absent, and the reason is now specific
rather than general. Four English or foreign-language editions have been
assessed and rejected: Wenger's (*Mennonite Quarterly Review* XIX.4, 1945),
Yoder's (Herald Press, 1977), and a 1975 French edition from the École Biblique
Mennonite Européenne — all in copyright — and the propaganda edition noted
below is a separate matter.

A public-domain English translation **does** exist and is on disk: W. J.
McGlothlin, *Baptist Confessions of Faith* (American Baptist Publication
Society, Philadelphia, 1911), pages 2–9, which renders Zwingli's Latin version
of the seven articles. The obstacle is mechanical, not legal: in the archive.org
scan `baptistconfessio00mcgl`, **pages 6 and 7 are too faint to read.** The
scan's own text layer returns gibberish for those two pages, three separate OCR
preprocessing approaches failed on them, and the glyphs are too degraded to
transcribe by eye without guessing. Those two pages carry the end of Article V
and the whole of Article VI — the article on the sword, which is the most
quoted passage in the document.

The fix is cheap: **a different scan of the same 1911 book.** HathiTrust and
Google Books both hold copies. Any copy whose pages 6–7 are legible closes this
gap in minutes; the rest of the volume's text layer is clean. Publishing the
confession with its central article missing was considered and rejected — a
confession with a hole where the pacifism article belongs is worse than no file.

**The London Baptist confessions — approved for inclusion, blocked by the same
scan.** The First London (1644) and Second London (1677/1689) confessions are in
the same McGlothlin volume, pages 171–200 and 220–289. Both are unambiguously
public domain. Four extraction routes were tried and all four failed:

1. *The PDF's own text layer.* Both confessions are printed with a marginal
   column of scripture references beside the text. The embedded OCR interleaves
   those references into the prose word by word — "are re- j^/f''^'^ deemed,
   quickened, and saved" — and no amount of cleanup recovers a sentence.
2. *Column splitting on the layout output.* Works on the 1689, which has a wide
   gutter, and raises page quality from 0.6 to 0.9. Fails on the 1644, where the
   reference column is set flush against the text with no gutter, and where the
   column switches sides partway down a page.
3. *Fresh OCR of the page images.* The images are crisp — far better than the
   embedded text layer, which returns nothing at all for several pages that are
   perfectly readable by eye. But Tesseract also merges the reference column into
   the text lines, so the same interleaving returns.
4. *Filtering the references out by word height.* The references are set smaller,
   but word height in OCR output tracks ascenders and descenders rather than
   font size, so this deletes short main-text words ("us", "an", "own") along
   with the references.

A bespoke per-block OCR pipeline would do it. It is not worth building, because
**both documents are on Wikisource in clean transcription**, and this project
already has a working route for that: the Westminster, Augsburg and Heidelberg
texts were produced from Wikisource EPUB exports. Export those two the same way
and they drop straight in.

McGlothlin is worth keeping only for Schleitheim, where no other public-domain
English text is known to exist.

The volume additionally carries the Standard Confession of 1660, the Orthodox
Creed of 1678, and the Philadelphia and New Hampshire confessions, all subject
to the same typography.

**Damaged pages in this scan, for whoever checks a replacement copy.** Book
pages 6, 7, 219, 226, 227, 252, 258 and 271 return nothing usable from the
embedded text layer. Pages 6 and 7 are genuinely faint in the image and defeated
three preprocessing approaches; the rest are perfectly legible images whose text
layer simply failed, and fresh OCR recovers them. Any replacement scan should be
checked at pages 6 and 7 first, since those carry Schleitheim's Articles V and VI
and are the only pages whose damage is physical rather than a text-layer
failure.

**Luther, *On the Jews and Their Lies*.** Deliberately not hosted. See
`reformation/NOTE-on-the-jews-and-their-lies.md`.

**Melito of Sardis, *On Pascha*.** Cannot be hosted, and this is structural
rather than a sourcing failure. The complete text was recovered only in the
twentieth century, so every English translation of it is modern and in
copyright, including the St Vladimir's Seminary Press edition (Stewart-Sykes,
Popular Patristics 20) supplied to this project. The 1885 Ante-Nicene Fathers
fragments at `church-fathers/melito-fragments.txt` are what the public domain
allows. See `notes/canon-lists-and-the-crosby-schoyen-codex.md`.

## On Internet Archive collections

The `catholictexts` collection was surveyed. It is mostly 19th-century Catholic
devotional works, Latin primers, periodicals and directories — little patristic
primary text. The exceptions worth pursuing are its **Library of the Fathers**
volumes, which are public domain and represent a different translation tradition
from ANF/NPNF, including works those series do not carry.

Archive.org item pages are JavaScript-rendered and return nothing useful to a
fetch tool. Full text for any item lives at:

    https://archive.org/download/<identifier>/<identifier>_djvu.txt

Download that file, put it somewhere the session can read, and extraction is
straightforward. Fetching it is not.

## Verification that runs after every batch

Three automated audits, all currently clean across the repository:

1. **File endings** — apparatus, running headers, or a heading as the last line;
   also anything not ending on a complete sentence.
2. **Mid-file apparatus** — elucidation and editorial-introduction markers
   appearing inside a file, which signal that the extraction spanned a work
   boundary. This is the check that caught 14,248 words of 1885 editorial
   commentary sitting inside six files the manifest called complete works.
3. **Modern appendices** — articles added by the electronic edition's editor,
   which are not public domain. The guard now also matches `E. TONY`,
   `etony@`, and `AGREED STATEMENT ON CHRISTOLOGY`, which are the signature of
   a second modern insertion found in the Chalcedon section of volume 37. That
   volume carried two: a signed modern article, and a two-line pointer to it in
   the section's own table of contents. Both were excluded.

## A note on the councils volume

`creeds-and-councils/` now carries volume 37 of the scan — Percival's 1900
edition of *The Seven Ecumenical Councils* — split into eleven files. This one
departs from the repository's usual practice of stripping editorial apparatus,
and the departure is deliberate. In every other volume there is a Father's text
and an editor's commentary around it, and the two can be separated. Here the
volume *is* the primary-source collection: the canons, the ancient epitomes, the
historical notes and the excursuses are printed as one interleaved apparatus and
cannot be pulled apart without destroying the document. Percival died in 1899
and the edition is 1900, so all of it is public domain. Every manifest entry for
these files says so in its `notes` rather than claiming a bare "Complete".

Word counts in `manifest.json` are recomputed from the files, never carried
forward by hand.
