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

## The fetch tool does not return pages

Settled by test, because a claim circulated that texts had been "pulled directly
over HTTPS" and that a 512 KB sync index was the real limit. Neither is right,
and no such index figure was ever claimed here.

The fetch tool converts a page to markdown and then has **a small fast model
answer a prompt about it**. That is its documented behaviour. It does not return
page bytes. Asked live for two chapters of the Roberts-Donaldson *Didache*
verbatim, it returned no text at all — it returned a refusal, on the mistaken
ground that an 1885 translation is copyrighted. Sometimes that layer refuses,
which is visible. Sometimes it produces fluent prose with verses quietly missing,
which is not: that is what happened to 1 Enoch, and the curly-quote transform
found later was the same layer leaving fingerprints.

**Nothing in this repository was obtained by fetch.** Every file was extracted
from bytes on disk. Where a `source_url` names a website — the two Justin files
and the Thirty-Nine Articles — the bytes came from a page saved to disk, and the
URL is a provenance citation, not a retrieval route. That distinction is what
generated the confusion, so it is worth stating plainly in any manifest entry.

Known-good closing lines, for anyone checking a fetched copy against this one:
Eusebius IV–X ends "Christians. (10)"; *City of God* ends "join me in giving
thanks to God. Amen."; *Contra Celsum* III–VIII ends "GLORY BE TO THEE, OUR GOD;
GLORY BE TO THEE."

## Fifteen entries are marked unverified

An audit of every `source_url` found **15 entries whose provenance is a citation
rather than a file** — five creeds and ten short pagan-source and related
extracts, all predating the current working method. They are now carrying
`"verification": "unverified - provenance is a citation, not a source file"` in
`manifest.json`. They have not been rewritten.

Two of them fail a check against this repository's own byte-sourced witnesses,
and the failures are not cosmetic:

- `creeds-and-councils/nicene-creed-325.txt` shares only a 25-word run with the
  creed as printed in Percival 1900, and its two most doctrinally loaded
  clauses — "of the essence of the Father" and "being of one substance with the
  Father" — do not appear in the witness in that form. NPNF reads "of the
  substance of the Father" and "of one substance (ὁμοούσιον, consubstantialem)".
- `creeds-and-councils/chalcedonian-definition.txt` shares only a 17-word run
  with the Definition as printed in the same volume.

`creeds-and-councils/nicene-creed-381.txt` fares much better — a 76-word verbatim
run — and can be treated as substantially verified.

For the two that diverge, the remedy is not to edit them. Point entries at
`council-of-nicaea-325.txt` and `council-of-chalcedon-451.txt`, which are byte
extractions and carry the conciliar text as printed.

One further problem surfaced in the same audit: `pagan-sources/lucian-death-of-
peregrinus.txt` names its translation as **A. M. Harmon, Loeb Classical Library**
and asserts public domain in its own header. Harmon's Lucian volume containing
*The Passing of Peregrinus* is Loeb 1936 — after the 1928 line, and Loeb is on
this project's forbidden list. It is flagged unverified along with the rest and
should be re-sourced or dropped rather than trusted.

## OCR damage is documented, not repaired

See `notes/ocr-damage-inventory.md`: 852 distinct damaged forms, 8,827
occurrences, from two recurring scan errors (**ii** and **li** both read for
**h**). A mechanical repair was built and **deliberately not applied**. A silent
edit across primary texts is a correct-and-quote move, and the damage staying
visible is better than a repository that looks clean and is not.

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

## Scripture

`scripture/` carries the King James Version, split Old and New Testament, from
the Project Gutenberg edition (ebook 10). Complete, chapter and verse numbered,
public domain in the United States. It is here so that scripture quoted anywhere
in the app can be checked against a hosted text rather than against a session's
memory — which the project's standing rule forbids.

It also carries the American Standard Version (1901) and Tischendorf's 8th
edition Greek New Testament, both from eBible.org, whose PDFs state their public
domain status on the title page. Three witnesses rather than one: an English
translation readers know, a second English translation that is closer to the
Greek and Hebrew word order, and the Greek itself.

