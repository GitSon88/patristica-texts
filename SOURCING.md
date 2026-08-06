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

### Lucian, *The Death of Peregrinus* — removed

`pagan-sources/lucian-death-of-peregrinus.txt` has been **deleted from the
repository and from the manifest**. It named its translation as **A. M. Harmon,
Loeb Classical Library**, and asserted public domain in its own header. Harmon's
Lucian volume containing *The Passing of Peregrinus* is Loeb 1936 — after the
1928 line, and Loeb is on this project's forbidden list.

The reason for removing rather than flagging: a file that asserts public domain
in its own header while being in copyright is worse than a gap. A gap is
obvious. A false clearance passes an audit, and would keep passing every future
audit, because the header says the work has already been checked.

**The replacement, if wanted:** H. W. Fowler and F. G. Fowler, *The Works of
Lucian of Samosata*, Clarendon Press, Oxford, 1905 — four volumes, public domain,
on Project Gutenberg and the Internet Archive. *The Death of Peregrine* is in
volume 4. Until a file is on disk, the Peregrinus entry should quote nothing and
link out instead.

**A related check, not a problem.** `pagan-sources/suetonius-claudius-25-4.txt`
names J. C. Rolfe, which is also Loeb — but Rolfe's Suetonius is 1914, before the
line, so it is public domain. It stays, flagged unverified with the rest for the
separate reason that no file sits behind it. Loeb attribution alone is not
disqualifying; the edition year is what decides.

## OCR damage is documented, not repaired

See `notes/ocr-damage-inventory.md`: 852 distinct damaged forms, 8,827
occurrences, from two recurring scan errors (**ii** and **li** both read for
**h**). A mechanical repair was built and **deliberately not applied**. A silent
edit across primary texts is a correct-and-quote move, and the damage staying
visible is better than a repository that looks clean and is not.

## Read a page of the output. Audits do not catch everything.

The Westminster Confession shipped unquotable and nothing in the checking regime
caught it. The file was built from the Wikisource transcription of the **1647
printing**, in which the long ſ came through as a **spaced lowercase s**: "Pre s
ented", "mo s t", "Maje s tie", 672 times. The Wikisource maintenance banner
"This work is incomplete" survived into the file as well, and the source page
meant it.

Every automated check passed. First and last line were sane. The apparatus regex
found nothing. Word count looked plausible. What was never done was **reading a
page of the extracted text**, and a human trying to quote from it found the
defect in one sentence.

**That step is now part of the method**: after extraction, read a full page of
real body text before registering anything. Specifically look for the long ſ
rendered as `s` with spaces around it or as `f`; for parallel columns or
interleaved recensions flattened into consecutive paragraphs; and for a modern
editorial layer running alongside the original.

### The replacement

`reformation/westminster-confession.txt` is now built from the Wikisource
transcription of the **Tercentenary Edition** (Publishing Office of the
Presbyterian Church of England, London, 1946) — 12,045 words, 33 chapters, the
signatories from Charles Herle down to Adoniram Byfield, no spaced-s anywhere, no
export chrome. A full page was read before it was registered.

On the 1946 imprint: it is a plain sixpenny reprint with no editor, no
introduction and no notes. The text is the 1647 original, and a faithful reprint
of a public-domain document acquires no new copyright. That is firmer ground than
the Easton clearance recorded above, which concerns a translation.

### The standing check

Export chrome and completeness banners are a small fixed vocabulary, so this is a
grep rather than a habit. Run across **every** file, not only the ones known to
come from a wiki:

    grep -rn "this work is incomplete\|see the help pages\|the style guide\|\
    leave a comment on the talk page\|Exported from Wikisource\|\
    START OF THE PROJECT GUTENBERG\|END OF THE PROJECT GUTENBERG\|\
    Transcriber's Note\|wsexport\|not been proofread" --include=*.txt .

Currently clean across all 147 files. Keep the strings exact. A looser version of
this grep — bare `proofread`, bare `Project Gutenberg`, `needs to be` — returns
15 files, every one a false positive from ordinary prose, and a check that cries
wolf stops being run.

### The spaced-letter pattern is a prompt to read, never a licence to fix

The check that catches the Westminster defect is a search for single letters
separated by spaces. It has a high false-positive rate, and **every false
positive is a faithful transcription of real typography**. Run across the
repository it returns four files:

