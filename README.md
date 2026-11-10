# US case law: the Supreme Court of the United States

Part of [lexifina/global-law](https://github.com/lexifina/global-law), one git history for every country's law: this is the case law of the Supreme Court of the United States, United States, which global-law reads at `united-states/supreme-court/caselaw/`.

Every published opinion of the Supreme Court of the United States in the Caselaw Access Project, as a git history: one file per decision, one commit
per decision, dated the day it was decided. 328,247 decisions from 1791-01-01 to the end of the source.

The U.S. Reports print every order as its own entry, and each is kept here: 268,794 memorandum orders (certiorari denied, dismissed, rehearing denied; under 300 characters), 29,213 short per curiam decisions and orders (300 to 999 characters) and 30,240 opinions (1,000 characters or more). Each file's `decision_type` says which.

## Questions as git commands

| Question | Command |
|---|---|
| A decision by its citation | `grep -P '^410 U.S. 113\t' decision-index.tsv` then `cat <path>` |
| Decisions given in a window | `git log --format='%(trailers:key=In-force,valueonly,separator=) %an' \| awk '$1 >= "1973-01-01" && $1 <= "1973-01-31"'`: read the `In-force:` trailer, not the commit date (see Corrections), or use the global-law tools |
| Decisions citing 11 U.S.C. § 1182 | `cat judgments/united-states/federal/usc/title-11/1182.jsonl` |
| The section as the court applied it | each overlay row's `law_commit` is the section's own version in the US Code record on the day decided (or `law_record_from` when the decision is older than the record) |
| Who wrote a line? | `git blame decisions/...`: each line is credited to the decision (court and citation) |

## Dates and authors
The commit date is the day the decision was given, at noon US Eastern (`In-force:` and `Decided:`). Git cannot hold dates
before 1970, so earlier decisions are committed on 1970-01-01, a second apart, and `In-force:` holds the real day. The
author is the court and the official citation, at an address that cannot exist. These are court rulings, not laws: every
commit is `Kind: court`.

## Source
The Caselaw Access Project's bulk data (Harvard Law School Library), dedicated to the public domain under CC0; court
opinions are not copyrightable (17 U.S.C. § 105; Georgia v. Public.Resource.Org, 2020). Each file keeps its source URL.
Text is CAP's OCR, unchanged.

## What it does not show
Opinions after the end of CAP's coverage (added from CourtListener where noted), unpublished decisions not in the
reporters, and statute citations to US Code titles the record does not hold.

## Added from the CAP bulk data (2026-11-08)
9,226 decisions this repo did not hold, from the Caselaw Access Project's bulk files (U.S., S. Ct., L. Ed. 2d, B.R., Ct. Cl., Fed. Cl., Cl. Ct., Vet. App., Alaska Fed., Alaska, Ohio Law Abs., F. and 13 more): every decision of
its courts in CAP that was not here by its CAP id, nor as the same case (day and name) read from another reporter. Each
was appended as its own commit, dated the day decided, after the decisions already here (the history is append-only, so
they are not in date order; the tools read each decision's own date). Their head matter, the publisher's text, is left
out. Now 383,560 decisions.

## Corrections (2026-11-09)
An independent check of the CAP case-law repos found data defects; they are fixed here by new commits on top of the
history (nothing is rewritten). Now 386,132 decisions.

- 2,616 decisions added, each as its own commit dated the day decided, in date order: decisions with text the first build missed (volumes whose extract was lost, decisions merged with a different decision of the same day, docket and name, decisions CAP files under no court name or under another place's jurisdiction).
- 33,095 files had no text: CAP holds no opinion text for them, only head matter. Each now gives the head matter under `## CAP head matter` (`text_source` in the front matter), in a `Kind: correction` commit on the same file (`Corrected:`, `Corrects:` the commit that added it).
- decision-index.tsv: 71,415 rows added (the citations and CAP ids of decisions held here under another reporter, and of the new decisions), 335 rows that gave a citation to a different decision removed.
- `decision-relations.tsv` lists 394 files as `same-decision-as` another file here: decisions the CAP append added although the same decision (same day, docket or name, and text) was already here. Both files are kept; the tools read the relation.
- `decision-dates.tsv` lists the 6,412 decisions whose CAP date gives only the month or the year (precision `month` or `year`); they are committed on the first day of it, and In-force says that day.

Dates in the commit history: git cannot hold dates before 1970, so a decision before 1970 is committed from 1970-01-01 noon UTC-5, a second apart.
Here 90,700 decisions are before 1970, so 47,501 of their commits are dated 1970-01-02 or later, 1,378 of them in or after April 1970 (an append continued an earlier count). The commit date is not the day decided: `git log --since/--until` over 1970 mixes them with the decisions of 1970. Read the `In-force:` trailer (as the window recipe above now does) or use the global-law tools.

Head matter differs by when a decision came in: this repo's first build, from the Caselaw Access Project, includes each decision's head matter (syllabus, headnotes, counsel) in its file; the decisions appended later from the CAP bulk data leave it out (the publisher's text, not the court's), except where CAP has no opinion text, where the file gives the head matter under `## CAP head matter`. A text search over head-matter words therefore finds them only in the first build's decisions.

The CAP append also left 44 empty commits (a re-append of decisions whose CAP date gives only the month or year); they change no file and cannot be removed without rewriting the history.

A second pass (2026-11-09) listed 1,335 more appended files as `same-decision-as` an earlier copy: orders printed both in the Supreme Court Reporter and the U.S. Reports, of the same day, docket and text (their dockets were compared one by one, and the text and its length checked, since an orders list shares its dockets among many orders).

A third pass (2026-11-09) listed the 98 remaining files the CAP append added a second time from the same CAP record (a date given only to the month or year, compared as written) as `same-decision-as` the first copy; with the earlier passes every one of the 326 such re-adds is listed. The 44 empty commits of that re-append change no file.

## Parallel-reporter copies (2026-11-09)
165 files here are the same decision as another file here, and are now listed in `decision-relations.tsv` as `same-decision-as` the file kept: one decision printed in two reporters (an official and a regional reporter, such as 604 Pa. 242 and 985 A.2d 955), each a CAP case with its own CAP id (140), or two CAP records of one case under one citation (25). Both files are kept; the kept file is the one other rows already keep, else the official reporter's, else the earlier added; decision-index.tsv now gives the other file's citations, docket and CAP id to the kept file. A pair is matched by the same day decided, a citation, docket or name in common, and the same opinion text (head matter left out): similarity of at least 0.8 over the first 4,000 characters, or one text inside the other; orders shorter than 300 characters only with the same docket on both, and never where both give dockets with no number in common.