The point of having free equivalents on hand is that there is then no reason to
reach for a copyrighted edition. The ESV, Nestle-Aland, the UBS text and Biblia
Hebraica Stuttgartensia are all in copyright and none of them offers this project
anything these three do not. See `notes/quoting-modern-sources.md`.

Splitting these further, one file per book, is straightforward if the app ever
wants to bind a citation to a single book rather than fetch a testament.

## Machine translation is never acceptable

A PDF supplied to this project, *On the Procession of the Holy Spirit* by Cardinal
Johannes Franzelin, states on its title page: "Gpt-5 Translation". It was
rejected outright.

This is worth stating as a rule rather than as one rejection. A machine
translation of a Latin theological text is unverified text wearing the clothes of
a translation. It has no named translator, no edition, and no one who checked it.
Publishing it would breach the project's first rule — never fabricate or
paraphrase a text — while looking exactly like compliance. **Any file whose
provenance is an AI translation is rejected regardless of how good it reads, and
regardless of whether the underlying original is public domain.**

## Post-1928 works cleared by non-renewal

`church-fathers/hippolytus-apostolic-tradition.txt` is Burton Scott Easton's
translation, and the printed page it comes from says "Copyright 1934, Cambridge
University Press". It is in this repository because United States copyright in it
was not renewed, and Project Gutenberg — which does its own renewal-record
research before releasing anything from this period — cleared and released it in
2020, with a transcriber's note stating the work is public domain in the country
of publication.

That is a different kind of clearance from a pre-1929 imprint, and the manifest
entry says so rather than resting on a bare "public_domain": true. Anyone
uncomfortable with the distinction can drop one file and one manifest entry.

## Rejected from the bulk supply of August 4

- **The MacArthur Study Bible (ESV).** The ESV text is Crossway's and the study
  notes are MacArthur's. Both in copyright. Not usable in any form.
- **Two large archives of Bibles and interlinears.** They contain Biblia Hebraica
  Stuttgartensia, Nestle-Aland 27 and 28, the UBS Greek New Testament and the ESV
  Interlinear — all in copyright — and several files carry the filename markers of
  known pirate sites. Nothing from these archives may be used. A handful of
  genuinely public-domain items sit inside them (the 1901 American Standard
  Version, the 1885 Revised Version, the 1917 JPS Tanakh, Brenton's 1851
  Septuagint, Tischendorf), but all are page-image PDFs, and Gutenberg or
  Wikisource gives the same texts in far better shape.
- **Prayers of the Early Church**, ed. J. Manning Potts, The Upper Room, 1953. A
  modern devotional compilation; the selection and renderings are the editor's.
- **Lig uit lig**, A. van Selms, 1951, in Afrikaans. Modern and in copyright.
- **Out of scope rather than unusable**, all public domain and all available if
  wanted later: Aquinas' *Summa* II-II, Kurtz's *Church History* vol. 3, a 1904
  hymnbook study, a 19th-century sermon, and a second translation of the Augsburg
  Confession, which the repository already carries in the 1921 Concordia
  Triglotta version.
- **Still worth adding on request:** the complete Douay-Rheims Bible (Gutenberg
  1581), which would give readers the deuterocanonical books the canon-list
  entries discuss, and Peter Martyr Vermigli's treatise in Thomas Becon's
  sixteenth-century English (Gutenberg 22151).

## The sacraments list — status

Two of the six came straight out of the 38-volume scan and are in:

- `church-fathers/tertullian-on-baptism.txt` — ANF vol. 3, tr. S. Thelwall, 1885.
  Coxe's Elucidation is excluded; the treatise ends where Tertullian ends, asking
  the reader to remember "Tertullian the sinner".
- `church-fathers/augustine-on-baptism-against-the-donatists.txt` — NPNF first
  series vol. 4, tr. J. R. King, 1887. All seven books.

Tertullian's *On Prayer* (same volume, same translator) went in alongside them,
since it sits immediately after *On Baptism* in ANF and is the earliest Latin
commentary on the Lord's Prayer.