- `reformation/westminster-confession.txt` — the genuine defect, now replaced.
- `church-fathers/apostolic-constitutions.txt` — "through Thy Son Jesus Christ
  **o u r** Lord". Printer's letterspacing for emphasis, correctly transcribed.
- `church-fathers/jerome-letters.txt` — "this however is only the **a b c** of
  your soldiership". The idiom itself.
- `reformation/menno-simons-complete-works.txt` — a single incidental match.

The tempting response to the last three is to close them up. **Doing so would be
the correct-and-quote failure, arrived at by a checker instead of by hand** —
and worse than by hand, because a checker applies it uniformly and silently to
things it never read.

The rule: a spaced-letter hit means *go and read that line*. It never means
repair it. If reading the line shows real damage, the finding is documented, not
corrected — see the OCR inventory above.

### Completeness, not just cleanliness

A file can be free of chrome and still be two thirds of a document, which is what
happened here: 8,453 words read as plausible against a true ~12,000. Cleanliness
and completeness are separate checks. Every confessional text has a known
structural count, and that count is the check:

| file | expected | found |
|---|---|---|
| `westminster-confession.txt` | 33 chapters | 33 |
| `second-london-baptist-confession-1689.txt` | 32 chapters | 32 |
| `augsburg-confession.txt` | 21 articles + 7 on abuses | 28 |
| `heidelberg-catechism.txt` | 129 questions | 129, Q1 "only comfort in life and death" to Q129 "Amen" |
| `thirty-nine-articles.txt` | 39 articles | 39 |

Two things the sweep turned up, both noted rather than repaired:

- The Augsburg file carries the spaced-s rendering inside a **single German
  scripture quotation** (Ps. 119:46). The English text is unaffected.
- In the Thirty-Nine Articles, **Article 1's heading and number were lost in
  extraction**. Its text is present and complete — the file opens on "There is
  but one living and true God, everlasting, without body, parts, or passions" —
  but unlabelled. The heading has not been supplied. Inserting text that is not
  in the file is the edit this project does not make; re-extracting from a
  Wikisource transcription is the fix.

### Why the 1946 imprint is safe — the argument, not the conclusion

Recorded in full so the next person to question it can weigh it rather than take
it on trust. Neither the person who wrote this nor the session that reasoned it
out is a lawyer.

1. **The text is not in copyright.** The Westminster Confession was completed in
   1646 and presented to Parliament in 1647. Copyright in the text expired
   centuries ago and cannot be revived by reprinting.
2. **There is no new authorship in the 1946 volume.** It names no editor, carries
   no introduction, no notes, no commentary, no modernisation credited to anyone.
   It is the 1647 text set in type, priced at sixpence. Copyright subsists in
   original expression; a faithful reprint adds none.
3. **The one right that *does* arise from reprinting has expired anyway.** United
   Kingdom law gives a publisher a right in the *typographical arrangement* of a
   published edition — the layout of the printed page, distinct from the text. It
   runs 25 years from publication. For a 1946 imprint that expired in 1971. And
   it protects the arrangement, not the words: a plain-text transcription does
   not reproduce a typographical arrangement in any event.
4. **This is firmer ground than the Easton clearance** recorded above. Easton is
   a *translation*, which is itself a copyrightable work, and rests on a
   non-renewal finding. The Westminster reprint involves no translation and no
   new authorship, so there is nothing for a copyright to attach to.

If any step of that is wrong, the file should go. It is stated in steps so the
wrong step can be identified.

## Josephus — what Phase 14 will need, and what it does not have

The app session's Phase 14 (Caiaphas, Pilate, Herod the Great, Herod Antipas)
needs Josephus properly. **The 38-volume scan does not contain him** — it is
ANF/NPNF, Christian authors only — and this repository holds nothing but two
short extracts, `josephus-james.txt` and `josephus-testimonium-flavianum.txt`,
both of which are among the fifteen entries flagged unverified for having a
citation rather than a file behind them.

That matters more than usual here. The Testimonium Flavianum is the single most
disputed paragraph in Josephus, and the repository's copy of it has no bytes
behind it. An entry arguing from it is arguing from an unverified text.

**What to download:** William Whiston's translation, 1737, public domain and the
standard English Josephus. Project Gutenberg carries *The Antiquities of the
Jews* as ebook 2848, and the complete *Works of Flavius Josephus* is on the
Internet Archive. Either gives Book XVIII, which carries Pilate, the Testimonium,
Herod Antipas, John the Baptist and Caiaphas' deposition, and Book XX, which
carries the James passage and Theudas under Cuspius Fadus.

Getting Whiston in would close three things at once: Phase 14's sourcing, the two
unverified extracts, and the Theudas chronology the Gamaliel entry now rests on.

## Line endings: what is right here is wrong in the app repo

`.gitattributes` in this repository is `* text=auto eol=lf`. Every file here is
plain UTF-8 prose; normalising to LF stops a Windows checkout showing all 148
files as modified, which happened twice in one hour and had to be swept with
`git checkout -- .` each time.

**Do not carry that setting across to `patristica-app`.** The app session
corrected this, and the correction matters:

- Patristica.html is built by **byte-exact concatenation** of its source
  fragments, and `build.py --check` compares a **sha256**.
- Any line-ending translation makes the same source hash differently on a
  Windows machine than on Netlify, so the check that exists to catch a stale
  build starts failing on a clean one.
- The app therefore carries `* -text` deliberately, which is the opposite
  setting, and it is right there for the same reason this one is right here.

The related symptom in the app — `.bat` files showing as wholly modified — is
not an attributes problem at all. Their committed blobs are LF, the working
copies are CRLF, and **CRLF is correct for `.bat` on Windows**, so committing
them is the fix rather than normalising them away.

The general rule: a repository of prose wants normalised line endings; a
repository whose build hashes its own bytes wants none. Check which kind you are
in before setting either.

## Phase 16 — the four texts it needs, none of them on disk

The app session surveyed Phase 16 (Wycliffe, Hus, Savonarola, Erasmus) before
writing it and found **exactly one usable source in this repository: Foxe**.
That is a real problem rather than a thin patch. Foxe's *Book of Martyrs* is a
1563 Protestant martyrology written to make a case — good evidence for what
Protestants believed about these men, not evidence for what the men themselves
wrote. Four entries resting on it would quote the Catholic side only through a
hostile witness, in every entry rather than in one lane.

All four gaps are public domain:

| author | text | where |
|---|---|---|
| Hus | *De Ecclesia: The Church*, tr. David S. Schaff, 1915 | Wikisource; archive.org `deecclesiachurch00husjuoft` |
| Hus | *The Letters of John Hus*, Workman and Pope, 1904 | Wikisource, complete |
| Erasmus | *In Praise of Folly*, tr. John Wilson, 1668 | Project Gutenberg 30201 |
| Wycliffe | *Select English Works*, ed. Arnold, 1869–71 | archive.org `selectenglishwor03wycluoft` |
| Savonarola | *The Triumph of the Cross* | Project Gutenberg 74508 |

**Hus first.** *De Ecclesia* is the book he was burned over; without it the entry
describes a trial whose evidence it cannot quote. **Erasmus second**, because his
absence distorts most — he is the one of the four who stayed in the Catholic
Church, and quoting him only through Protestants who claimed him would misread
him in exactly the direction the Reformation already pulls.

**A warning for whoever extracts Wycliffe:** Arnold's edition is **Middle
English**. It must be quoted as it stands or summarised in the entry's own words.
Modernising the spelling and then presenting the result as a quotation is the
correct-and-quote failure in its most tempting form, because the modernised
version reads better.

## Josephus — in, and it retires two unverified extracts

`jewish-sources/josephus-antiquities-of-the-jews.txt`, 546,148 words. Whiston's
1737 translation from Project Gutenberg 2848. All twenty books, no editorial
layer — this edition carries none of Whiston's dissertations, so there is no
boundary to mark, which makes it the only one of the eight without one.

All three passages the app needs are present and were located rather than
assumed: the **Testimonium Flavianum at 18.3.3**, **Theudas under Cuspius Fadus
at 20.5.1**, and the **death of James at 20.9.1**.

That last one matters for a reason beyond Phase 14. `pagan-sources/josephus-
james.txt` and `pagan-sources/josephus-testimonium-flavianum.txt` were two of the
fifteen entries flagged for having a citation rather than a file behind them —
and the Testimonium is the single most disputed paragraph in Josephus. An entry
arguing from it was arguing from a text with no bytes behind it. Both are now
marked **superseded** in the manifest, pointing at the full work. They are not
deleted: they are what `REPO_BIND` currently points at, and removing them would
break bindings rather than fix anything. Repoint the bindings, then remove.

Antiquities only. *Wars of the Jews* is a separate work and a separate Gutenberg
ebook; nothing in the current phases needs it.

## Phase 16 — four of five in, Wycliffe rejected