**Three of the four remaining are now in**, from archive.org plain text:

- `reformation/zwingli-selected-works.txt` — Jackson, University of Pennsylvania,
  1901. German works translated by Lawrence A. McLouth, Latin by Henry Preble and
  George W. Gilmore. The whole volume as printed, Jackson's introductions
  included. It carries the *Refutation of Baptist Tricks*, which is Zwingli
  arguing against the Anabaptists on baptism — the piece the sacraments section
  actually needs.
- `reformation/menno-simons-complete-works.txt` — Funk, Elkhart, Indiana, 1871.
  **Do not trust the word "Complete" in the title.** The publishers' own closing
  note, retained at the end of the file, states that they condensed and abridged
  parts of Menno's writings on the Incarnation of Christ and left out material
  they judged unimportant. The manifest entry says so. Anything the app quotes
  from the Incarnation material should be checked against another edition.
- `reformation/synod-of-jerusalem-1672-confession-of-dositheus.txt` — Robertson,
  Thomas Baker, London, 1899, with the appended Confession published under the
  name of Cyril Lucar which the Synod condemned. Robertson's own introduction is
  omitted.

**Trent is the one still missing.** Sessions here
cannot fetch text from the web — the one attempt, on 1 Enoch, silently dropped
verses and renumbered chapters, which is why the rule is download the file, then
extract from bytes. Each of these needs a file put where a session can read it:

- **Council of Trent, Waterworth 1848.** Project Gutenberg has it. Take the plain
  text or EPUB. Waterworth's long historical introduction is separate from the
  canons and decrees and should not be mixed into the same file.
- **Zwingli.** Samuel Macauley Jackson's *Selected Works of Huldreich Zwingli*
  (University of Pennsylvania, 1901) is the edition to want, and *The Latin Works
  and Correspondence*, vol. 1 (Putnam, 1912), is the other. Both public domain,
  both on the Internet Archive. Avoid the Zwingli volume in the Library of
  Christian Classics (Westminster, 1953) — in copyright, and easy to land on
  first in a search.
- **Menno Simons.** *The Complete Works of Menno Simon*, translated from the
  Dutch, Elkhart, Indiana, 1871, is public domain and on the Internet Archive.
  This would also give the sacraments section a real Anabaptist voice on baptism
  while Schleitheim stays blocked.
- **Confession of Dositheus, Synod of Jerusalem 1672.** J. N. W. B. Robertson's
  *The Acts and Decrees of the Synod of Jerusalem* (Thomas Baker, London, 1899)
  is the translation, public domain and on the Internet Archive.

For any Internet Archive item, the plain text lives at
`https://archive.org/download/<identifier>/<identifier>_djvu.txt`. Download that
file rather than the item page, which is JavaScript-rendered and returns nothing
useful.

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

**The Second London Confession (1677/1689) is now in**, at
`reformation/second-london-baptist-confession-1689.txt`, from a Wikisource
export rather than from the scan below. That page carries a Wikisource banner
saying the source document is not known — meaning no scan backs the
transcription on the wiki — so the text was corroborated independently against
the 1911 McGlothlin printing at ten points spread across the document. Eight
matched exactly; the two misses fall on words the McGlothlin OCR mangles, not on
differences of wording. Thirty-two chapters and the signatories, complete.

**The 1644 First London Confession is still out.** The facsimile of the original
1644 printing (`1644_Anabaptist_Confession_of_Faith.djvu`) is a genuine and
valuable object — original spelling, long s, the printer's imprint — but its OCR
layer is not publishable: a clean-token ratio of 0.83, marginal scripture
references interleaved into the prose exactly as in McGlothlin, and visible word
loss (Article XXVII breaks off mid-sentence as "is _ X wit"). Roughly 4,900
words recovered against an expected 6,000-plus. It should come from a Wikisource
transcription the same way the 1689 did.

**The earlier attempt on both, for the record — blocked by the McGlothlin
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