Each carried an editorial layer, each boundary was verified rather than taken on
trust, and a page of each was read before registering.

- **`reformation/hus-de-ecclesia.txt`** — 113,115 words, all 23 chapters, Schaff
  1915. His forty-six-page introduction is excluded; the file opens at Chapter I.
  One thing to know: **Schaff's footnotes remain interleaved**, because the scan
  runs each page's footnotes into the text stream. They are visibly modern
  scholarship against Hus's translated prose, and they are 1915, not 1413.
- **`reformation/erasmus-praise-of-folly.txt`** — 36,820 words, Wilson 1668.
  Gutenberg **9371** was used rather than 30201: both are Wilson, but 30201 opens
  with the 1668 editor's *Life of Erasmus*, and 9371 does not. The file opens on
  Erasmus's own prefatory epistle to Thomas More, which is his.
- **`reformation/savonarola-triumph-of-the-cross.txt`** — 56,652 words, all four
  books, Procter's edition. His introduction is excluded: it argues against a
  rival translation that dropped three chapters on the sacraments — useful
  context for an entry to *know*, but the editor's polemic, not Savonarola.
- **`pagan-sources/lucian-death-of-peregrine.txt`** — 6,211 words, Fowler 1905.
  This replaces the file removed for naming a Loeb 1936 translation while
  asserting public domain in its own header. Its presence in volume 4 was
  **confirmed by locating it**, not assumed, since that was the specific thing
  asserted last time. Fowler's explanatory notes at pp. 191–243 are excluded.

### Wycliffe — extracted, read, and rejected

`selectenglishwor03wycl`, Arnold's edition, volume III. It was extracted and then
**not registered**, for three reasons that compound:

1. **The editorial layer is not separable.** Arnold's headnotes and page
   footnotes are interleaved throughout, and the scan flattens them into the text
   stream. Measured across 1,524 paragraphs: **458 carry Middle English markers
   and 599 are modern English.** A session hunting a quotable line would land on
   Arnold roughly as often as on Wyclif.
2. **The Middle English does not survive the OCR.** Thorn and yogh come through
   as ASCII noise — "snffride ]?e kyngis lege men die for hunger", "l>at",
   "\ox\^", "^elde". Quoted verbatim that is gibberish on the page.
3. **The only repair available is the forbidden one.** Cleaning it up means
   modernising the spelling, and modernising before quoting is the
   correct-and-quote failure in the form the pre-flight specifically warned
   about, because the modernised version reads better.

A file that can be neither quoted verbatim nor repaired is not a source; it is a
reference the entry must summarise from, and it would sit in the manifest looking
like something a reader could open. **The Wycliffe entry should summarise and
link out until a better text exists.**

What would fix it: a transcription rather than a scan — Wikisource has some
Wyclif in edited Middle English with the thorns intact — or, for quotation
purposes, a modern scholarly edition that is openly licensed. Arnold's own text
is sound; it is this scan of it that cannot be used.

## Trent — in, with the boundary marked

`reformation/council-of-trent-buckley.txt`, 111,290 words. Buckley's translation,
George Routledge and Co., London. **The title page of this scan reads 1861**,
not the 1851 the pre-flight assumed — recorded as the page shows it rather than
as expected, since both printings exist and only the scan is evidence.

All six sessions the sacraments entries need are present and verified by their
running heads: **VII** (sacraments in general), **XIII** (Eucharist), **XIV**
(penance and extreme unction), **XXII** (the sacrifice of the Mass), **XXIII**
(holy orders, whose heading the OCR mangles but whose running heads confirm it),
**XXIV** (marriage).

### Where the boundary falls, and why

The volume has three layers. The file contains only the first:

1. **Lines 1531–16111 — Trent.** Bull of Indiction, all twenty-five sessions, the
   confirming bulls of Pius IV, the Index approbation. This is the file.
2. **Lines 16113–17661 — "The Constitutions taken from the ancient law".**
   Pre-Trent canon law from Lateran, Lyons and Sixtus IV, printed because Trent
   referred to it. Related to the council; not the council. Excluded.
3. **Lines 17662–end — the Appendix.** Condemnations of Wyclif, Hus and Luther,
   the bull of Leo X against Luther (1520), the Baian errors, Jansenius,
   Quesnell, the **Synod of Pistoia (1794)**, and an address by **Pius VII in
   1805**. Excluded.

Buckley's own preface says the appendix is about a third of the volume. A session
quoting from the back third would attribute an eighteenth-century condemnation to
a sixteenth-century council, in the entries whose whole purpose is to let the
Catholic position speak in its own words. The cut was verified by searching the
finished file for Pistoia, Auctorem, Quesnell, Jansenius, Wyclif and "Ancient
Constitutions" — none present.

### The OCR, measured rather than eyeballed

Reading a page first suggested the file was too damaged to quote: "yenerable",
"diyine", "hj" for "by", "Qod", "Euchanst". Measuring corrected that impression.
The substitution is **y read for v**, and it runs to **67 occurrences in 108,726
words**; malformed tokens of every kind are 0.77%. In a 147-sentence sample from
Session XIII, **three sentences carry a visible substitution — one in fifty.**

The page that alarmed me was heading-dense, and display type is where this scan
is worst. The prose is sound. The rule that follows: **read the passage before
quoting it and choose a clean sentence**; where a sentence is damaged, that is
visible, so nobody quotes it unawares — and nobody should silently repair one.

**Hanover College's hand-keyed Waterworth is still worth having** as a second
witness for those six sessions specifically. Not because this file is unusable,
but because a doctrinal quotation deserves two witnesses and Waterworth is keyed
rather than scanned. Its known omissions — the closing oration and the appendix —
are by design, so structural divergence between the two is expected.

## Trent — the original pre-flight, kept for the reasoning

Neither file is on disk yet. Two witnesses are wanted, and the second is chosen
deliberately: **Buckley (1851, Routledge)** from Wikisource, and the **Hanover
College** hand-keyed transcription of Waterworth (1848). The raw OCR at
`canonsdecreesofs00cath_djvu.txt` is the third choice, not the second — it is
scan OCR and likelier to carry exactly the damage that made the Westminster file
unquotable.

Sessions needed: **VII** (sacraments in general), **XIII** (Eucharist), **XIV**
(penance and extreme unction), **XXII** (the sacrifice of the Mass), **XXIII**
(holy orders), **XXIV** (marriage).

### The boundary that must be marked

**About a third of Buckley's volume is not Trent.** His own preface says the
appendix of additional statutes forms about one third of the book. From roughly
page 365 the volume carries condemned propositions on contrition and penance,
and pages 401 and 422 are *Auctorem fidei* material condemning the Synod of
Pistoia — **1794**, two and a half centuries after the council.

A session quoting from the back third of that file would attribute
eighteenth-century condemnations to a sixteenth-century council, in an entry
whose whole purpose is to let the Catholic position speak in its own words. That
is the same class of error as the interleaved Ignatian recensions and the Zwingli
`Catabaptists.` blocks: two documents flattened into one file with no seam
visible to a reader.

**The extraction must stop at the end of Session XXV.** Whatever follows goes in
a separate file or nowhere.

### Proofreading is uneven between namespaces

Buckley's Wikisource *mainspace* pages read clean in modern orthography, no
spaced-s. The underlying `Page:` scans are unproofread and carry OCR noise —
"matters to be Seated of" for *treated of*, "interpretation cf the sacred
scriptures", "imagess", "CONSIDERALE". If an export pulls from unproofread pages
that noise comes with it. **Spot-check the six sessions specifically**, not a
page chosen at random.

### The diff will not be symmetric, and that is expected

Buckley says of Waterworth that the scriptural references are inaccurate, the
closing oration is omitted, and the appendix is missing. So structural
differences between the two are Waterworth being incomplete by design, not a
defect discovered. **Diff them for wording inside the decrees; treat structural
divergence as expected rather than as a finding.**

## The Lucian contradiction — resolved on evidence

Two sessions disagreed: one reported `pagan-sources/lucian-death-of-
peregrinus.txt` present in the manifest, this one reported it deleted. Settled by
reading the published state directly out of git rather than by inference or by
any fetch:

    git fetch origin
    git show origin/main:manifest.json      # 147 entries, zero lucian
    git ls-tree --name-only origin/main pagan-sources/

`origin/main` carries **147 manifest entries, no Lucian entry, and no Lucian file
in the tree**. The deletion is commit `e8c761b`, pushed. There is no orphaned
manifest entry and no manifest-versus-disk gap; the other session is reading a
copy from before that commit. The `REPO_BIND` binding should be dropped, and
Fowler 1905 is still worth sourcing.

Reading `git show origin/main:<path>` is the reliable way to settle any
"what is actually published" question. It returns bytes from the remote, with no
summarising layer and no cache.

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
