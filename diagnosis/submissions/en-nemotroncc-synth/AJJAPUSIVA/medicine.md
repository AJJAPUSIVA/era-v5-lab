# Medicine sheet: en-nemotroncc-synth (AJJAPUSIVA)

## Submission status

Spec v1 handoff draft. The current labels cover353 of500 documents; the final500-row submission is still incomplete.
The initial B4 candidate remains below; current per-disease counts and additional document-specific review rules follow.
Gold-set results are pending; no pass or zero-harm result is claimed.
Final submission deadline: 31 October 2026, 18:00 IST.

The executable candidate below is separate from the historical document-specific
review notes in the appendix. Those notes are retained for traceability, not
submitted as independently runnable or validated medicines. Their local
application and Parquet validation statements are prior review claims and have
not been rechecked in this revision because the source pack is unavailable here.

## Medicine 1: B4 billing-heading placeholder

1. **Disease:** B4 (template junk in prose). One documented occurrence among
   the first 150 reviewed documents; this is not a count for the entire pack.
   Example: `d2fb665c40af5edb`, `-DATE-` at zero-based character offset 326,
   in `Total Due After -DATE-:`. Only one example is available; two further
   examples are not invented. This optional initial medicine is below the
   more-than-five-occurrences threshold for a mandatory medicine section.
2. **Medicine:** replace the exact heading `Total Due After -DATE-:` with
   `Total Due After the Due Date:` only when it occurs exactly once.
   Leave all other text unchanged. The function accepts a text string and
   returns a text string; it has no document-ID filter, so the same rule can
   be evaluated on unseen gold documents. It requires only the Python standard
   library and installs no tools.

```python
def medicine_b4(text):
    """Return text with one exact billing-heading placeholder repaired."""
    old = "Total Due After -DATE-:"
    new = "Total Due After the Due Date:"
    if text.count(old) != 1:
        return text
    return text.replace(old, new, 1)
```

3. **Action:** fix text; never drop documents. Do not infer a calendar date.
   This rule does not repair other placeholders or broken LaTeX.
4. **Harm test:** pending execution by Data A / Data B on the English gold
   set using this exact function. Gold-set identifier/version, document count,
   changed count, dropped count, and pass/fail are all pending. Use the runner
   below and report its returned counts. More than 1% of gold documents
   changed or dropped fails. Synthetic checks establish function behavior only.
5. **Side effects:** the literal heading might be intentional quoted text,
   documentation of a template, or an actual bill that requires a specific
   date. The rule cannot distinguish these contexts and may change valid text.
   Repeated occurrences are deliberately left unchanged for review. A gold-set
   pass alone does not establish effectiveness on the full dataset.

## Runnable harm-test helper

Execute this block after the medicine block. Data A / Data B supply the actual
gold texts as an iterable of strings. The helper counts source documents,
including changes in any position; it does not print source text or identifiers.
For future medicines, `None` represents a dropped document. This is a local
handoff interface, not a claim to implement an unavailable spec v1 harness API.

```python
def harm_test(medicine, gold_texts):
    total = changed = dropped = 0
    for text in gold_texts:
        if not isinstance(text, str):
            raise TypeError("Gold documents must be text strings")
        result = medicine(text)
        if result is not None and not isinstance(result, str):
            raise TypeError("Medicine must return a text string or None")
        total += 1
        if result is None:
            dropped += 1
        elif result != text:
            changed += 1
    if total == 0:
        raise ValueError("Gold set must not be empty")
    affected = changed + dropped
    return {
        "gold_documents": total,
        "changed_documents": changed,
        "dropped_documents": dropped,
        "affected_percent": 100 * affected / total,
        "passes_one_percent_threshold": 100 * affected <= total,
    }
```

## Remaining work

- Complete the147 unlabelled source documents, including preview-only321–330 and pending classifications noted below.
- Confirm provisional structure diagnoses with a second reader; counts include provisional C3 entries.
- Data A / Data B gold results remain pending. No gold-test file or gold data is included in this update and no gold test was run.
- Later dated/current per-row entries supersede historical decisions. Earlier local-application statements record prior review claims, not independently rerun gold tests.

## Current disease summary and handoff limits

Counts below are from the353 current labels, not the full500-document pack. A `clean` label records no confirmed listed disease under the reviewer-approved taxonomy policy; separate editorial corrections do not imply the original was factually perfect. Structure classifications explicitly marked provisional remain review work, not validated diagnoses.

| Code | Label occurrences |
|---|---:|
| B4 | 1 |
| B5 | 1 |
| C1 | 19 |
| C3 | 34 |
| C4 | 3 |
| C5 | 2 |
| F1 | 5 |
| F3 | 3 |

### C1 candidate: exact reviewed boilerplate spans

1. **Disease:** C1;19 labelled occurrences, with this candidate covering the three explicit examples below. Other C1 interventions are recorded in their per-row rules.
- `3cd12d535137ab2f`: `The Awesomer is reader-supported. When you buy through links on our site, we may earn an a` at offset0.
- `701cacaccdb06931`: `Citation Disclaimer: These citations have been automatically generated based on the inform` at offset45.
- `76b4036d49000599`: `Enter your email address to follow this blog and receive notifications of new posts by ema` at offset2211.

2. **Medicine:** remove only the exact ID/hash-guarded reviewed spans. This is a sample-specific rule, not a general cleaner for unseen gold.

```python
import hashlib

def medicine_c1_reviewed(doc_id, text):
    spans = {
        '3cd12d535137ab2f': ('3cd12d535137ab2f29d7b8a38461d7c3e35610d67581e5e57df0999e49918a80', 0, 180),
        '701cacaccdb06931': ('701cacaccdb06931da002988266d4a60dc9f4b9152b3a2462d8d8ac262dd0939', 45, 287),
        '76b4036d49000599': ('76b4036d49000599952ad548de4cdee33afdeb27b5df3e12b44067c012a1b9be', 2211, 2304),
    }
    rule = spans.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    return text[:rule[1]] + text[rule[2]:]
```

3. **Action:** fix; do not drop.
4. **Harm test:** pending Data A / Data B. No gold set supplied or executed. Exact guards limit changes to reviewed source identities; an unchanged unrelated gold set would not establish general usefulness.
5. **Side effects:** deletion can remove context when boilerplate is substantive; exact spans/IDs must not be broadened without review. Local exact-output and changed-input controls passed.

### C3 contextual candidate: reconstruct three verified sentence/verse boundaries

1. **Disease:** C3;34 labelled occurrences, including provisional diagnoses. The following candidate covers three explicit pairs only. Complete-sentence/heading joins elsewhere are not treated as independently verified strict C3 examples here.
- `a147d6925833c136` ends body `o perish thereby.
When King Gunther heard all this, great was his`; continuation `fe0100ca641983cc` starts `desire to win the beautiful warrior-maiden for his wife, and he d`.
- `ee6655c72b3799da` ends body `ed with joy.
"Happy am I," said Siegmund,
"Thus to behold the day`; continuation `89a8e620955b1040` starts `Which sees the beauteous Kriemhild
Enthroned, with us to stay.
Wi`.
- `84649d75dd11ae14` ends body `e spring and forth they bounded like two panthers over the grassy`; continuation `bca8eefdcfb28646` starts `plain, all the huntsmen following. Siegfried was the swifter; com`.

2. **Medicine:** contextual two-record operation, only the specified matching source identities/hashes. Preserve synthetic Q&A separately; emit one joined body and trace both source IDs. `None` here means no pair operation, not document drop. This pair API is separate from the single-text B4 harm helper.

```python
import hashlib

def medicine_c3_reviewed_pair(left_id, left, right_id, right):
    reviewed = {
        ('a147d6925833c136', 'fe0100ca641983cc'): ('a147d6925833c1363b68666dd55c988c133223eced68d43cacc02d70fb0691e0', 'fe0100ca641983ccb5cc98a67b4f0095934c3da502a4793cebc0be7051b7a224'),
        ('ee6655c72b3799da', '89a8e620955b1040'): ('ee6655c72b3799dacd976bf36a2a009c70cb5cefdd0c4b4396f78f6a16f558bf', '89a8e620955b10402627a2686535abdf7d821d542c0c0f52a6f37b37b766522d'),
        ('84649d75dd11ae14', 'bca8eefdcfb28646'): ('84649d75dd11ae14397b9cda227514f8de584f19c092a106ed119dd030373536', 'bca8eefdcfb2864690c54dd866aad2d3194135c99dbca1dab1eba7f3e79f0c2f'),
    }
    expected = reviewed.get((left_id, right_id))
    if expected is None or (hashlib.sha256(left.encode("utf-8")).hexdigest(), hashlib.sha256(right.encode("utf-8")).hexdigest()) != expected:
        return None  # No pair operation; retain both original records.
    left_body, marker_l, left_qa = left.partition("\n\nQuestion:")
    right_body, marker_r, right_qa = right.partition("\n\nQuestion:")
    return {
        "source_ids": (left_id, right_id),
        "body": left_body + " " + right_body,
        "source_qa": (marker_l + left_qa, marker_r + right_qa),
    }
```

3. **Action:** fix by contextual reconstruction; retain both original records outside the derived output.
4. **Harm test:** pending Data A / Data B; requires their contextual gold-pair protocol. No gold test run or pass claimed. Exact guards are sample-specific and do not prove generalization.
5. **Side effects:** wrong adjacency/provenance may join unrelated material. Local checks reconstruct both original bodies and Q&A exactly, except for the explicitly inserted join space in the derived body. This does not certify every prior C3 diagnosis.

## Historical and per-row review appendix

Earlier sections can describe superseded classifications or local artifacts. Current CSV and later corrections govern; all gold tests remain pending. No local artifact is part of the commit.

# English synthetic Parquet — diagnosis medicine (DRAFT)

## Scope and method
Source: `en-nemotroncc-synth.parquet`, 500 rows. `labels.csv` contains 353 unique document labels; 147 source documents remain unlabelled. This supersedes earlier draft wording that said only rows 1–150 had been reviewed. The latest labelled batch covers source rows 371–400 (30 rows: 371 fix/C1;F1, 374 fix/F1;C1, and 28 clean). Rows 321–330 were previewed but are not included in the label count. Earlier audit-only reviews and pending classifications are not counted as completed labels. Known pending classifications include 116, 175, 178, 179, 183, 184, and 223; consult the per-row audit for source limits and repairs. The original source pack is preserved. Gold tests remain pending with Data A / Data B. Later per-row entries supersede earlier decisions; historical counts below describe prior review stages.

## Confirmed interventions
- Drop document rows 22 (F3), 113 (C5 / search-result collage), and 124 (C5/C1 / snippets lacking coherent article).
- Join 101+102, 114+115, 116+117, 119+120, 121+122+123, 142+143 in source order.
- Remove an affiliate disclosure from 111; an automated-citation disclaimer from 112; the email subscription call-to-action from 120.
- Split row 125 at the confirmed new question beginning `i have a 2003 chev s-10 pickup`, retaining the Infiniti G37 and Chevrolet S-10 material as two separate entries.
- Remove the clearly anomalous character sequence `e~ \\` in row 148; normalize the known broken LaTeX sigma commands in 95.

## Unresolved precision / do not fabricate
- Rows 75, 76: manually reviewed local join applied; C3 remains provisional because both source rows end in complete sentences.
- Rows 109, 110: manually reviewed body join and separate Q&A applied locally. Missing list after row 110's `such as:` remains unresolved; C3 is provisional for row 109.
- The rest of the 500-row dataset is not asserted to be clean. Inspect the review-status column before downstream use.

## Row 16: manual C4 medicine
- Document: `a14eaa5e58baf3c3`. A manual review selected a four-way split at candidate topic boundaries. These are not verified original article boundaries.
- Evidence: topic changes before `Australia's English`, `England and Wales,`, and `Welsh, a Celtic language,`.
- Rule: apply only to this document ID and exact reviewed source text. Split into Irish education, Australian languages, English legal jurisdiction, and Celtic languages. Preserve all characters, including boundary whitespace; derive segment IDs from the source ID.
- Zero-based character offsets (end exclusive): `0:188`, `188:338`, `338:409`, `409:1091`.
- Code (after checking the source ID and exact source text):

```python
bounds = [0, 188, 338, 409, 1091]
segments = [text[a:b] for a, b in zip(bounds, bounds[1:])]
assert "".join(segments) == text
```

- Applied locally: one source document becomes four segments; no characters are deleted, added, or rewritten. The original source and previous reviewed package remain intact.
- Validation: all four segments are nonempty, contiguous, and reconstruct the 1,091-character original exactly; the local Parquet round-trip matches the segments.
- Harm test: not run; no gold set was supplied. Reconstruction verifies preservation only, not semantic correctness or the required gold-set harm threshold.
- Side effects: splitting can remove cross-topic context. The final Celtic-language block still needs content review. Do not apply this document-specific rule to other rows.

## Row 34: manual C4 medicine
- Document: `5958fb6996158153`. A manual review selected five paragraph-level topic blocks: 3D printing, disruptive technologies, military contracts/mobile devices, AI, and Digi.me. Original article boundaries remain unverified.
- Evidence: paragraphs start with `Disruptive technologies span`, `Microsoft's employees raised`, `AI is increasingly integrated`, and `Digi.me, founded`.
- Rule: apply only to this document ID and exact reviewed source text. Split at the four stated paragraph starts. Preserve all characters and whitespace; derive segment IDs from the source ID.
- Zero-based character offsets (end exclusive): `0:386`, `386:689`, `689:897`, `897:1270`, `1270:1399`.
- Code (after checking source ID and exact source text):

```python
bounds = [0, 386, 689, 897, 1270, 1399]
segments = [text[a:b] for a, b in zip(bounds, bounds[1:])]
assert "".join(segments) == text
```

- Applied locally: one source document becomes five segments, with no rewriting or deletion. The original source and previous reviewed package remain intact.
- Validation: segments are nonempty and contiguous; joining them reconstructs all 1,399 original characters exactly. The local Parquet round-trip matches the segments.
- Harm test: not run; no gold set was supplied. Text preservation does not validate semantic correctness or establish compliance with the gold-set harm threshold.
- Side effects: the original may be a compiled technology summary; splitting can remove context. Some blocks still mix subtopics (notably military contracts and mobile devices). No finer split is assumed, and this rule must not be applied to other rows.

## Rows 75-76: manual list reconstruction (C3 provisional)
- Documents: `60f1a32f80f6a3cf` and `ab4f03c12b813b47`. Row 75 promises 15 films but ends at numbered entry 11. Row 76 overlaps on Moonstruck and supplies four further films.
- Rule: for these two exact source texts only, append row 76 after row 75 with two newlines. Retain row 76's Moonstruck paragraph as additional detail under item 11. Prefix its four remaining film paragraphs with `12. ` through `15. `, in source order. Do not rewrite or discard either Moonstruck description.
- Code (after checking source IDs and exact source text):

```python
markers = ['"Obvious Child"', '"Sabrina"', "\"She's Gotta Have It\"", '"To All The Boys I\'ve Loved Before"']
starts = [text76.index(marker) for marker in markers]
ends = starts[1:] + [len(text76)]
joined = text75 + "\n\n" + text76[:starts[0]]
joined += "".join(f"{n}. " + text76[a:b]
                  for n, a, b in zip(range(12, 16), starts, ends))
```

- Applied locally: two documents become one 15-entry list. Both source IDs remain traceable. All source characters remain in order; only 18 characters (a two-newline separator and four numbering prefixes) are added.
- Validation: entries are numbered 1-15; both original source texts can be recovered exactly using the source-piece map; the local Parquet round-trip matches the joined record.
- Harm test: not run; no gold set was supplied. These checks do not establish the required gold-set harm threshold.
- Side effects: source continuation is inferred from list contents, not verified against the original article. Moonstruck still has two descriptions, deliberately retained to avoid losing distinct details. Existing factual inconsistencies in the text are not repaired. C3 remains provisional under the repository's mid-sentence/code-block definition. Do not generalize this join to other rows.

## Row 81: B4 placeholder medicine
- Document: `d2fb665c40af5edb`. The billing explanation contains `-DATE-` at zero-based character offset 326.
- Manually reviewed action: replace `Total Due After -DATE-:` with `Total Due After the Due Date:`. This is a generic explanation of a billing term; no calendar date is inferred or inserted.
- Rule: apply only to this source document and exact heading, with exactly one matching occurrence. Classify as B4 (template junk in prose).

```python
old = "Total Due After -DATE-:"
new = "Total Due After the Due Date:"
assert text.count(old) == 1
cleaned = text.replace(old, new, 1)
assert cleaned.replace(new, old, 1) == text
```

- Applied locally: one placeholder is replaced; all other source text is unchanged. Original data is retained separately.
- Validation: exactly one target heading; reversing the replacement reconstructs the source exactly; local Parquet round-trip matches the result.
- Harm test: not run; no gold set was supplied. This document-specific check does not establish the required gold-set harm threshold.
- Side effects: do not apply this substitution to an actual bill requiring a specific date, or to unrelated uses of `-DATE-`.

## Rows 109-110: join article bodies and preserve Q&A separately
- Documents: `d896f8ca6a2beae1` and `fdd2b470571c5e4d`. Row 109 ends its body with a leading-platforms heading; row 110 starts with discussion of leading platforms. Source Parquet metadata confirms both share the same article URL, WARC record ID `84ec882b-7e1b-42c1-bce9-812e0e3345e3`, and source partition. This supports common provenance; it does not recover missing content or verify full original ordering. Both have `kind2=diverse_qa_pairs`, so Q&A is source synthetic content, not assumed boilerplate.
- Manually reviewed rule: for these two exact source texts only, split each at its first `\n\nQuestion:`. Keep that delimiter with its Q&A. Join the two bodies in source order with two newlines, and retain each Q&A block separately with its source ID. Preserve both source IDs on the joined body. Do not rewrite answers or invent a missing list.

```python
bodies, qa_blocks = [], []
for text in (text109, text110):
    boundary = text.index("\n\nQuestion:")
    body, qa = text[:boundary], text[boundary:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = "\n\n".join(bodies)
```

- Applied locally: two mixed article/Q&A records become one article body and two separate Q&A records. All original source characters remain recoverable; only a two-newline join separator is inserted.
- Validation: each body plus its Q&A reconstructs its exact source text. Local Parquet round-trip matches all three output records; both source IDs are preserved on the joined body.
- Unresolved: the article still ends `with all leading business systems such as:`. Flag the missing list; do not claim the joined article is complete. Row 109 has no mid-sentence cutoff, so its C3 diagnosis remains provisional. Q&A factual accuracy has not been checked.
- Harm test: not run; no gold set was supplied. Reconstruction checks do not establish semantic correctness or the required gold-set harm threshold.
- Side effects: the inferred continuation could be wrong; moving Q&A changes context. Keep both Q&A blocks and provenance, and do not generalize this rule to other records.

## Rows 150-151: restore the split verse (C3)
- Documents: `ee6655c72b3799da` and `89a8e620955b1040`. Row 150 ends `Thus to behold the day`; row 151 begins `Which sees the beauteous Kriemhild`. Both share the source URL and WARC record ID `2865e9a6-e149-4ff2-82e5-e58d6f188315`.
- Applied rule: split each exact source text before its first `\n\nQuestion:`; keep that delimiter with its Q&A. Join bodies 150 and 151 with one newline to preserve verse layout. Keep the two Q&A blocks separate, linked to their respective source IDs; attach both source IDs to the joined body.

```python
bodies, qa_blocks = [], []
for text in (text150, text151):
    offset = text.index("\n\nQuestion:")
    body, qa = text[:offset], text[offset:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = "\n".join(bodies)
```

- Quotation decision: preserve the missing opening quotation mark before `Nor is it save for love` as source-inherited punctuation. The publisher's [The Betrayal chapter](https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=betrayal) and [PDF, printed page 28](https://www.heritage-history.com/site/hclass/christian_europe/ebooks/pdf/upton_nibelungs.pdf) reproduce it. This resolves attribution of the issue; no punctuation repair is claimed.
- Validation: no source characters deleted or rewritten; one joining newline inserted. Both original texts reconstruct exactly, verified by SHA-256 hashes. The local Parquet round-trip matches the article and two Q&A records.
- Revisit record: local `changed-rows-review.md`, under Rows 150-151, records original bodies, Q&A, source IDs, source hashes, exact join offset, separator, and quotation decision. To reverse, split at the recorded join offset, remove the one added newline, and append each saved Q&A to its body. The source Parquet is unchanged; local data outputs are not submitted.
- Harm test: not run; no gold set supplied. The reconstruction checks do not establish the required gold-set harm threshold.
- Side effects: extracting Q&A changes document context; retain provenance and both Q&A blocks. This exact-source rule must not be generalized to arbitrary adjacent rows.

## Row 152: source-verified dialogue restoration (C3)
- Document: `adc3f46213437a54`. Internal dialogue cuts off after `an thou dost hunt this`, and an earlier speech lacks its opening quotation mark.
- Reference: [Project Gutenberg, Upton translation of The Nibelungs](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm), Chapter XV (printed page 83) and Chapter XVI (printed page 85). This parallel edition supplies the opening quotation and the missing ` day!` plus closing quotation. The Heritage History transcription itself lacks the sentence ending, so adjacent source-pack rows cannot recover it.
- Applied rule: restore only those two exact passages in this document. Match the pack's straight quotation style; leave all other wording and synthetic Q&A unchanged.

```python
edits = [
    ('\nThen have we armed to no purpose!"', '\n"Then have we armed to no purpose!"'),
    ('an thou dost hunt this\nAlas!', 'an thou dost hunt this day!"\nAlas!'),
]
cleaned = text152
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    restored = restored.replace(new, old, 1)
assert restored == text152
```

- Validation: seven characters inserted in total; no source characters deleted or rewritten. Both edits reverse exactly to the original source hash; synthetic Q&A is unchanged. Local Parquet round-trip matches the result.
- Revisit record: local `changed-rows-review.md`, under Row 152, stores original text and hash, exact replacements, source offsets, reference edition, and reversal instructions. Original Parquet remains intact; local data outputs are not submitted.
- Harm test: not run; no gold set supplied. Exact reversal does not establish the required gold-set harm threshold.
- Side effects: restoring from a parallel edition introduces an editorial choice; record that provenance and do not apply these replacements to other documents.

## Rows 154-155: restore a sentence split across documents (C3)
- Shared URL and WARC record ID `2865e9a6-e149-4ff2-82e5-e58d6f188315` verify common source provenance. Row 154 ends `over the grassy`; row 155 begins `plain, all the huntsmen following.`. This is a direct sentence continuation.
- Applied rule: for these exact source records, separate each body from its first `\n\nQuestion:` delimiter, retaining the delimiter with Q&A. Join the two bodies with one space. Preserve both source IDs and keep both synthetic Q&A sections separately.

```python
bodies, qa_blocks = [], []
for text in (text154, text155):
    offset = text.index("\n\nQuestion:")
    body, qa = text[:offset], text[offset:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = " ".join(bodies)
```

- Validation: the reconstructed sentence reads `over the grassy plain, all the huntsmen following.`. No source characters deleted or rewritten; one space inserted. Reversing at the saved join offset and appending each Q&A exactly reconstructs the source texts, verified by SHA-256.
- Unresolved: row 154 still appears to omit speech after `replied:`. Row 155's `upon. the` and fused caption `THEDEATHOF SIEGFRIED.` are retained pending separate review. This join does not make either row fully repaired.
- Revisit record: local `changed-rows-review.md` contains both original records, joined result, preserved Q&A, source hashes, and join offset. Original Parquet remains unchanged; local review data is not submitted.
- Harm test: not run; no gold set supplied. Exact reconstruction is not a substitute for the gold-set harm threshold.
- Side effects: Q&A extraction changes context; retain source IDs on the joined body and on each Q&A. Apply only to these reviewed records.

## Rows 156-158: join directly connected narrative bodies (C3)
- Source IDs: `3ec2166b803a7ed8`, `7dd2b369f4ee29a4`, `2fc62e1bd955eae6`. All share the source URL and WARC ID `2865e9a6-e149-4ff2-82e5-e58d6f188315`.
- Evidence: row 156 ends `cried out:` and row 157 supplies the quoted speech. Row 157 ends `Swooning she was borne away;` and row 158 continues `and thereupon,`. These are direct narrative connections, not merely shared topics.
- Applied rule: split each exact source record before its first `\n\nQuestion:`. Join bodies 156 and 157 with a newline for the speech; join 157 and 158 with one space to complete the sentence. Preserve all source wording, headings, three source IDs, and each Q&A separately. This supersedes the earlier keep-separate review decision.

```python
bodies, qa_blocks = [], []
for text in (text156, text157, text158):
    offset = text.index("\n\nQuestion:")
    body, qa = text[:offset], text[offset:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = bodies[0] + "\n" + bodies[1] + " " + bodies[2]
```

- Validation: zero source characters deleted or rewritten; one newline and one space inserted. Both boundary continuations checked; all three original texts reconstruct exactly using saved offsets and Q&A, verified by SHA-256.
- Unresolved: missing quotation marks in 156/157, possible `to!` OCR error in 157, and the unsupported `her father` wording in row 157's Q&A remain unchanged and flagged. Joining does not resolve those issues or make the entire output clean.
- Revisit record: local `changed-rows-review.md` contains originals, output, three Q&A blocks, exact source hashes, separators, and join offsets. Reverse the two separator insertions at the saved offsets and append each source Q&A to its original body.
- Harm test: not run; no gold set supplied. Exact reconstruction does not establish the gold-set harm threshold.
- Side effects: Q&A extraction changes context. Retain provenance and do not generalize to arbitrary neighboring rows.


## Row 160: preserve with verified omissions flagged (C3)
- Document: `b2a595175aa47ff4`. Preserve the entire source row and synthetic Q&A unchanged; do not drop or merge it. Its end is a complete sentence, but there are internal dialogue omissions.
- Verified evidence: [Heritage History, King Etzel Woos Kriemhild](https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=etzel) and [Project Gutenberg, Chapter XIX, printed pages 101-102](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) both contain speeches after `Giselher rebuked Hagen, saying:` and `added significantly:` that are absent from the pack.
- Decision: retain the source text and label `fix / C3` for the internal missing passages; do not claim the row is clean. Source-based restoration remains pending, with no added words or punctuation applied.
- Revisit: the two links and exact anchors identify the passages to compare before any future repair. No harm test applies to the current no-change preservation decision; any later restoration still requires validation and the prescribed harm test.

## Row 161: source-verified opening quotation restoration
- Document: `06a617ce8f69b17c`. The speech beginning `Thou art the guilty one, not we!` lacks its opening quotation mark in the pack.
- Reference: [Project Gutenberg, Upton translation, Chapter XX, printed page 105](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) confirms an opening quotation at that exact speech. Restore one straight quote to match the pack's style. Keep row 161 separate from row 162 and preserve its synthetic Q&A unchanged.
- Label: `fix`; disease code left empty because the supplied disease list has no exact category for a missing prose quotation mark. Do not misclassify it as truncation or markup damage.

```python
old = '\nThou art the guilty one, not we!'
new = '\n"Thou art the guilty one, not we!'
assert text161.count(old) == 1
cleaned = text161.replace(old, new, 1)
assert cleaned.replace(new, old, 1) == text161
```

- Validation: one character added; all other source text and Q&A identical. Exact inverse replacement reconstructs the source.
- Revisit record: local `changed-rows-review.md` stores source text, applied result, edit offset, source reference, and reversal instructions. No separate row files are created.
- Harm test: not run; no gold set supplied. Side effect: this is an editorial restoration from a parallel edition; apply only to the reviewed source ID and exact phrase.

## Rows 162-163: restore the split ferryman sentence (C3)
- Documents: `6e37ba20647c0a25` and `bf9ebeae946c4859`. Both share the same source URL and WARC ID `2865e9a6-e149-4ff2-82e5-e58d6f188315`. Row 162 ends `She told him the`; row 163 begins `ferryman's house was on the other bank`.
- Applied rule: separate each exact source body from its first `\n\nQuestion:` delimiter; keep the delimiter with its Q&A. Join the two bodies with one space. Preserve both source IDs and both synthetic Q&A blocks separately.

```python
bodies, qa_blocks = [], []
for text in (text162, text163):
    offset = text.index("\n\nQuestion:")
    body, qa = text[:offset], text[offset:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = " ".join(bodies)
```

- Validation: zero source characters deleted or rewritten; one joining space inserted. Both original texts reconstruct exactly from the recorded join offset and their Q&A, verified by SHA-256.
- Unresolved: possible OCR errors `Yet fate bath so declared` in row 162 and `Now halt thou lied to me!` in row 163 remain unchanged. Repeated verse-opening quotation marks are preserved. The join does not make the entire output clean.
- Revisit record: local `changed-rows-review.md` includes both original records, joined result, preserved Q&A, source hashes, exact separator and join offset. Reverse by splitting at that offset, removing the inserted space, and appending each source Q&A to its own body.
- Harm test: not run; no gold set supplied. Exact source reconstruction does not establish the gold-set harm threshold.
- Side effects: Q&A extraction changes context; retain provenance and apply only to these exact reviewed records.

## Rows 164-165: reconnect chapter heading and chapter text
- Documents: `5f01dd1e1e757f40` and `f0d2f0cf3e08335b`. Shared source URL and WARC ID `2865e9a6-e149-4ff2-82e5-e58d6f188315`. Row 164 ends with `The Comrades`; row 165 begins `Hagen, seeing Volker not far away`.
- Source verification: [Heritage History, The Comrades](https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=comrades) places that exact opening immediately after the heading. This is a heading/body regrouping, not a mid-sentence cutoff.
- Applied rule: split each exact source record at its first `\n\nQuestion:`. Join bodies with one newline, retaining the heading and all original wording. Preserve both source IDs and keep each Q&A separately.

```python
bodies, qa_blocks = [], []
for text in (text164, text165):
    offset = text.index("\n\nQuestion:")
    body, qa = text[:offset], text[offset:]
    assert body + qa == text
    bodies.append(body)
    qa_blocks.append(qa)
joined_body = "\n".join(bodies)
```

- Labels: `fix`; no disease code assigned to row 164's heading/body regrouping because it is not a clear C3 sentence cutoff. Row 165 is flagged C3 for an independently verified internal omission, not for this join.
- Unresolved: the publisher chapter contains a passage beginning `Quickly arming themselves` between the paragraph ending `have his life!` and `When the Huns heard this`; the pack omits it. This join does not restore that passage. Possible `cloth` OCR readings and `my, foe` punctuation remain unchanged; the publisher transcription also contains them.
- Validation: one newline inserted; zero source characters deleted or rewritten. Exact reconstruction of both originals verified with SHA-256; both synthetic Q&A blocks unchanged.
- Revisit record: local `changed-rows-review.md` records originals, joined result, source reference, join offset, separator, source hashes, and preserved Q&A. Reverse by splitting at the saved offset, removing the single inserted newline, and appending each source Q&A.
- Harm test: not run; no gold set supplied. Exact reconstruction does not establish the gold-set harm threshold. Q&A extraction changes context; keep provenance and do not generalize this manual join.

## Row 166: preserve source-inherited transcription issues
- Document: `3e3f61cd431b6790`. Preserve original text and synthetic Q&A unchanged, as requested after source verification.
- [Heritage History, The Comrades](https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=comrades) contains the same `beloved. spouse`, `frame cloth appear`, `Balmung once loth strike`, and `"Truly "said` readings. They were not introduced by this pack's row boundary.
- [Project Gutenberg, Chapter XXIII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) provides alternative corrected readings, recorded in local review history. No substitutions are applied.
- Decision: source-faithful preservation, not a disease-free claim. Label remains `fix` for unresolved transcription issues, with no forced disease code because the supplied list lacks an exact category. Original provenance does not establish grammatical correctness.
- No text transformation or harm test is performed. This unchanged row is excluded from the applied-changes document.

## Row 167: source-verified opening quotation restoration
- Document: `3ef34026380566db`. The speech beginning `Give place!` lacks its opening quotation mark in the pack.
- Reference: [Project Gutenberg, Upton translation, Chapter XXIV, printed page 125](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) confirms an opening quotation at that exact speech. Restore one straight quote to match the pack's style. Keep row 167 separate from row 168 and preserve its synthetic Q&A unchanged.
- Label: `fix`; disease code left empty because the supplied disease list has no exact category for a missing prose quotation mark. Do not misclassify it as truncation or markup damage.

```python
old = 'shouting: Give place!'
new = 'shouting: "Give place!'
assert text167.count(old) == 1
cleaned = text167.replace(old, new, 1)
assert cleaned.replace(new, old, 1) == text167
```

- Validation: one character added; all other source text and Q&A identical. Exact inverse replacement reconstructs the source.
- Revisit record: local `changed-rows-review.md` stores source text, applied result, edit offset, source reference, and reversal instructions. No separate row files are created.
- Harm test: not run; no gold set supplied. Side effect: this is an editorial restoration from a parallel edition; apply only to the reviewed source ID and exact phrase.

## Row 168: caption spacing and source-verified quotation repair
- Document: `0074a878bb00d533`. Restore `THE MINSTRELAND HAGEN.` to `THE MINSTREL AND HAGEN.` and add an opening straight quote before `An thou lov'st me, do not so!`. Keep row 168 separate; preserve caption content and all synthetic Q&A.
- Sources: [Heritage History, The Minstrel Knight](https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=knight) shows the caption with distinct words. Its prose also lacks the opening quote; [Project Gutenberg, Chapter XXIV](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) supplies that quote and confirms the caption words. The quote restoration is an editorial repair from a parallel edition, not a recovered pack-boundary character.
- Label: `fix`; disease code left empty because the supplied list has no exact category for these prose punctuation/caption-spacing defects.

```python
edits = [
    ('THE MINSTRELAND HAGEN.', 'THE MINSTREL AND HAGEN.'),
    ("\nAn thou lov'st me, do not so!", "\n\"An thou lov'st me, do not so!"),
]
cleaned = text168
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    restored = restored.replace(new, old, 1)
assert restored == text168
```

- Validation: two characters inserted (one space and one quotation mark); no source characters deleted or rewritten. Reverse replacements reconstruct the original exactly; Q&A unchanged.
- Revisit record: local `changed-rows-review.md` contains original and revised text, source references, exact edit anchors and offsets, source hash, and reversal instructions. No individual row files created.
- Harm test: not run; no gold set supplied. These exact-source checks do not establish the gold-set harm threshold. Do not generalize quotation insertion to other prose or verse.

## Rows 169-170: source-verified join, closing quote, and Q&A attribution
- Documents: `df1822e736052c65` and `3e6bb6fb3b9b57a6`. Both share the source URL and WARC ID `2865e9a6-e149-4ff2-82e5-e58d6f188315`. Row 169 ends `but far too`; row 170 starts `proud was the hero`.
- Reference: [Project Gutenberg, Upton translation, Chapters XXIV-XXVI](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm). Chapter XXV, printed pages 130-131, confirms the continuous sentence and a closing quote after `rue it!`. Chapter XXIV, printed page 128, identifies Volker as the speaker of the challenge before the Huns make no answer.
- Applied rule: separate each exact source body from its first `\n\nQuestion:`. Restore one straight closing quote in row 169, join bodies with one space, and retain both Q&A blocks separately. Correct only the first row 169 Q&A question's speaker from Kriemhild to Volker; the answer remains unchanged. Preserve both source IDs and original Q&A in the revisit record.

```python
parts = []
for text in (text169, text170):
    offset = text.index("\n\nQuestion:")
    parts.append((text[:offset], text[offset:]))
old = 'if so, then dearly shall they rue it!\n'
new = 'if so, then dearly shall they rue it!"\n'
assert parts[0][0].count(old) == 1
body169 = parts[0][0].replace(old, new, 1)
old_question = "Question: Did the Huns respond to Kriemhild's challenge?"
new_question = "Question: Did the Huns respond to Volker's challenge?"
assert parts[0][1].count(old_question) == 1
qa169 = parts[0][1].replace(old_question, new_question, 1)
joined_body = body169 + " " + parts[1][0]
qa_blocks = [qa169, parts[1][1]]
```

- Validation: exact original texts reconstruct by splitting at the saved join offset, removing the joining space, reversing the closing-quote insertion and question-speaker replacement, and restoring each original Q&A. SHA-256 hashes match. Other body text, Q&A answers, and row 170 Q&A are unchanged.
- Labels: `fix / C3` for the split sentence. The quote and Q&A attribution repairs are documented separately; no unrelated disease code is forced for them.
- Revisit record: local `changed-rows-review.md` contains original and revised records, exact anchors, join offset, source IDs, hashes, references, and reversal instructions. No individual row files created; source Parquet unchanged.
- Harm test: not run; no gold set supplied. Reconstruction checks do not establish the gold-set harm threshold.
- Side effects: speaker replacement is a manually verified synthetic-Q&A edit; restrict it to this exact question and source record. Preserve the original Q&A for audit. Q&A extraction changes context; retain provenance.

## Rows 172-173: restore the split verse (C3)

- Documents: `49d11c77033a2d36` and `928d4bf58d34db84`.
- Reason: row 172 ends inside a quoted verse; row 173 continues it. Project Gutenberg, Chapter XXVIII, confirms the sequence. Heritage History was unavailable; this is parallel-edition verification.
- Reference: [Project Gutenberg, Chapter XXVIII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: join only the two article bodies with one newline; retain both Q&A blocks separately with their source IDs. No quotation, spelling, factual or Q&A edits.
- Zero-based join offset: `2779` in the joined body; inserted separator: `\n`.
- Side effects / unresolved: moving Q&A changes context. Possible transcription issues in both rows remain unchanged. This join does not make the full output disease-free. Restrict the rule to the exact reviewed source IDs and source hashes.
- Reversal: take `joined_body[:2779]` and `joined_body[2780:]`, then append each original Q&A block to its matching body. Compare the restored SHA-256 hashes below.
- Validation: exact source reconstruction passed for both documents; precisely one newline added to the bodies; both Q&A blocks unchanged. Source Parquet SHA-256: `0f1c3a9866ada7fb5ea2a244135d9766375351bec366c13cd9c5abfb3d82573c`.
- Harm test: pending Data A / Data B; no gold-set result claimed.
- Status: applied locally, not pushed. Row 171 remains set aside unchanged.

```python
import hashlib
expected = ['49d11c77033a2d36f5ea2ecfbf17e8d010328acf85c732ecddf68bd219b0d83e', '928d4bf58d34db841769080b568c27c142ae5161fa3174bcea70802289a8b6a6']
parts = []
for text, digest in zip((text172, text173), expected):
    assert hashlib.sha256(text.encode('utf-8')).hexdigest() == digest
    boundary = text.index("\n\nQuestion:")
    parts.append((text[:boundary], text[boundary:]))
joined_body = "\n".join(body for body, qa in parts)
qa_blocks = [qa for body, qa in parts]
join_offset = len(parts[0][0])
assert joined_body[:join_offset] + qa_blocks[0] == text172
assert joined_body[join_offset + 1:] + qa_blocks[1] == text173
```

## Row 175: source-checked editorial repairs

- Document: `eafaaca57dc561cd`.
- Reason and source verification: Heritage History (hagen) reproduces the missing quote and hire/cloth readings. Gutenberg Chapter XXIX supplies the closing quote and him/doth readings. Applied as explicit editorial repairs from the parallel edition, not as claims of pack extraction damage.
- References: [Heritage History](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=hagen); [Project Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: Restore closing quote; hire -> him; cloth -> doth. Synthetic Q&A unchanged.
- Original text SHA-256: `eafaaca57dc561cd71d1df986c767ad5141998f74528dc261f94f73bfd739c58`; revised SHA-256: `ee0d022f351ac41f66a0b84e8afd32f4dc4ab4940c121530e07775b7bf86210c`.
- Exact edit anchors and zero-based offsets before each edit: `[('wrought on helm and shield!\n', 301), ('withheld hire by force.', 721), ('friend Hagen, cloth not a falsehood', 2079)]`.
- Scope: this exact source ID and hash only; no global spelling or punctuation substitution.
- Labels: classification pending because no precise supplied disease code covers these editorial defects; do not insert blank-code fix labels or claim clean.
- Validation: inverse edits reconstruct original exactly; Q&A identical; source Parquet unchanged. Harm test pending Data A / Data B.
- Side effects: cross-edition changes can override intentional source wording. Restoring missing text changes the sample; original source retained for exact reversal. No broader editorial normalization is applied.
- Status: applied locally, not pushed. Audit: `changed-rows-review.md` and batch review JSON.

```python
import hashlib
assert hashlib.sha256(text175.encode('utf-8')).hexdigest() == 'eafaaca57dc561cd71d1df986c767ad5141998f74528dc261f94f73bfd739c58'
edits = [('wrought on helm and shield!\n', 'wrought on helm and shield!"\n'), ('withheld hire by force.', 'withheld him by force.'), ('friend Hagen, cloth not a falsehood', 'friend Hagen, doth not a falsehood')]
cleaned = text175
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text175
```

## Row 177: source-checked editorial repairs

- Document: `976023f4a0f3f62b`.
- Reason and source verification: Heritage History (hagen, terror, rudiger) and Gutenberg Chapters XXIX-XXXI confirm a chapter-sized gap and no stray save at the boundary. Removed only the detached save token. The missing chapter remains unresolved; it may reflect synthetic selection and is not silently reconstructed.
- References: [Heritage History](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=rudiger); [Project Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: Remove detached save only; retain omission flag. Synthetic Q&A unchanged.
- Original text SHA-256: `976023f4a0f3f62ba54afbc3109c77764b3cdeed55300be08e3a3b7659327e01`; revised SHA-256: `ff90c21cf2a2ec847ce31d6f137afc6ac3f8aa653a4a037b7e229720e4eb5baa`.
- Exact edit anchors and zero-based offsets before each edit: `[('it had ever occurred. save\nThe Margrave Rudiger', 816)]`.
- Scope: this exact source ID and hash only; no global spelling or punctuation substitution.
- Labels: classification pending because no precise supplied disease code covers these editorial defects; do not insert blank-code fix labels or claim clean.
- Validation: inverse edits reconstruct original exactly; Q&A identical; source Parquet unchanged. Harm test pending Data A / Data B.
- Side effects: cross-edition changes can override intentional source wording. Restoring missing text changes the sample; original source retained for exact reversal. Chapter XXX omitted between two related narrative sections. No fabricated restoration, unrelated-text C4 diagnosis, or clean claim.
- Status: applied locally, not pushed. Audit: `changed-rows-review.md` and batch review JSON.

```python
import hashlib
assert hashlib.sha256(text177.encode('utf-8')).hexdigest() == '976023f4a0f3f62ba54afbc3109c77764b3cdeed55300be08e3a3b7659327e01'
edits = [('it had ever occurred. save\nThe Margrave Rudiger', 'it had ever occurred.\nThe Margrave Rudiger')]
cleaned = text177
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text177
```

## Row 178: source-checked editorial repairs

- Document: `3212b90956b7cb74`.
- Reason and source verification: Heritage History (rudiger) also lacks the first speech closing quote; Gutenberg Chapter XXXI confirms it. Restored one straight quote as an editorial repair; repeated verse-opening quotation marks are legitimate and preserved.
- References: [Heritage History](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=rudiger); [Project Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: Restore closing quote. Synthetic Q&A unchanged.
- Original text SHA-256: `3212b90956b7cb744be751be5403c846495fc2e864cef7a7cc852fe2c5224083`; revised SHA-256: `581ba43d8528987030325fde99eb6b78019b94ba4a6d6e2ca07225e3a9e6dd53`.
- Exact edit anchors and zero-based offsets before each edit: `[('hither to thy court!\n', 222)]`.
- Scope: this exact source ID and hash only; no global spelling or punctuation substitution.
- Labels: classification pending because no precise supplied disease code covers these editorial defects; do not insert blank-code fix labels or claim clean.
- Validation: inverse edits reconstruct original exactly; Q&A identical; source Parquet unchanged. Harm test pending Data A / Data B.
- Side effects: cross-edition changes can override intentional source wording. Restoring missing text changes the sample; original source retained for exact reversal. No broader editorial normalization is applied.
- Status: applied locally, not pushed. Audit: `changed-rows-review.md` and batch review JSON.

```python
import hashlib
assert hashlib.sha256(text178.encode('utf-8')).hexdigest() == '3212b90956b7cb744be751be5403c846495fc2e864cef7a7cc852fe2c5224083'
edits = [('hither to thy court!\n', 'hither to thy court!"\n')]
cleaned = text178
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text178
```

## Row 179: source-checked editorial repairs

- Document: `2f4410b98b7ef464`.
- Reason and source verification: Heritage History (rudiger) contains bath and the same unusual verse; Gutenberg Chapter XXXI reads hath. Both editions include the short Gernot speech before the encounter in row 180. Restored that exact Heritage History paragraph at the end of row 179 body; preserve the source verse unchanged.
- References: [Heritage History](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=rudiger); [Project Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: bath -> hath; Restore source-verified Gernot speech before Q&A. Synthetic Q&A unchanged.
- Original text SHA-256: `2f4410b98b7ef4648d98dafdc900386404851c64952e31b1c1edd7471ab384ee`; revised SHA-256: `0cad4fe867fea5bb6eb024f915ecddb5388a89df87f3e8c2accb4ad2e1bf44e1`.
- Exact edit anchors and zero-based offsets before each edit: `[('bath been so hacked and hewed', 623), ("Seizing his arms, Rudiger rushed upon the Burgundians and the strife began once more. Hagen and Volker stood aside, nor did Giselher seek to meet his sword; but deep were the wounds it dealt, and many the knights that fell before it. Rudiger's liegemen followed him, and soon the hall was filled with the din of battle.\n\nQuestion:", 1972)]`.
- Scope: this exact source ID and hash only; no global spelling or punctuation substitution.
- Labels: classification pending because no precise supplied disease code covers these editorial defects; do not insert blank-code fix labels or claim clean.
- Validation: inverse edits reconstruct original exactly; Q&A identical; source Parquet unchanged. Harm test pending Data A / Data B.
- Side effects: cross-edition changes can override intentional source wording. Restoring missing text changes the sample; original source retained for exact reversal. Verse wording shared by both editions preserved; restored paragraph is an explicit external-source addition, not recovered pack text.
- Status: applied locally, not pushed. Audit: `changed-rows-review.md` and batch review JSON.

```python
import hashlib
assert hashlib.sha256(text179.encode('utf-8')).hexdigest() == '2f4410b98b7ef4648d98dafdc900386404851c64952e31b1c1edd7471ab384ee'
edits = [('bath been so hacked and hewed', 'hath been so hacked and hewed'), ("Seizing his arms, Rudiger rushed upon the Burgundians and the strife began once more. Hagen and Volker stood aside, nor did Giselher seek to meet his sword; but deep were the wounds it dealt, and many the knights that fell before it. Rudiger's liegemen followed him, and soon the hall was filled with the din of battle.\n\nQuestion:", 'Seizing his arms, Rudiger rushed upon the Burgundians and the strife began once more. Hagen and Volker stood aside, nor did Giselher seek to meet his sword; but deep were the wounds it dealt, and many the knights that fell before it. Rudiger\'s liegemen followed him, and soon the hall was filled with the din of battle.\nWhen Gernot saw the terrible havoc Rudiger\'s sword wrought among the Burgundians, he shouted: "I pray thee, cease, Sir Rudiger! Now must I seek vengeance for my true liegemen thou hast slain and thereby turn thy gift against thyself!"\n\nQuestion:')]
cleaned = text179
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text179
```

## Rows 180-181: source-verified sentence continuation (C3)

- Documents: `a4ac4737059e936a` and `1ff0d9db2ef30443`. Row 181 is supporting boundary context, not a completed label in this batch.
- Evidence: row 180 ends inside a quotation; row 181 continues the same sentence. Both have matching source URL and WARC record ID.
- References: [Heritage History, Hildebrand and Dietrich](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=hildebrand); [Project Gutenberg, Chapter XXXII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Action: join bodies with one space at zero-based offset `2660`; preserve both source Q&A blocks separately and link both source IDs.
- Validation and reversal: split before/after the inserted space and reattach each Q&A; both original source hashes match exactly. No source characters deleted or rewritten; one space added. Source Parquet unchanged.
- Limitation: Supporting row 181 ends mid-verse; leave its 181-182 boundary for next review. Preserve source backslash and source punctuation. Joined output is not claimed complete.
- Harm test: pending Data A / Data B. Q&A extraction can alter context; apply only to these exact source hashes.
- Label: row 180 fix/C3, evidence tied to the original body ending; no label added for 181 yet.
- Status: applied locally, not pushed; audit stores originals and output.

```python
import hashlib
texts = (text180, text181)
expected = ['a4ac4737059e936a0b3613b82f5fdffc5949215b5bf47368e6a3f68953bd3088', '1ff0d9db2ef304438f2091a78e5ed759aea33e851b9912498ee9b7a765422559']
parts = []
for text, digest in zip(texts, expected):
    assert hashlib.sha256(text.encode('utf-8')).hexdigest() == digest
    boundary = text.index("\n\nQuestion:")
    parts.append((text[:boundary], text[boundary:]))
joined_body = " ".join(body for body, qa in parts)
qa_blocks = [qa for body, qa in parts]
join_offset = len(parts[0][0])
assert joined_body[:join_offset] + qa_blocks[0] == text180
assert joined_body[join_offset+1:] + qa_blocks[1] == text181
```

## Preserved rows and classification follow-up

Rows 174 and 176 were read in full, compared with sources, and labelled clean without text changes. Row 171 is now repaired; its prior hold is superseded. Rows 171, 175, 177, 178, and 179 have source-checked editorial repairs but no precise supplied disease category; their labels remain pending. Do not treat missing chapter selection in row 177 as proven end-truncation or claim the repaired sample is complete. Gold tests remain pending.

## Row 171: source-verified dialogue and Q&A repairs

- Document: `b24bfebb52c88259`. The earlier hold is superseded by the explicit request to check sources and apply the repair.
- Sources: [Project Gutenberg, Chapters XXVI-XXVII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm); [Heritage History, The Feast](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=feast). The Heritage History Dankwart chapter was unavailable; no independent check of that chapter is claimed.
- Verification: Gutenberg Chapters XXVI-XXVII confirms the quotation, sooth reading and omitted reply. Heritage History feast chapter independently reproduces the missing closing quote; Heritage History dankwart chapter returned a verification page and is not claimed verified. First Q&A revised using the explicit royal-court alibi in the original body.
- Applied action: Restore closing quote; Booth -> sooth; restore omitted Blodelin reply; remove stray pre-quote space; reframe first Q&A around source-supported innocence claim. Keep row separate.
- Context check: Compared with rows 170-180 and applied outputs: 170-171 and 171-172 are complete narrative boundaries, not mid-sentence/verse joins. The omitted reply belongs inside 171 and is not recovered by a neighbouring-record join.
- Restoration detail: inserted Blodelin reply is from Gutenberg; straight quotes, unaccented name and spaced apostrophe style follow the pack. These are documented external-source additions, not recovered Parquet characters.
- Q&A: original first pair retained in local audit; replacement asks about the existing royal-court alibi instead of asserting refusal to fight. Second Q&A unchanged. No answers elsewhere are rewritten.
- Source SHA-256: `b24bfebb52c88259fdc97af8b6f0598cea9cf1b4cf2b36a068583ce192be5e5f`. Revised SHA-256: `f7178292a9a7e92c79afbe014c7b9560093be2e7a775dea9c9c2767ba2e0e8f8`.
- Edits below run in sequence; exact offsets before each edit: `[859, 1827, 2102, 2296, 2560]`. Reverse in the opposite order to reconstruct the exact original.
- Label status: pending exact disease-code classification; no unsupported end-truncation/markup code, blank-code fix label, or clean claim added. `labels.csv` remains at 175 records while five reviewed editorial-repair rows await classification.
- Side effects: external-edition restoration and synthetic-Q&A rewriting change the sample. Restrict to this exact ID and source hash; retain original body and Q&A. Not a general corpus rule.
- Validation: all unique-anchor and inverse-edit checks passed; second Q&A unchanged; other rows 172-180 and their applied outputs unchanged; original Parquet SHA-256 remains `0f1c3a9866ada7fb5ea2a244135d9766375351bec366c13cd9c5abfb3d82573c`.
- Harm test: pending Data A / Data B; reconstruction does not prove gold-set safety. Applied locally, not pushed.

```python
import hashlib
assert hashlib.sha256(text171.encode('utf-8')).hexdigest() == 'b24bfebb52c88259fdc97af8b6f0598cea9cf1b4cf2b36a068583ce192be5e5f'
edits = [('imprinted on his brow!\n', 'imprinted on his brow!"\n'), ('in good Booth to hold thee', 'in good sooth to hold thee'), ('\n"Gramercy!" quoth Dankwart,', '\n"What thou hast done concerns me naught," replied Blodelin, "it is enough that \'t was thy kinsman did the deed; and therefore must thou die!"\n"Gramercy!" quoth Dankwart,'), ('I had spared my words ";', 'I had spared my words";'), ("Question: What is the reason for Dankwart's refusal to fight Blodelin? Answer: Dankwart refuses to fight Blodelin because he was not present when Siegfried was killed, and thus feels he is not the guilty party.", 'Question: Why did Dankwart claim that vengeance against him would punish the guiltless? Answer: He was at the royal court when Siegfried was killed beside the spring.')]
cleaned = text171
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text171
```

## Rows 180-182: extend the sentence join through the split verse

- Documents: `a4ac4737059e936a`, `1ff0d9db2ef30443`, `f8b53427fe40a49e`.
- Supersedes the earlier 180-181 output. Emit exactly one body for all three source IDs, not overlapping two-row joins.
- Reason: row 181 begins by continuing 180 and ends inside verse completed by 182. Both [Gutenberg, Chapter XXXII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) and [Heritage History](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=hildebrand) confirm the sequence.
- Editorial repair: remove one backslash from the exact anchor in row 181 at zero-based source offset `774`. Heritage History contains it; Gutenberg provides the clean reading. This is a deliberate parallel-edition correction.
- Join separators: space after body 180, newline after corrected body 181. Zero-based join offsets `[2660, 5304]` in the resulting body. Retain all three Q&A blocks unchanged with their respective source IDs.
- Unresolved: Row 182 proposed quotation, outdid and first-Q&A corrections have not been applied. Joined body is not claimed completely clean.
- Labels: source rows 181 and 182 fix/C3 for the verse split; note pending source/Q&A issues rather than claim output clean. Exact-code questions for editorial repairs elsewhere remain pending.
- Validation: all three originals reconstruct exactly after removing the two inserted separators and reversing the backslash edit. Q&A unchanged. Source Parquet unchanged; gold harm tests pending Data A / Data B.
- Side effects: joins and Q&A extraction alter context; restrict to these exact source IDs and hashes. Apply no global backslash removal.
- Applied locally, not pushed. Batch JSON contains source pieces, hashes, offsets and the sole active joined output.

```python
import hashlib
texts = [text180, text181, text182]
expected = ['a4ac4737059e936a0b3613b82f5fdffc5949215b5bf47368e6a3f68953bd3088', '1ff0d9db2ef304438f2091a78e5ed759aea33e851b9912498ee9b7a765422559', 'f8b53427fe40a49ecb2a2a9d8f2d2013828dde6c9a7886c734be3e12bedb82cb']
assert [hashlib.sha256(t.encode('utf-8')).hexdigest() for t in texts] == expected
old, new = 'the\\ knights', 'the knights'
assert text181.count(old) == 1
fixed181 = text181.replace(old, new, 1)
parts = []
for text in (text180, fixed181, text182):
    k = text.index("\n\nQuestion:")
    parts.append((text[:k], text[k:]))
joined_body = parts[0][0] + " " + parts[1][0] + "\n" + parts[2][0]
qa_blocks = [qa for body, qa in parts]
a = len(parts[0][0])
b = a + 1 + len(parts[1][0])
assert joined_body[:a] + qa_blocks[0] == text180
assert (joined_body[a+1:b] + qa_blocks[1]).replace(new, old, 1) == text181
assert joined_body[b+1:] + qa_blocks[2] == text182
```

## Row 182: source-checked verse and synthetic-Q&A repairs

- Document: `f8b53427fe40a49e`; already classified fix/C3 for the original split verse. This follows the user request to fix the remaining source-checked defects.
- Verification: Heritage History reproduces the missing stanza quote and out did spacing; Gutenberg Chapter XXXII has the opening quote and outdid. Both narrative contexts identify Volker as the minstrel speaking about Wolfhart. Synthetic Q&A is corrected from narrative evidence, not quoted from either source.
- References: [Gutenberg, Chapter XXXII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm); [Heritage History, Hildebrand and Dietrich](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=hildebrand).
- Applied repairs: restore one opening quote before the stanza; change only the exact out-did anchor to the Gutenberg spelling; reframe first Q&A to identify Volker and the correct target Wolfhart.
- Original first Q&A and body retained below. Second Q&A unchanged. First Q&A is an explicit editorial correction based on narrative context.
- Source SHA-256 `f8b53427fe40a49ecb2a2a9d8f2d2013828dde6c9a7886c734be3e12bedb82cb`; revised `f42f99210ae41e8c8c24812a35cdb83099076bc5bb49a3a6f70cd66cd6f148ba`. Sequential edit offsets: `[61, 829, 2604]`.
- Update the sole active 180-182 body, replacing its earlier version; do not emit duplicate joined records. Separators remain space and newline; join offsets remain `[2660, 5304]`. Link all three source IDs.
- Validation: inverse edits reconstruct row 182 exactly; splitting the active body and reattaching saved source Q&A reconstructs all three source records after reversing the documented edits. Original Parquet unchanged; 177 unique label records remain.
- Side effects: parallel-edition spelling/quotation corrections and Q&A rewriting change the sample. Apply only to the exact source hash; preserve source and old output for reversal.
- Gold-set harm tests pending Data A / Data B; no claim of gold-test safety. Applied locally, not pushed. Previous statements that row 182 repairs are pending are historical and superseded by this section.

```python
import hashlib
assert hashlib.sha256(text182.encode('utf-8')).hexdigest() == 'f8b53427fe40a49ecb2a2a9d8f2d2013828dde6c9a7886c734be3e12bedb82cb'
edits = [('\nAnd though he may have slain a host', '\n"And though he may have slain a host'), ('out did all the rest', 'outdid all the rest'), ("Question: Who was the minstrel who vowed to smite Hagen? Answer: Dietrich's men", 'Question: Which minstrel threatened to strike Wolfhart? Answer: Volker')]
cleaned = text182
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text182
```

## Row 184: source-checked quotation and Q&A clarification

- Document: `cf90f9bc9cbe33c8`. Keep this record separate from 183 and 185; its body ends in a complete sentence.
- Verification: Heritage History repeats the missing closing quote; Gutenberg Chapter XXXIII supplies it. Both sources attribute the explanation about Wolfhart to Gunther. The second synthetic question is clarified from this narrative; its answer is unchanged.
- References: [Heritage History, Kriemhild's Revenge](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=revenge); [Gutenberg, Chapter XXXIII](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Applied action: Restore the closing quote after friends and warriors?; clarify the second question about who attributed the strife to Wolfhart. Keep Gunther as answer, first Q&A unchanged and row separate.
- Original SHA-256 `cf90f9bc9cbe33c8915f4ab44d46589b07be1b7d1f5a42512f7657a683b042d5`; revised `0bd39cf412aebd59b5a85fec3219376bdf23deaaa616d0e657850f325db00d0a`. Exact sequential edit offsets: `[230, 2648]`.
- Validation: inverse edits reconstruct the original exactly; first Q&A and both answers unchanged. Only the closing quote and second question change. Original Parquet unchanged.
- Label status: punctuation and question ambiguity lack a precise supplied code; classification pending. Do not label clean or invent a code. Existing 177 label records unchanged.
- Side effects: quotation repair follows the parallel edition; clarifying synthetic questions changes the sample. Use this source ID and exact hash only; original text and Q&A retained locally for reversal.
- Gold harm test pending Data A / Data B; reconstruction is not a gold-test result. Applied locally, not pushed.

```python
import hashlib
assert hashlib.sha256(text184.encode('utf-8')).hexdigest() == 'cf90f9bc9cbe33c8915f4ab44d46589b07be1b7d1f5a42512f7657a683b042d5'
edits = [('all my friends and warriors?\n', 'all my friends and warriors?"\n'), ('Question: Who spoke after Dietrich? Answer: Gunther', 'Question: Who said that Wolfhart grew insolent and caused the strife? Answer: Gunther')]
cleaned = text184
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text184
```

## Row 183: completed review and bounded repairs

- Document: `dc2b689db17c58fe`.
- Verification and limitations: Gutenberg Chapters XXXII-XXXIII confirms opening quote before Alas and closing quote after joy on earth. Narrative states that Hagen wounded Hildebrand and Hildebrand killed Volker; the first synthetic Q&A reverses these facts.
- References attempted/checked: [Source page](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=hildebrand), [Source page](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=revenge), [Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Action: Restore two quotes; correct first Q&A to Hagen wounding Hildebrand; keep separate.
- Scope: exact source ID and hash only. No global punctuation or boilerplate deletion rule.
- Original SHA-256 `dc2b689db17c58fe427435d332a850af166b31bbac66737c5f3c8064bc6ef2b6`; revised `14587b9766f0336e0274a3cd2f4341dcaf897e49642ab78e3994f10dfb0935a1`. Sequential source/edit offsets: `[1778, 2075, 2776]`.
- Validation: every edit reversed exactly; source Parquet unchanged. Synthetic Q&A is explicitly revised, with originals retained.
- Label: classification pending; no unsupported code added. Gold tests pending Data A / Data B; no gold pass claimed. Applied locally, not pushed.
- Side effects: source-version or editorial differences can be legitimate. Retain original data; do not extrapolate exact deletion/rewriting rules to unrelated records.

```python
import hashlib
assert hashlib.sha256(text183.encode('utf-8')).hexdigest() == 'dc2b689db17c58fe427435d332a850af166b31bbac66737c5f3c8064bc6ef2b6'
edits = [('\nAlas! my lord,"', '\n"Alas! my lord,"'), ('last day of my joy on earth!\n', 'last day of my joy on earth!"\n'), ('Question: Who was the first to be killed by Hagen? Answer: Sir Volker', 'Question: Who wounded Hildebrand before he fled the hall? Answer: Hagen')]
cleaned = text183
undo = []
for old, new in edits:
    assert cleaned.count(old) == 1
    offset = cleaned.index(old)
    undo.append((offset, old, new))
    cleaned = cleaned[:offset] + new + cleaned[offset + len(old):]
restored = cleaned
for offset, old, new in reversed(undo):
    assert restored[offset:offset + len(new)] == new
    restored = restored[:offset] + old + restored[offset + len(new):]
assert restored == text183
```

## Row 185: completed review and bounded repairs

- Document: `59d846af6f37b0a8`.
- Verification and limitations: Gutenberg Chapter XXXIII confirms both missing opening quotes and the verse continuation in 186. The last synthetic Q&A invents a plea absent from the body. Clarify it using the explicit order to chain Gunther; preserve other answers.
- References attempted/checked: [Source page](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=revenge), [Gutenberg](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm).
- Action: Restore two quotes; clarify first and last Q&A; join body with 186 using newline.
- Scope: exact source ID and hash only. No global punctuation or boilerplate deletion rule.
- Original SHA-256 `59d846af6f37b0a88cb9dcc756b66cb5c3efd472f3e218e1a23a0a149b6aaa8a`; revised `5dcd3bf5f6dc239b27d7402beb3e0fcddd6c5a7751644bd34aec167dafbc2732`. Sequential source/edit offsets: `[246, 1719, 2864, 2590]`.
- Validation: every edit reversed exactly; source Parquet unchanged. Synthetic Q&A is explicitly revised, with originals retained.
- Label: fix / C3. Gold tests pending Data A / Data B; no gold pass claimed. Applied locally, not pushed.
- Side effects: source-version or editorial differences can be legitimate. Retain original data; do not extrapolate exact deletion/rewriting rules to unrelated records.

```python
import hashlib
assert hashlib.sha256(text185.encode('utf-8')).hexdigest() == '59d846af6f37b0a88cb9dcc756b66cb5c3efd472f3e218e1a23a0a149b6aaa8a'
edits = [('\nBe merciful, O Queen!', '\n"Be merciful, O Queen!'), ('\nNow will I quickly make an end', '\n"Now will I quickly make an end'), ("Question: How did Kriemhild respond to Gunther's plea for Hagen's release? Answer: She ignored his plea and ordered Gunther to be chained and cast into a separate dungeon.", 'Question: What did Kriemhild order for Gunther after Dietrich departed? Answer: She ordered him to be chained and cast into a separate dungeon.'), ("Question: What was the last action Kriemhild took before the text ends? Answer: She swung Siegfried's sword with both hands, intending to strike Hagen.", "Question: What did Kriemhild do with Siegfried's sword before striking Hagen? Answer: She swung it with both hands, intending to strike Hagen.")]
cleaned = text185
undo = []
for old, new in edits:
    assert cleaned.count(old) == 1
    offset = cleaned.index(old)
    undo.append((offset, old, new))
    cleaned = cleaned[:offset] + new + cleaned[offset + len(old):]
restored = cleaned
for offset, old, new in reversed(undo):
    assert restored[offset:offset + len(new)] == new
    restored = restored[:offset] + old + restored[offset + len(new):]
assert restored == text185
```

## Row 188: completed review and bounded repairs

- Document: `35ebd1eaea214130`.
- Verification and limitations: Original course endpoint and alternate university course URL unavailable. The supplied record has an empty Lesson period footer and a generic administrative instruction. Remove only these C1 spans; do not infer a timetable or restore unavailable content.
- References attempted/checked: [Source page](https://default-unimi-d8cl1.prod.cineca.it/en/education/degree-programme-courses/2021/economic-botany-and-zoology).
- Action: Remove only recorded empty administrative/footer or navigation spans; Q&A unchanged.
- Scope: exact source ID and hash only. No global punctuation or boilerplate deletion rule.
- Original SHA-256 `35ebd1eaea21413069bb10cf97dbbfdf234dd6424cffce45baa8e3f7da9ac166`; revised `d045cd32f4b3726b54945e2a008d92d22f595a72cd09008c0f155f2a07266116`. Sequential source/edit offsets: `[850]`.
- Validation: every edit reversed exactly; source Parquet unchanged. Synthetic Q&A unchanged.
- Label: fix / C1. Gold tests pending Data A / Data B; no gold pass claimed. Applied locally, not pushed.
- Side effects: source-version or editorial differences can be legitimate. Retain original data; do not extrapolate exact deletion/rewriting rules to unrelated records.

```python
import hashlib
assert hashlib.sha256(text188.encode('utf-8')).hexdigest() == '35ebd1eaea21413069bb10cf97dbbfdf234dd6424cffce45baa8e3f7da9ac166'
edits = [('Lesson period:\n(In case of multiple editions, please check the period, as it may vary)\n', '')]
cleaned = text188
undo = []
for old, new in edits:
    assert cleaned.count(old) == 1
    offset = cleaned.index(old)
    undo.append((offset, old, new))
    cleaned = cleaned[:offset] + new + cleaned[offset + len(old):]
restored = cleaned
for offset, old, new in reversed(undo):
    assert restored[offset:offset + len(new)] == new
    restored = restored[:offset] + old + restored[offset + len(new):]
assert restored == text188
```

## Row 189: completed review and bounded repairs

- Document: `dc29e8d2291caee1`.
- Verification and limitations: Current publisher page no longer contains the archived passage. Remove the two adjacent empty navigation headings only. Preserve grammar, dates, historical claims and Q&A, with factual/source-version concerns retained.
- References attempted/checked: [Source page](https://www.basketballengland.co.uk/get-involved/play/getting-started/).
- Action: Remove only recorded empty administrative/footer or navigation spans; Q&A unchanged.
- Scope: exact source ID and hash only. No global punctuation or boilerplate deletion rule.
- Original SHA-256 `dc29e8d2291caee1639f3daa97f9ce300df36951e1b9d828b53fc4a2e91b9973`; revised `cdf49dbe4bd3143960f29feb0d05b0423fd58abd88e0bd7aec18d06adaabb1a8`. Sequential source/edit offsets: `[487]`.
- Validation: every edit reversed exactly; source Parquet unchanged. Synthetic Q&A unchanged.
- Label: fix / C1. Gold tests pending Data A / Data B; no gold pass claimed. Applied locally, not pushed.
- Side effects: source-version or editorial differences can be legitimate. Retain original data; do not extrapolate exact deletion/rewriting rules to unrelated records.

```python
import hashlib
assert hashlib.sha256(text189.encode('utf-8')).hexdigest() == 'dc29e8d2291caee1639f3daa97f9ce300df36951e1b9d828b53fc4a2e91b9973'
edits = [('THE BASICS OF THE GAME\nPOSITIONS\n', '')]
cleaned = text189
undo = []
for old, new in edits:
    assert cleaned.count(old) == 1
    offset = cleaned.index(old)
    undo.append((offset, old, new))
    cleaned = cleaned[:offset] + new + cleaned[offset + len(old):]
restored = cleaned
for offset, old, new in reversed(undo):
    assert restored[offset:offset + len(new)] == new
    restored = restored[:offset] + old + restored[offset + len(new):]
assert restored == text189
```

## Rows 185-186: complete the split verse

- Source IDs: `59d846af6f37b0a8`, `271e09d4d18ea32c`. Gutenberg Chapter XXXIII confirms the continuation; source URL and WARC record match.
- Apply Row185 repairs above first, then separate each record at the first `\n\nQuestion:` and join bodies with one newline at offset `2588`. Keep original and revised Q&A provenance separately.
- Reversal: split before/after the newline, reattach saved Q&A, and reverse the Row185 edits. Both original hashes match. Row186 Q&A unchanged.
- Labels: both fix/C3. Side effect: extraction changes Q&A context; restrict to exact source hashes. Sources contain a concluding verse absent after 186; no mid-sentence cutoff is established at the complete prose ending. Do not fabricate that verse.
- Status: local, not pushed; gold tests pending.

```python
body185, marker185, qa185 = cleaned185.partition("\n\nQuestion:")
body186, marker186, qa186 = text186.partition("\n\nQuestion:")
assert marker185 and marker186
joined_body = body185 + "\n" + body186
qa_blocks = [marker185 + qa185, marker186 + qa186]
assert joined_body[:len(body185)] + qa_blocks[0] == cleaned185
assert joined_body[len(body185)+1:] + qa_blocks[1] == text186
```

## Row 190: F1 redaction and C1 boilerplate removal

- Document: `1669b86835f433ae`. Source page unavailable; findings based on full supplied pack text, not claimed web verification.
- Evidence: person name at offset 476, email at offset 1118, person name at offset 1436, email at offset 1678. Navigation at offset 0; comment/sidebar block begins at offset 1369.
- Action: redact only the identified privacy spans; remove leading Pages and comment/sidebar block. Preserve post, poem and both synthetic Q&A pairs. Do not store private values in evidence, code or audit. Hex empty for F1.
- Source hash `1669b86835f433ae6dfc72a3b1ca10f363efe0c7f6ae9db829cbbaaa06ee4c0c`; revised `93d2e135a9182c0cbb0ab5c3db049d578b1f172043582ad421ec52370dffebdb`. Reversal requires exact original Parquet retained outside the repository; do not copy removed personal data into a reversal record.
- Side effects: boilerplate may contain useful author/context information; redaction changes attribution. Strictly restricted to this source ID/hash; no global pattern-based corpus cleaning.
- Validation: source hash unchanged; no contacts in derived output or newly generated evidence; exact Q&A unchanged; both retained text sections match the source after documented redaction. Gold tests pending Data A / Data B; local, not pushed.

```python
import hashlib
assert hashlib.sha256(text190.encode('utf-8')).hexdigest() == '1669b86835f433ae6dfc72a3b1ca10f363efe0c7f6ae9db829cbbaaa06ee4c0c'
spans = [{'type': 'person name', 'source_offset': 476, 'length': 11}, {'type': 'email', 'source_offset': 1118, 'length': 23}, {'type': 'person name', 'source_offset': 1436, 'length': 12}, {'type': 'email', 'source_offset': 1678, 'length': 23}]
cleaned = text190
for span in reversed(spans):
    start = span['source_offset']
    end = start + span['length']
    marker = "[" + span['type'] + " REDACTED; source offset " + str(start) + "]"
    cleaned = cleaned[:start] + marker + cleaned[end:]
assert cleaned.startswith("Pages\n")
cleaned = cleaned[len("Pages\n"):]
start = cleaned.index("\nNo comments:")
end = cleaned.index("\n\nQuestion:", start)
cleaned = cleaned[:start] + cleaned[end:]
assert hashlib.sha256(cleaned.encode('utf-8')).hexdigest() == '93d2e135a9182c0cbb0ab5c3db049d578b1f172043582ad421ec52370dffebdb'
```


## Row 171: classification resolved

- Reviewer explicitly selected **fix / C3** for the source-verified internal dialogue omission. This is an internal omission, not a claimed end-of-document cutoff.
- Evidence: Missing Blodelin reply before "Gramercy!" quoth Dankwart, at offset 2102; verified in Gutenberg Chapter XXVII.
- Added the original source ID `b24bfebb52c88259` to labels.csv. The previously applied text repairs and their source/reversal records remain unchanged.
- Row171 is no longer classification-pending. Gold tests remain pending; local update, not pushed.


## Row 175: fix verdict confirmed

- Verdict: **fix**, confirmed by reviewer. Closing quote and hire/him, cloth/doth repairs remain applied; exact reversal rechecked.
- Disease code remains pending because the supplied taxonomy has no precise prose-punctuation/OCR category. No unsupported code or blank-code fix CSV row added.
- Original and revised text remain in the existing Row175 audit. Labels unchanged; local only, gold tests pending.

## Row 177: restore the source-verified missing chapter

- Document: `976023f4a0f3f62b`. Supersedes the earlier stray-word-only repair and unresolved omission status.
- Sources: [Gutenberg, Chapter XXX](https://www.gutenberg.org/files/73227/73227-h/73227-h.htm) and [Heritage History, The Night of Terror](https://www.heritage-history.com/index.php?c=read&author=upton&book=nibelungs&story=terror) verify the chapter belongs between the two retained sections.
- Action: retain removal of stray save; insert the complete Gutenberg chapter before The Margrave Rudiger. Existing body text and synthetic Q&A otherwise unchanged. No join with neighbouring sample records.
- External-source provenance: downloaded reference HTML SHA-256 `1c927aaffb18a6736ff0a8d5a50510d59277421e86d0a86bccd43ac6aa60c0d4`; extracted chapter SHA-256 `a0806d05410a5b2426a4356cecc47f5b1c51f00a023f6202a22a7464a2f7cac3`; 4014 added chapter characters, 14 prose paragraphs and intervening verse. HTML page numbers/illustrations removed; typography whitespace flattened; decorative NOT changed to Not. Gutenberg substantive wording, Unicode quotation marks and names retained, not rewritten to match Heritage OCR.
- Label: fix/C3 for internal omission, using the same interpretation selected for row171. Evidence explicitly describes an internal chapter gap, not a claimed final-sentence cutoff.
- Original source hash `976023f4a0f3f62ba54afbc3109c77764b3cdeed55300be08e3a3b7659327e01`; revised `45c76fc12ebe7aacf9872db1f7b62b99f37cadd8480c8e4a2bc8473807a67a83`. Exact old/new anchors below support full reversal. Original Parquet retained unchanged.
- Validation: two inverse edits reproduce the exact original; original Q&A identical; chapter appears once; other review rows unchanged. This is an external-edition restoration, not proof of the original synthetic record's intended completeness.
- Side effects: inserts substantial material into a synthetic sample and changes its length/content distribution. Strictly source-ID/hash-specific; no automatic insertion in other documents. Gold-set harm tests and second-reader agreement pending.
- Local, not pushed. Original, imported chapter, revised output, extraction provenance and hashes retained in batch JSON and this audit.

```python
import hashlib
assert hashlib.sha256(text177.encode('utf-8')).hexdigest() == '976023f4a0f3f62ba54afbc3109c77764b3cdeed55300be08e3a3b7659327e01'
chapter = 'The Night of Terror\n\nNot long were the Burgundians to enjoy repose, however, for fresh bodies of Huns came pouring in from all sides, and soon Etzel and Kriemhild had mustered another force; nor was it easy to repel them, by reason of their great numbers.\nThe onslaught and defence did last\nTill hindered by the night.\nThose dauntless guests beyond all praise\nWith Etzel’s men did fight\nThe whole of one long Summer’s day—\nSo do the legends tell—\nHella! what thousands of brave men\nBy Burgund weapons fell.\n\nThus night came on, and gladly would the war-worn heroes have sought a speedy death. Their thoughts also turned toward peace, and they made known to the Huns that they would have speech with the King. Word was sent to Etzel, and he forthwith appeared, with Kriemhild, before the palace, upon the stairway of which stood the three princes in their blood-stained armor, and behind them Hagen and Volker and Dankwart.\n\nThen Etzel spoke: “What would ye have with me? ’Twere vain to sue for peace, for never so long as I have breath shall it be granted to those who so cruelly have slain my only child and many of my kin!”\n\nGünther answered that it had been forced upon them, while Giselher, turning to the Huns, cried out:\n\n“What charge against me do ye bring? Did I not come hither in all peace and friendship to your land?”\n\nThe knights replied: “Already hath such friendship cost us dear! Full many a widowed wife and orphaned child have bitter cause to wish thou ne’er hadst left the Rhine!”\n\nAgain Günther sought to make peace; but Etzel would hear naught thereof. “Your griefs are not as mine,” he said, “for to my loss is added shame; nor may it be effaced save with your blood. Wherefore, I say, not one of you shall depart hence with his life!”\nThen said the youthful Giselher:\n“O beauteous sister mine,\nI little treachery feared when thou\nDidst bid me cross the Rhine.\n“To thee I’ve ever faithful been,\nNor grief nor sorrow wrought,\nBelieving that thou heldst me dear,\nTo bring thee joy I sought.\nNor can I think thou’lt wish me ill;\nBe still my sister true!\nWith kindly judgment look on us,\nSure less thou canst not do.”\n\nOnce more a ray of love warmed Kriemhild’s sorely wounded heart, and she replied:\n“Now if Sir Hagen you’ll agree\nInto my hands to give,\nThen will I not refuse to grant\nThat ye three all shall live;\nSince ye in truth my brethren are\nAnd from one mother spring,\nOf further mercy I’ll consult\nWith those about the King.”\n\n“Now, Heaven forbid!” cried Gernot. “Far rather death than such disloyalty!”\n\nAnd Giselher said: “Never yet did I break faith with comrade, nor will I fail to-day in knightly duty.”\n\nThus was the last spark of affection extinguished in Kriemhild’s bosom. She no longer felt pity even for her own kin, since they had refused to deliver over to her the slayer of her Siegfried and her child. Summoning the Huns, she bade them fire the palace on all sides.\n\nNow was it in truth a night of terror for the Burgundians. Blazing shafts flew on to the roof, and soon it was wrapped in flame. Thick smoke and fiery vapors filled the hall, and the heroes suffered tortures. Gladly would they have welcomed a swift death in battle. Many called on God to pity their distress, and one knight cried out woefully for a drop of water. Hagen shouted: “Keep to the walls, my comrades, and raise your shields aloft!”\n\nAnd still that dauntless hero, with the brave Volker, kept unceasing watch before the door. At daybreak Volker said to Hagen: “Let us within, and then perchance the watchful Huns will think we have perished.”\n\nAnd so in truth did Etzel and Queen Kriemhild now believe. Yet still six hundred of them were alive. When this news was brought to Kriemhild she caused a great store of gold to be brought and divided among the Huns, with which to spur their valor. Thereupon a thousand of the boldest rushed to the assault; but though many of the Burgundians too were slain, not one of Kriemhild’s warriors came forth from that dread hall of death to claim his gold.'
assert hashlib.sha256(chapter.encode('utf-8')).hexdigest() == 'a0806d05410a5b2426a4356cecc47f5b1c51f00a023f6202a22a7464a2f7cac3'
edits = [('it had ever occurred. save\nThe Margrave Rudiger', 'it had ever occurred.\nThe Margrave Rudiger'), ('\nThe Margrave Rudiger\n', '\n\nThe Night of Terror\n\nNot long were the Burgundians to enjoy repose, however, for fresh bodies of Huns came pouring in from all sides, and soon Etzel and Kriemhild had mustered another force; nor was it easy to repel them, by reason of their great numbers.\nThe onslaught and defence did last\nTill hindered by the night.\nThose dauntless guests beyond all praise\nWith Etzel’s men did fight\nThe whole of one long Summer’s day—\nSo do the legends tell—\nHella! what thousands of brave men\nBy Burgund weapons fell.\n\nThus night came on, and gladly would the war-worn heroes have sought a speedy death. Their thoughts also turned toward peace, and they made known to the Huns that they would have speech with the King. Word was sent to Etzel, and he forthwith appeared, with Kriemhild, before the palace, upon the stairway of which stood the three princes in their blood-stained armor, and behind them Hagen and Volker and Dankwart.\n\nThen Etzel spoke: “What would ye have with me? ’Twere vain to sue for peace, for never so long as I have breath shall it be granted to those who so cruelly have slain my only child and many of my kin!”\n\nGünther answered that it had been forced upon them, while Giselher, turning to the Huns, cried out:\n\n“What charge against me do ye bring? Did I not come hither in all peace and friendship to your land?”\n\nThe knights replied: “Already hath such friendship cost us dear! Full many a widowed wife and orphaned child have bitter cause to wish thou ne’er hadst left the Rhine!”\n\nAgain Günther sought to make peace; but Etzel would hear naught thereof. “Your griefs are not as mine,” he said, “for to my loss is added shame; nor may it be effaced save with your blood. Wherefore, I say, not one of you shall depart hence with his life!”\nThen said the youthful Giselher:\n“O beauteous sister mine,\nI little treachery feared when thou\nDidst bid me cross the Rhine.\n“To thee I’ve ever faithful been,\nNor grief nor sorrow wrought,\nBelieving that thou heldst me dear,\nTo bring thee joy I sought.\nNor can I think thou’lt wish me ill;\nBe still my sister true!\nWith kindly judgment look on us,\nSure less thou canst not do.”\n\nOnce more a ray of love warmed Kriemhild’s sorely wounded heart, and she replied:\n“Now if Sir Hagen you’ll agree\nInto my hands to give,\nThen will I not refuse to grant\nThat ye three all shall live;\nSince ye in truth my brethren are\nAnd from one mother spring,\nOf further mercy I’ll consult\nWith those about the King.”\n\n“Now, Heaven forbid!” cried Gernot. “Far rather death than such disloyalty!”\n\nAnd Giselher said: “Never yet did I break faith with comrade, nor will I fail to-day in knightly duty.”\n\nThus was the last spark of affection extinguished in Kriemhild’s bosom. She no longer felt pity even for her own kin, since they had refused to deliver over to her the slayer of her Siegfried and her child. Summoning the Huns, she bade them fire the palace on all sides.\n\nNow was it in truth a night of terror for the Burgundians. Blazing shafts flew on to the roof, and soon it was wrapped in flame. Thick smoke and fiery vapors filled the hall, and the heroes suffered tortures. Gladly would they have welcomed a swift death in battle. Many called on God to pity their distress, and one knight cried out woefully for a drop of water. Hagen shouted: “Keep to the walls, my comrades, and raise your shields aloft!”\n\nAnd still that dauntless hero, with the brave Volker, kept unceasing watch before the door. At daybreak Volker said to Hagen: “Let us within, and then perchance the watchful Huns will think we have perished.”\n\nAnd so in truth did Etzel and Queen Kriemhild now believe. Yet still six hundred of them were alive. When this news was brought to Kriemhild she caused a great store of gold to be brought and divided among the Huns, with which to spur their valor. Thereupon a thousand of the boldest rushed to the assault; but though many of the Burgundians too were slain, not one of Kriemhild’s warriors came forth from that dread hall of death to claim his gold.\n\nThe Margrave Rudiger\n')]
cleaned = text177
for old, new in edits:
    assert cleaned.count(old) == 1
    cleaned = cleaned.replace(old, new, 1)
restored = cleaned
for old, new in reversed(edits):
    assert restored.count(new) == 1
    restored = restored.replace(new, old, 1)
assert restored == text177
```

## Reproducibility
The document-specific code above records the new transformations for rows 16, 34, 75-76, 81, 109-110, 150-151, 152, 154-155, 156-158, 161, 162-163, 164-165, 167, 168, 169-170, 171, 172-173, 175, 177, 178, 179, 180-181 (superseded), and 180-182. Use the source document IDs and stated offsets or exact markers to reproduce them on the original sample pack. Local review manifests and Parquet derivatives are not part of this submission. The earlier `first150_cleaning_manifest.json` and build script do not include these new transformations. Gold-set harm tests remain pending.


## Rows 221–230 review update

Nine additional full-read records (221,222,224–230) have no supported disease from DISEASES.md; labels now contain 194 unique documents. Original-page availability differs; full details remain in the local audit. Earlier scope statements describe earlier review stages; rows 201–220 have local audit entries, and this update does not infer labels for them. Row 223 has an editorial repair but remains pending taxonomy classification. The remaining pack is not asserted reviewed.

### Row 223: bounded editorial repair, classification pending

- Document: `acdee45b2b213fc4`; one document. Remove only three unsupported explanatory clauses. Retain the named geometric connections and the rest of the supplied text. This is not a D4 diagnosis: machine translation is not established.
- Original Arizona event could not be retrieved. Matching primary seminar abstract: https://www.math.utah.edu/Stochastics/Seminar/2021-02-10-sam-gralla/ (indexed abstract available).
- Runnable rule, guarded by document ID and exact source hash:

```python
import hashlib

def repair_row223(doc_id, text):
    if doc_id != "acdee45b2b213fc4" or hashlib.sha256(text.encode("utf-8")).hexdigest() != 'acdee45b2b213fc4acca4e2a7042a7fcbcf7190cd55a410ab3d0f67a4b229ace':
        return text
    for clause in [', which relates the area of a surface to its curvature', ', a curve that is the locus of points equidistant from a circle', ', a curve that is the intersection of two hyperbolas']:
        text = text.replace(clause, "", 1)
    return text
```

- Applied result recorded verbatim with before/after hashes in the local review audit and current comparison. Original pack preserved.
- Validation: exactly three unique clauses removed; other nine texts unchanged; ID/hash guard preserves unrelated records. Idempotent on repaired text.
- Harm test: pending Data A / Data B; no gold set supplied. Guard checks are not gold-set results.
- Side effects: deletion sacrifices explanatory detail; this rule must not generalize to other documents. No disease code is invented for an editorial factual error.


## Rows 114–115: C1 boilerplate correction

- Disease: C1; two source documents, `18e539a751c5a4b3` and `593cf027a182e228`. Both labels are fix/C1. This corrects the earlier empty-code join notes. No C3/C4 code is assigned for the join.
- Row 114: remove only the repeated title wrapper beginning `Content of the article:` at original offset 119, ending at 264 (exclusive). Preserve the author disclaimer.
- Row 115: remove the related-article preview/footer from `Learn moreJust finished` at original offset 1770 to 2533 (exclusive), before the Q&A.
- Source URL: https://gamestogather.org/assassins-creed/why-an-ac1-remake-could-to-soft-reboot-the-franchise-while-tying-nicely-with-ac-valhallas-modern-day-plot-and-ending/
- Source status: original webpage unavailable during verification; boilerplate classification is supported by the complete supplied text. No source retrieval success is claimed.
- Action: apply the exact source hash guard below before the previously approved 114+115 body join. Preserve each document's Q&A separately; do not join Q&A into the article body.

```python
import hashlib

def repair_114_115(doc_id, text):
    rules = {
        '18e539a751c5a4b3': ('18e539a751c5a4b3e1021f83c99d890f84f8447fac1e1ff2107d8916c267137d', 119, 264),
        '593cf027a182e228': ('593cf027a182e228980e20083e67c2e54640a8fd6375b28677007a5ae56b3dc3', 1770, 2533),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    return text[:rule[1]] + text[rule[2]:]
```

- Validation: exact spans removed, all Q&A characters preserved, disclaimer retained, repaired records unchanged on a second pass. Other source documents are protected by ID/hash guards.
- Remaining issue: row 115's final two Q&A concern AC Valhalla city targets described in the removed related-article preview, so they are not supported by the retained main article. They remain unchanged and flagged for separate Q&A review, not certified correct.
- Harm test: pending Data A / Data B; no gold set available. Local preservation checks are not a gold-test pass.
- Side effects: removing preview material removes the context for those two Q&A. Apply only to the exact reviewed records; no generic disclaimer or fan-comment deletion.


## Rows 116–117: review correction and bounded newline repair

This section supersedes the earlier empty-code fix entries. Row 116 (`affc40538cb81c3e`) has one editorial newline repair, with taxonomy classification pending; its invalid fix-without-code entry is removed from labels.csv and preserved here and in the local audit. Row 117 (`f7cc74aa2a292910`) is full/clean under the listed taxonomy, with no text changes. Labels now contain 193 unique documents. Pending row 116 has not been discarded from the review.

- Original URL: http://educatingourworld.com/index.php/coming-books/humanity-sin-salvation ; both HTTP and HTTPS attempts unavailable. No successful source verification claimed.
- Row 116: restore one newline at original character offset 792, between question 6 ending `Of?` and `Question 7`. Preserve the author introduction, all numbered questions, and all generated Q&A verbatim.
- Rows 116 and 117 continue the same numbered contents list at questions 9/10. They do not cut a sentence at this boundary; no C3 diagnosis is justified merely by the approved body join. No C4, C5, B5 or other unrelated code is forced onto a legitimate contents list or a missing newline.
- Previously approved body join retained as an optional local reconstruction: concatenate only the two article bodies before their generated Q&A with a newline; retain each source ID and each Q&A block separately.
- Q&A limitation: the contents list asks questions without providing answers. It does not substantiate row 116's humility recommendation or exclusive dichotomy answer, or row 117's definition of special providence. These remain unchanged and flagged for answer verification, not certified source-supported.

```python
import hashlib

def repair_row116(doc_id, text):
    if doc_id != 'affc40538cb81c3e' or hashlib.sha256(text.encode("utf-8")).hexdigest() != 'affc40538cb81c3e716ad73a1e6c4454e8844d4882d0bc82a180d9410d57b93a':
        return text
    return text.replace("Of?Question 7", "Of?\nQuestion 7", 1)
```

- Validation: one newline inserted; removing that newline reconstructs the exact original. Q&A unchanged; hash guard makes repeat application a no-op and preserves other documents.
- Harm tests: pending Data A / Data B; no gold set supplied.
- Side effects: inserts an inferred list boundary, not source-restored missing prose. Restrict to the exact document/hash; do not rewrite theological claims or treat religious content as harmful.


## Rows 119–120: final classification and C1 subscription-footer repair

Supersedes the earlier empty-code fix labels. Row 119 (`0ca7bce7da41acbb`) is full/clean: no listed disease found after full read. Row 120 (`76b4036d49000599`) is fix/C1 for email-subscription boilerplate. Same-article continuation and the previously approved body join do not themselves establish C3 or C4.

- Source: https://livinghopeomaha.com/visitors/why-do-you-practice-close-or-closed-communion/ ; original webpage unavailable during this review. Decisions are based on the full supplied records, not claimed source retrieval.
- Action: row 119 unchanged. In row 120 remove only the footer beginning `Follow Living Hope via Email` at original offset 2182, ending at 2304 exclusive, before the generated Q&A. Preserve article, visitor guidance, hymn, public attribution and all Q&A. This footer contains no actual email address; F1 is not assigned.
- Approved body join119+120 remains separate from diagnosis; concatenate article bodies only and retain Q&A separately under their source IDs.

```python
import hashlib

def repair_row120(doc_id, text):
    if doc_id != '76b4036d49000599' or hashlib.sha256(text.encode("utf-8")).hexdigest() != '76b4036d49000599952ad548de4cdee33afdeb27b5df3e12b44067c012a1b9be':
        return text
    return text[:2182] + text[2304:]
```

- Validation: exact footer removed, Q&A unchanged, all characters outside the deletion retained, unrelated document unchanged, repeat application a no-op.
- Harm test: pending Data A / Data B; gold set unavailable, no gold pass claimed.
- Side effects: loses subscription instructions intentionally; restrict deletion to exact ID/source hash and do not remove legitimate religious content or the hymn.


## Row 101: separate record; remove dangling section heading (C3)

This supersedes the earlier approved 101+102 join. User explicitly requested row101 remain separate. Do not concatenate row102 into row101. Row102 remains unchanged by this update; its proposed F3 drop has not been applied.

- Document: `a7e77ee57791124b`; label fix/C3. Section-level truncation: section4 heading at original offset 2723 has no body before generated Q&A; its body starts in source row102. This is not a mid-sentence cutoff. C3 is applied to the broader truncated-document condition, extending beyond the mid-sentence/code-block examples in DISEASES.md; reviewer confirmation of this interpretation remains appropriate.
- Action: remove only the dangling heading at offsets 2723:2772 (exclusive). Preserve prior completed paragraphs and all Q&A verbatim. No missing prose invented, no violent continuation imported.
- Source: https://arikiri.medium.com/5-things-i-have-learned-from-bullies-d4bd334c368b ; original webpage unavailable. Boundary is established by the supplied adjacent records, not retrieved webpage content.

```python
import hashlib

def repair_row101(doc_id, text):
    if doc_id != 'a7e77ee57791124b' or hashlib.sha256(text.encode("utf-8")).hexdigest() != 'a7e77ee57791124b4a6e8b6642ea9fc402ebf607e7e1e74c949231913916647e':
        return text
    return text[:2723] + text[2772:]
```

- Validation: exact heading removed; retained characters and Q&A unchanged; guard is idempotent. Original source pack preserved.
- Harm test: pending Data A / Data B; no gold set supplied.
- Side effects: deletion loses a section title rather than restoring its missing body. Do not generalize this rule to legitimate standalone headings. This is not a safety certification of the retained autobiographical discussion.


## Row 102: DONE — drop/F3

Supersedes the earlier empty-code fix label, proposed drop awaiting application, and any earlier101+102 join. Document `ef65e5b6d0cda5a5` is now drop/F3. Keep101 separate.

- Evidence: `even if it kills them, take the risk` at original character offset 314. The complete supplied text encourages potentially lethal violence and includes generated Q&A reinforcing violent advice. This is distinct from neutral reporting or criticism.
- Action: exclude the entire record, including all generated Q&A, from downstream cleaned output. Do not sanitize a sentence and retain the rest, or join this record into101. Original source pack remains preserved.
- Source URL: https://arikiri.medium.com/5-things-i-have-learned-from-bullies-d4bd334c368b ; original webpage unavailable during review. Diagnosis uses the full supplied record, not a claimed successful source retrieval.
- Runnable document-specific drop predicate (True means drop):

```python
import hashlib

def should_drop_row102(doc_id, text):
    return (doc_id == 'ef65e5b6d0cda5a5'
            and hashlib.sha256(text.encode("utf-8")).hexdigest() == 'ef65e5b6d0cda5a55c4562ab524991044b2ffda7fbe62057e6bda9f5364ebdab')
```

- Validation: exact source ID/hash matches; unrelated ID and changed-text inputs do not match. This is a label and exclusion decision; no full-pack cleaned export is generated by this update.
- Harm test: pending Data A / Data B; no gold set supplied.
- Side effects: drops non-harmful closing material and associated Q&A along with the harmful passage. Restrict to this exact record; do not generalize to all bullying narratives or self-defense discussion.


## Row 161 — reviewer-approved clean classification

Document `06a617ce8f69b17c`. User explicitly approved marking this record clean. labels.csv is now full/clean with empty disease, evidence and hex fields. This supersedes its previous fix-without-code classification. The previously documented source-verified opening-quotation repair remains in the audit and is not reverted. Clean reflects the reviewer decision under the supplied disease taxonomy, which has no exact missing-punctuation category; this update does not claim a new independent full-text/source verification or gold-test pass.


## Row 164 — DONE: reviewer-approved fix/C3

Supersedes the earlier empty-code fix classification. Document `5f01dd1e1e757f40` is fix/C3 for section-level truncation: `The Comrades` at original offset 2434 has no body before the generated Q&A, and its body starts in row165. This is not a mid-sentence cutoff; the broader truncated-document interpretation was explicitly approved by the reviewer, consistently with row101.

- Verified primary source: https://www.heritage-history.com/index.php?author=upton&book=nibelungs&c=read&story=comrades ; retrieved and confirms heading followed by the opening of row165.
- Action: retain the previously applied body join164+165, preserve all original wording and both Q&A blocks separately under their source IDs. No additional text replacement in this update. Original pack retained.
- Row165's source-verified internal omission remains unresolved. Marking row164 classification done does not certify the joined chapter complete or change row165's label.

```python
import hashlib

def join_164_165(id164, text164, id165, text165):
    expected = ('5f01dd1e1e757f40', '5f01dd1e1e757f40d9bb1ad295dd592346c6e42136f87909ed19e8db8e5cb450',
                'f0d2f0cf3e08335b', 'f0d2f0cf3e08335bfb7d2568f19af3e3f9ceaa0d00e776606461c368779d48d6')
    actual = (id164, hashlib.sha256(text164.encode("utf-8")).hexdigest(),
              id165, hashlib.sha256(text165.encode("utf-8")).hexdigest())
    if actual != expected:
        raise ValueError("Unexpected source records; do not join")
    body164, sep164, qa164 = text164.partition("\n\nQuestion:")
    body165, sep165, qa165 = text165.partition("\n\nQuestion:")
    if not sep164 or not sep165:
        raise ValueError("Expected separate Q&A blocks")
    return body164 + "\n" + body165, (sep164 + qa164, sep165 + qa165)
```

- Validation: guarded reconstruction retains all characters; original records reconstruct exactly using their bodies and separate Q&A blocks. One separator newline added between bodies.
- Harm test: pending Data A / Data B; no gold set supplied.
- Side effects: join changes document boundaries and Q&A context; preserve source IDs and do not generalize to unrelated chapters or other records.


## Row 166 — editorial corrections applied; disease classification pending

Supersedes earlier preserve-unchanged instructions, as user now requested repair. Document `3e3f61cd431b6790`. Five exact corrections verified against Project Gutenberg, Chapter XXIII: https://www.gutenberg.org/files/73227/73227-h/73227-h.htm . Publisher transcription retains the erroneous readings; this is an explicit parallel-edition editorial correction, not an assertion that the publisher agrees.

Original-offset edits: [(400, 'beloved. spouse', 'beloved spouse'), (2083, 'frame cloth appear', 'frame doth appear'), (2233, 'Balmung once loth strike', 'Balmung once doth strike'), (2477, '"Truly "said Volker', '"Truly," said Volker'), (1602, '"\' T were but certain death', '"\'Twere but certain death')]. Preserve all other characters, archaic wording, and generated Q&A. Keep row166 separate.

The invalid fix-with-empty-code CSV entry is removed and retained here pending taxonomy classification, consistently with row116. The supplied taxonomy has no ordinary OCR/transcription category: A3 requires encoding mojibake, D4 machine translation, and B5 broken Markdown/LaTeX; none is established here. No fabricated code or clean claim. Labels now contain 192 records; row166 remains reviewed and repaired in this audit.

```python
import hashlib

def repair_row166(doc_id, text):
    if doc_id != '3e3f61cd431b6790' or hashlib.sha256(text.encode("utf-8")).hexdigest() != '3e3f61cd431b679027fff3422d34deeeb7596ca3fe37c0624e15bf74ec2f92db':
        return text
    for old, new in [('beloved. spouse', 'beloved spouse'), ('frame cloth appear', 'frame doth appear'), ('Balmung once loth strike', 'Balmung once doth strike'), ('"Truly "said Volker', '"Truly," said Volker'), ('"\' T were but certain death', '"\'Twere but certain death')]:
        text = text.replace(old, new, 1)
    return text
```

Validation: five unique replacements; Q&A unchanged; guard is idempotent. Original source pack preserved. Gold harm tests pending Data A / Data B. Side effects: cross-edition editing may override source-inherited wording; exact ID/hash only. Editorial repairs complete; final classification remains pending.


## Row 166 — DONE: reviewer-approved clean classification

Document `3e3f61cd431b6790`. User explicitly requested final resolution (done or drop); retain the repaired record and mark done. Supersedes the preceding pending-classification status and CSV exclusion. Row166 is restored to labels.csv as full/clean with empty diseases, evidence and hex. Clean denotes no supported disease under the supplied taxonomy after full review; five source-verified editorial transcription corrections remain applied in the local audit and runnable rule. This does not assert that the original publisher text was free of transcription errors. No unsupported drop code, no original-source deletion, no reversal of the corrections. Labels now contain 193 unique records. Gold harm tests remain pending.


## Row 167 — DONE: clean classification, opening-quote repair retained

Document `3ef34026380566db`. User requested resolution. Following the already approved row161 missing-quotation treatment, labels.csv is full/clean with empty disease, evidence and hex. This supersedes the earlier fix-without-code label. The existing source-verified opening quotation restoration before `Give place!` remains applied in the local repair record; no further text substitution or source change is made. The supplied disease taxonomy has no exact missing-prose-punctuation category; no unrelated code is assigned. Clean reflects the listed-taxonomy classification and does not deny the documented original punctuation defect. Source verification and inverse-restoration details remain in the earlier row167 medicine/audit. Gold harm tests remain pending; no new gold test or independent source fetch claimed.


## Row 168 — DONE: repaired text and clean classification

Document `0074a878bb00d533`. User requested fixing and marking done. Supersedes the earlier fix-with-empty-code classification. Following reviewer-approved handling of rows161 and167, labels.csv is full/clean with empty disease/evidence/hex fields under the supplied taxonomy. The original editorial defects remain documented; clean is not an assertion that the original was identical to the corrected text.

Existing source-verified repairs revalidated against the exact source record: `THE MINSTRELAND HAGEN.` becomes `THE MINSTREL AND HAGEN.`; insert the opening straight quotation mark before `An thou lov'st me, do not so!`. Keep row168 separate and all Q&A unchanged. No fabricated disease code for ordinary caption spacing or prose punctuation. Prior edition references remain in the existing row168 medicine; no new source fetch is claimed.

Validation: exactly two inserted characters, zero deleted characters; reversing both substitutions reconstructs the exact original; Q&A unchanged. Original SHA-256: `0074a878bb00d53396494c2836a7e4eab9372e5f7a8f1fc410192152cfc27d9c`. Applied-text SHA-256: `0e7b9351c858e5012f91f6e2a5c928b99880912d30a773e89b918e6832f546d1`. Gold harm tests remain pending Data A / Data B. This update resolves the label mismatch, not the pending gold tests.


## Rows 231–240: source-checked batch update

Labels now contain 201 unique documents. Added231,232,233,234,237,239 as full/clean under listed taxonomy;238 and240 as fix/C1. Full source availability differs and does not constitute independent factual verification of every claim. Row235 bibliography remains pending; row236 has one applied editorial repair and unresolved summary-fidelity concerns, so neither is given an invented disease code or a clean label. Full source URLs, status, original/applied text and hashes recorded in local audit.

### C1: closing promotional invitations in rows238 and240

Two bounded examples: `9928e7e0ef8639ed` (virtual-tour CTA, original offset 2177) and `922e32f45f63d013` (sales-contact CTA, original offset 2044). Remove only final CTA paragraph and its separating blank line. Preserve all preceding prose, historical dates and professional quotations. The original238 announcement was retrieved and confirms the tour link; original240 unavailable, so its C1 diagnosis rests on explicit sales language in the supplied record. A company contact is not automatically private-person F1.

### Row236: document-specific editorial repair, taxonomy pending

Document `86f8b6294afb3c76`. Original collection retrieved: https://upjoke.com/f1-jokes . Replace one mistaken keyboard-joke explanation; preserve quoted joke and all other wording. Other questionable material, notably a rumor described as fact and abbreviated long jokes, remains unresolved. No generic factual-error code exists in the supplied list; no translation origin established. Row235 original assignment unavailable and bibliography not verified: no speculative citation substitution applied.

```python
import hashlib

def repair_231_240(doc_id, text):
    rules = {
        '86f8b6294afb3c76': ('86f8b6294afb3c769880c999402091a98aae825708ecae60e77e9634d4910b7e', ('replace', 'This joke exaggerates the passion of F1 fans.', 'The joke plays on the F1 keyboard key commonly used to open help.')),
        '9928e7e0ef8639ed': ('9928e7e0ef8639ed53b3698684611a92936e0db059419f088902fd28c083b9ba', ('truncate', 2175)),
        '922e32f45f63d013': ('922e32f45f63d0133206d065b4d8bd735dbbccde973ce44a2a6de0393a9f39a0', ('truncate', 2042)),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    operation = rule[1]
    if operation[0] == "replace":
        return text.replace(operation[1], operation[2], 1)
    return text[:operation[1]]
```

Validation: rule changes exactly three source records (236,238,240), is idempotent on repaired text, leaves all other source records unchanged, and uses exact document IDs and source hashes. Six newly clean texts unchanged. CSV header, unique IDs and verdict/code agreement checked.

Harm test: pending Data A / Data B; no gold set supplied. These local checks do not establish gold harm rates. Side effects: CTA deletion removes promotional navigation; editorial explanation changes source wording; restrict to exact reviewed documents. Preserve historical2025 migration dates as historical claims, not verified current guidance. No whole-pack cleaned export created; repairs stored in local audit and runnable rule.


## Rows 231–240 — approved completion status

User approved rows238 and240 as DONE/fix/C1 following the applied closing-CTA removal. Clean rows231,232,233,234,237,239 are also marked DONE. Their CSV notes now explicitly begin DONE; verdicts, codes, evidence and text repairs are unchanged. Source-availability limitations in the prior review remain valid. Rows235 and236 remain pending; no clean or done status is inferred for them. Gold harm tests remain pending; completion refers to this local row review, not gold-test success or remote submission.


## Row 235 — DONE: bibliography editorial repair; clean under listed taxonomy

Document `1b8fba9b89baf612`. User requested repair and completion where applicable. Supersedes row235's pending bibliography classification. Read full text; no supported disease from DISEASES.md. Added full/clean label following existing reviewer-approved treatment of editorial-only errors. This is not an assertion that the original bibliography was error-free; no generic factual-error code is invented. Labels now contain 202 records. Row236 remains pending.

Applied: remove four unverified bibliography entries and their associated in-text attributions; retain the educational proposal as a proposal. Keep two corroborated readings. Replace the incorrectly specified AAP2015 citation with the verified2019 fourth edition and its organizational coauthors/DOI; describe consulting it, rather than asserting each proposed protocol already complies. Narrow the nature-book attribution to its supported topic, and remove the blanket claim that all recommendations are research-supported. No claim that the four unverified works do not exist; search failure alone does not establish fabrication.

Original assignment URL unavailable. This is an explicit editorial revision using independently verified readings, not a restoration of the inaccessible original assignment or verification of all room-design recommendations. The journal publisher blocked direct access to the1967 article; its metadata was checked in the national scholarly bibliographic record. The nature-book author corroborates title/topic and2005 publication. AAP publisher confirms edition, authorship, title, year and DOI. Sources:

- https://publications.aap.org/aapbooks/book/530/Caring-for-Our-Children-National-Health-and-Safet
- https://publications.aap.org/books/chapter-pdf/1376040/aap_9781610022989-all_front_matter.pdf
- https://cir.nii.ac.jp/crid/1363107370804288512
- https://richardlouv.com/books/last-child/
- https://richardlouv.com/blog/newcoverdesignlastchildinthewoods

Runnable rule, exact ID/hash only:

```python
import hashlib

def repair_row235(doc_id, text):
    if doc_id != '1b8fba9b89baf612' or hashlib.sha256(text.encode("utf-8")).hexdigest() != '1b8fba9b89baf6124a95856b3d667e928e6056a7c59dde07c14e545d6efb5b93':
        return text
    replacements = [('that are safe, engaging, and research-based. The manual will address the following key areas, supported by relevant research and citations.', 'that are safe and engaging. The manual will address the following key areas, with relevant readings listed at the end.'), (' (Goldstein, 1999)', ''), ("These protocols align with the American Academy of Pediatrics' (AAP) recommendations for child care health and safety (AAP, 2015).", 'The manual will consult Caring for Our Children for health and safety standards in early care and education settings (American Academy of Pediatrics et al., 2019).'), (' (Wells, 2000)', ''), ("These design principles are based on research on the impact of the physical environment on young children's development (Louv, 2005).", "Children's access to nature is discussed in Last Child in the Woods (Louv, 2005)."), ("- The role of the environment in supporting children's development (Brewer, 1995)", "- The role of the environment in supporting children's development"), ('- The need for flexible, adaptable learning spaces (Dahlberg & Lenz Taguchi, 1994)', '- The need for flexible, adaptable learning spaces'), ('- American Academy of Pediatrics. (2015). Caring for Our Children: Health and Safety Guidelines for Early Education and Child Care. Retrieved from <https://www.aap.org/en-us/about-the-aap/Committees-Councils-Sections/Council-on-Families-Children-and-Youth/Pages/Caring-for-Our-Children.aspx>\n- Brewer, D. (1995). The environment as the third teacher. Exchange, 24(4), 1-5.\n- Dahlberg, G., & Lenz Taguchi, H. (1994). The image of the child: Studies of the impact of the image of the child on early childhood education. Journal of Education Policy, 9(1), 5-23.\n- Goldstein, H. (1999). The power of play: Learning whatsoever anyone needs to know. Schocken Books.\n- Louv, R. (2005). Last child in the woods: Saving our children from nature-deficit disorder. Algonquin Books.\n- Vygotsky, L. S. (1967). Play and its role in the mental development of the child. Soviet Psychology, 5(3), 6-18.\n- Wells, N. (2000). The meaning of nature: Cities and culture in the American landscape. University of Virginia Press.', '- American Academy of Pediatrics, American Public Health Association, & National Resource Center for Health and Safety in Child Care and Early Education. (2019). Caring for Our Children: National Health and Safety Performance Standards; Guidelines for Early Care and Education Programs (4th ed.). American Academy of Pediatrics. https://doi.org/10.1542/9781610022989\n- Louv, R. (2005). Last child in the woods: Saving our children from nature-deficit disorder. Algonquin Books.\n- Vygotsky, L. S. (1967). Play and its role in the mental development of the child. Soviet Psychology, 5(3), 6-18.')]
    for old, new in replacements:
        if text.count(old) != 1:
            raise ValueError("Unexpected citation context")
        text = text.replace(old, new, 1)
    return text
```

Validation: eight exact replacements, three remaining bibliography entries, removed attributions absent, other record IDs unchanged, repeat application a no-op. Original source preserved. Before SHA-256: `1b8fba9b89baf6124a95856b3d667e928e6056a7c59dde07c14e545d6efb5b93`; after SHA-256: `834c79db4a4313c1a27adc234a97069a174b1030abc0c8464be7afea89a751bc`.

Harm test: pending Data A / Data B; gold set unavailable. Side effects: removes potentially legitimate but unverified references and introduces a clearly documented newer verified AAP edition; not suitable for generic bibliography cleanup. Preserve the original for reversal and attribution review. No whole-pack cleaned export generated.


## Row 236 — DONE: source-checked editorial repair; clean under listed taxonomy

Document `86f8b6294afb3c76`. User requested repair and completion. Supersedes row236's pending classification and earlier single-sentence keyboard-only repair: apply this final rule to the original record. Added full/clean under the supplied taxonomy; original semantic errors remain documented. No generic factual-error code is invented. Labels now contain 203 unique records. All ten source rows231–240 have completed local review status; gold tests remain pending.

Primary source retrieved: https://upjoke.com/f1-jokes . It contains eight complete retained jokes and truncated previews of longer jokes. The transfer-rumor paragraph was not corroborated by the retrieved source; absence is not proof that no such joke ever existed.

Applied repairs:
- Broaden heading/introduction to include keyboard and chess wordplay, not exclusively racing trivia.
- Correct explanations of the keyboard help key, thrust/trust pun, chess-square name, speed slang and name/sound joke. Avoid portraying fictional jokes as evidence about real racing hazards or driver lifestyles.
- Remove the unverified transfer-rumor paragraph.
- Remove the incomplete afterlife-joke summary and the trailing incomplete founder-afterlife example; do not invent missing punchlines.
- Preserve eight complete quoted jokes exactly as in the supplied record. No Q&A block is present.

```python
import hashlib

def repair_row236(doc_id, text):
    if doc_id != '86f8b6294afb3c76' or hashlib.sha256(text.encode("utf-8")).hexdigest() != '86f8b6294afb3c769880c999402091a98aae825708ecae60e77e9634d4910b7e':
        return text
    paragraphs = ['F1 Wordplay and Racing Humor', 'The source collection includes jokes about Formula One as well as other meanings of F1. One joke plays on Harry Potter: "Why was Harry Potter surprised when Ferrari won the F1 race? Because he expecto\'d Petronas." It combines the sound of the spell \'expecto patronum\' with the name Petronas.', 'Another joke uses the two meanings of a circuit: "Why did the F1 racer get electrocuted after finishing 1 lap? Because he completed the circuit." The pun links a racing circuit with an electrical circuit.', None, 'Another joke plays on the slang meaning of speed: "An F1 racer gets addicted to amphetamines. He soon felt the need for speed." Speed refers both to fast movement and to amphetamines.', 'A fictional racing-accident joke uses the word shattered: "A F1 race driver got into an accident. He broke almost every single bone in his body. The doctors told him he would never drive again. He was absolutely shattered." The punchline combines physical injury with emotional devastation.', 'A keyboard joke reads: "I\'m obsessed with my F1 key. I think I need help." The joke plays on the F1 keyboard key commonly used to open help.', 'A crew-relationship joke asks: "Why doesn\'t the F1 driver get along with his crew? He has thrust issues." The pun substitutes thrust for trust.', 'A chess joke asks: "Why is the white bishop piece in chess the fastest? Because it\'s on F1." Here F1 is the name of a chessboard square, creating wordplay with Formula One.', None, 'A name-based joke asks: "Who was that Austrian F1 driver, Niki... Lauda?" The punchline plays on the sound of Lauda and louder.']
    return "\n\n".join(p for p in paragraphs if p is not None)
```

Validation: exact ID/hash guard, eight quoted jokes preserved verbatim, unrelated ID unchanged, repeat application a no-op. Original source pack preserved. Original SHA-256: `86f8b6294afb3c769880c999402091a98aae825708ecae60e77e9634d4910b7e`; final applied-text SHA-256: `47961f200492d1743474ce107ad41da5aa9210008f034696f71fece05df4a4ab`. This is an editorial cleanup of summary fidelity, not an F3 diagnosis or a C3 assertion about the complete original document.

Harm test: pending Data A / Data B; no gold set supplied. Side effects: drops some summary content and rewrites explanatory framing; exact document only, original retained for reversal. No whole-pack cleaned export created.


## Rows241–250 — reviewed batch update

Added nine DONE/full/clean labels:241–247,249,250. No listed disease code is supported after full-text review; factual/editorial repairs are recorded separately, consistently with prior reviewer-approved treatment. This does not deny original semantic errors or certify all claims. Row248 is not marked clean/done: three supported editorial repairs applied, but detailed exercise protocols and progression to surgery remain unverified. Its full reviewed source and revised text remain in local audit; no invalid empty-code fix row is added. Total labels: 212 unique documents.

Repairs:241 co-founder attribution;244 remove unsupported profitable-system and maximum-bet advantage framing while retaining topic structure;245 correct lifelong fluoride-immunity overclaim;247 correct project/team distinction and recruitment framing, remove unsupported duration/results implication;248 three bounded medical wording corrections with remainder pending;250 remove inconsistent colony chronology clause and restore missing verb. Rows242,243,246,249 unchanged. Original pages often unavailable; primary-source corroboration and limits recorded per row in audit/current comparison.

Runnable bounded rule (six exact source records changed; exact ID/hash only):

```python
import hashlib

def repair_241_250(doc_id, text):
    rules = {
        '86d81c2c18b6ce22': ('86d81c2c18b6ce2211cf189a5bec8eea912b616c88a5d6a2ab3558a1f7cca621', [('Kevin Kelly, the founder of WIRED magazine', 'Kevin Kelly, a co-founder of WIRED magazine')]),
        '56f7b0fec5a5a5f7': ('56f7b0fec5a5a5f74bcfde8dc1a974388469e40540b597ee8c73e9f183aae870', [('Blackjack: Maximizing Wins with Matrices\nIn blackjack, using a suitable matrix can provide an advantage over the house. This is particularly effective when playing at tables with fewer decks, as the matrix can help predict favorable outcomes. By printing out and using the matrix during gameplay, players can strategically place bets and increase their chances of winning cash.', 'Blackjack: Strategy and the House Edge\nA strategy chart can guide blackjack decisions under particular rules, but it does not guarantee profit or predict the next cards.'), ('Roulette: Effective Betting Strategies\nA well-designed roulette betting strategy can significantly enhance the likelihood of winning money prizes while maintaining enjoyment. These strategies often focus on covering more numbers while keeping bets profitable. For instance, the Martingale system involves doubling bets after each loss, which can help recoup losses and potentially yield a profit.', "Roulette: Betting Systems\nThe Martingale system doubles a bet after a loss. Betting patterns do not change a fair roulette game's outcome probabilities or remove the house edge; increasing stakes increases the money at risk."), ("Bonus Money: Responsible Gaming\nWhen playing with bonus money, it's essential to use it wisely. Instead of squandering it on unsuitable games or risky bets, players should strategically use these funds to win real money prizes and extend their gameplay. However, it's crucial to understand and adhere to any wagering requirements to keep winnings and remain profitable.", 'Bonus Money and Wagering Requirements\nBonus offers may have wagering requirements and other restrictions. They do not guarantee profitable play.'), ('Poker: A Complex Game Worth Mastering\nPoker is a game of skill and strategy, with the potential for significant real money prizes. Instead of wasting time and money on less profitable games, players should focus on honing their poker skills to maximize their winnings.', 'Poker: Skill and Uncertainty\nPoker involves decisions and competition with other players. Skill does not guarantee winnings.'), ('Managing Bankroll: Free Money and Maximum Bets\nFree bankroll from bonus presents can be used strategically to win more money prizes and extend gameplay. By placing the maximum allowable bet, players increase their chances of hitting it big and making the most of their free money.', 'Managing Bankroll and Bet Size\nLarger bets increase financial exposure. Setting limits does not change the underlying odds or guarantee profit.')]),
        '2ce289d44022c56c': ('2ce289d44022c56ceeffeb8a2337adefb33c322015d43e20c043895f36662983', [('Systemic Fluoride Benefits', 'Fluoride During Tooth Development and After Eruption'), ("An interesting aspect of fluoride's benefits is its ability to integrate into teeth during their formation. If fluoride is present in drinking water during childhood, it can become incorporated into the entire tooth structure, creating a lifelong resilience to acid damage. This is why fluoride is often added to drinking water in small amounts, a practice known as water fluoridation, to help protect teeth and prevent tooth decay.", 'Fluoride can become incorporated into developing teeth. Continued fluoride exposure after teeth erupt also helps protect enamel and prevent tooth decay. Childhood exposure does not make teeth permanently immune to acid damage.')]),
        '4853919e6ab2f0fb': ('4853919e6ab2f0fb3363d5f615b237667e82b4c3989704463a483917b7b8f253', [('making them capable of detecting subtle changes that could indicate the onset of certain illnesses.', 'which researchers are investigating for possible early signs of respiratory illness.'), ('The Seattle Flu Study, as it is known,', 'The research involves the Seattle Flu Study team and'), ("The study is focused on the Seattle area due to its residents' potentially higher risk of respiratory diseases, given their frequent exposure to others through work or activities, health conditions, or other factors. With the pandemic still ongoing, the study's results are eagerly anticipated and are expected to take about six months.", 'The study focused on recruiting participants in the Seattle area, including groups at higher risk of infection such as frontline workers and people in group living settings. Its aim was to investigate possible early detection, not to establish that all Seattle residents were at higher risk or that the device was already a proven diagnostic tool.')]),
        'dea9d17e5299b611': ('dea9d17e5299b61197bb34478694337196fa61484bdde7d0d6518c9b8ea3fb36', [('including heart and lung diseases, joint problems, and balance difficulties', 'including neck, shoulder, or back pain, reduced joint movement, balance difficulties, and difficulty breathing'), ('Regular stretching can help build and maintain these muscles.', 'Regular stretching can help maintain flexibility.'), ('These exercises can help realign your spine and improve your overall posture.', "Good posture should maintain the spine's natural curves.")]),
        '08b696f23e121a8b': ('08b696f23e121a8beb21b7446ca2463d2671f349fe5305f19d99f4c3dd774003', [('Constructed in 70 BCE, just prior to the founding of the colony,', 'Constructed in 70 BCE,'), ('inscriptions that still legible today', 'inscriptions that are still legible today')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed text")
        text = text.replace(old, new, 1)
    return text
```

Validation: exactly six source records changed by the rule; four unchanged; repaired inputs are idempotent; CSV header, uniqueness, single-line fields and verdict/code consistency pass. Original pack preserved. Full original/applied text and hashes recorded locally. No full-pack cleaned export created.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial changes alter source-derived meaning, and244 in particular removes promotional benefit assertions; these are transparent source-specific revisions, not original-page restorations. Never generalize to all gambling, medical or glossary documents. Source verification does not constitute individual medical/financial guidance.


## Row248 — DONE: completed editorial medical correction

Document `dea9d17e5299b611`. User requested fixing and marking done. Supersedes row248's pending classification and the previous partial three-replacement rule: the following final rule is applied to the exact original source record. Added full/clean under the supplied taxonomy, consistently with previously approved treatment of editorial-only errors. This does not describe the original medical assertions as correct; no generic medical-misinformation code is invented. Total labels: 213 unique records. All ten rows241–250 now have completed local row-review status.

Original article remains unavailable. Independently retrieved primary health source: https://medlineplus.gov/guidetogoodposture.html (NIH/National Library of Medicine). This is an explicit editorial revision of the supplied text, not a faithful reconstruction of the inaccessible article. Retain general posture awareness, movement, core support and sitting guidance that this primary source supports.

Remove the unsupported women-specific risk generalization, heart/lung disease causation, fixed exercise doses, balance-ball and prone-lift prescriptions, claims of anatomical spine realignment, and exercise-to-surgery progression. Remove any suggestion stretching alone builds muscle or that the guide is comprehensive. Preserve six-topic organization with general supported guidance. No Q&A block exists.

```python
import hashlib

def repair_row248(doc_id, text):
    if doc_id != 'dea9d17e5299b611' or hashlib.sha256(text.encode("utf-8")).hexdigest() != 'dea9d17e5299b61197bb34478694337196fa61484bdde7d0d6518c9b8ea3fb36':
        return text
    return "Posture Awareness and Everyday Habits\n\nPosture is how the body is held during movement and at rest. Slouching can contribute to neck, shoulder, or back pain, reduced flexibility, balance difficulties, and difficulty breathing.\n\n1. Body Awareness\n\nNotice your posture during everyday activities. When standing, keep your head level and let your arms rest naturally at your sides.\n\n2. Movement and Flexibility\n\nChange sitting positions, take brief walking breaks, and stretch gently to relieve muscle tension.\n\n3. Core Muscles\n\nRegular physical activity and exercises that strengthen the back, abdomen, and pelvis can help support posture.\n\n4. Persistent Discomfort\n\nConsult a healthcare professional about persistent neck or back discomfort.\n\n5. Supporting the Spine\n\nGood posture maintains the spine's natural curves. When sitting, support the lower back and keep the feet supported on the floor or a footrest.\n\n6. Practice\n\nImproving posture takes practice. Incorporate posture awareness into everyday routines."
```

Validation: exact document ID/source hash guard; unrelated ID unchanged; repeat application a no-op; named unsupported claims and fixed exercise doses absent from final text. Original SHA-256: `dea9d17e5299b61197bb34478694337196fa61484bdde7d0d6518c9b8ea3fb36`; final applied-text SHA-256: `49232b92e77098efd8b395754886cea03a7d8760989a7277724bda2896a2b97a`. Original pack preserved; full original and revised text in local audit/current comparison. CSV uniqueness and verdict/code agreement checked.

Harm test: pending Data A / Data B; no gold set supplied. Side effects: substantive source-specific revision shortens and generalizes the article, removing original women's-health detail. This is not a generic medical cleaning rule, individualized exercise prescription or gold-test pass. No whole-pack cleaned export created.


## Rows251–260 — DONE: taxonomy review and editorial corrections

Added ten full/clean labels under the existing reviewer-approved taxonomy treatment. No supported disease code found after full reads and mechanical checks. Editorial semantic errors are documented separately; do not assign A3, C3, C4, D4 or another unrelated code merely to populate a fix label. Original factual correctness is not implied. Labels now contain 223 unique documents.

Seven edited records:251 index/basket terminology;252 explicit July2017 historical framing;254 plaque/tartar and flossing technique;256 restore merged module-topic boundaries;257 remove unverified interview guarantee;259 separate Sputnik, SCORE and Moon-relay chronology, remove invented dish-event date;260 correct definitions, stress and childhood thresholds, remove unverified lemon-treatment claim and incorrect previous-normal comparison. Records253,255,258 retained unchanged. Source availability and residual verification limits recorded per row in local audit and comparison. No current-law, current-fee, accreditation or individualized medical certification claimed.

```python
import hashlib

def repair_251_260(doc_id, text):
    rules = {
        '2a78f43220caf1ae': ('2a78f43220caf1aefbc7983e0b727f500225e3af46b6f219792be5600166a909', [("Market Baskets: Measuring Market Performance\n\nA market basket, also referred to as a market index, is a crucial tool used to assess the performance of a market relative to its peers. For instance, the S&P 500 index or the Dow Jones Industrial Average are prominent examples of market baskets. These indices are designed to represent a section of the market, allowing investors to track its performance over time.\n\nMarket baskets are not confined to tracking market performance; they also measure a company's performance across various industries. This comprehensive approach helps investors make informed decisions about where to allocate their resources.\n\nThe data used to calculate market basket measures typically comes from diverse sources, including research firms like Morningstar. In the past, companies have also employed alternative metrics such as earnings, sales, and profits to gauge their performance.\n\nThe key factors determining a market's success are its capacity to absorb growth and deliver returns. However, a market can also flourish if the underlying industry factors are improving.\n\nMarket data can serve as a predictive tool, indicating whether a company is likely to be profitable. Nevertheless, a company's actual performance can also significantly influence its success.", 'Market Indexes and Baskets of Securities\n\nA market index measures the performance of a selected basket of securities intended to represent a market or sector. Examples include the S&P 500 and the Dow Jones Industrial Average.\n\nAn index is a measure calculated from its constituent securities, not another name for every kind of market basket. It can provide a benchmark for comparing investment performance. Its value does not by itself establish whether an individual company is profitable.\n\nIndex funds seek to track a market index. The index itself is not an investment that can be purchased directly.')]),
        '23e46f27cccca21e': ('23e46f27cccca21e58329ee2aa29f22e83524e79b05c23d117457f0a610ace30', [('In the realm of housing rights, a contentious issue is whether landlords should be permitted to discriminate against individuals with criminal records, particularly former prisoners. This debate is currently being explored by the Seattle City Council, which is contemplating a bill that could significantly alter the landscape of tenant selection processes.\n\nThe proposed legislation seeks to address the challenge faced by ex-felons in securing adequate housing. It aims to prohibit landlords from automatically disqualifying potential tenants based solely on their criminal history. Councilmember Lisa Herbold, a proponent of the bill, argues that such blanket exclusions amount to an "extrajudicial punishment," extending the sentence of the offender beyond what the courts have decreed. Herbold posits that once an individual has served their time and paid their debt to society, landlords should not be empowered to withhold housing as an additional form of penalty.\n\nThe council is also considering an even more restrictive measure: banning landlords from conducting criminal background checks altogether. This would significantly curtail landlords\' ability to assess the suitability of potential tenants, potentially shifting the balance of risk and responsibility in the landlord-tenant dynamic.\n\nOne unanswered question in this debate is whether the proposed restrictions would extend to individuals on the sex offender registry. This is a complex issue, as it involves balancing the rights of former offenders to secure housing with the safety and well-being of the community, particularly vulnerable populations such as children. As the Seattle City Council continues to deliberate, these nuances will likely be a key focus of their discussions.', 'Historical account: July 2017 Seattle housing-policy debate\n\nIn the realm of housing rights, a contentious issue is whether landlords should be permitted to discriminate against individuals with criminal records, particularly former prisoners. This debate is currently being explored by the Seattle City Council, which is contemplating a bill that could significantly alter the landscape of tenant selection processes.\n\nThe proposed legislation seeks to address the challenge faced by ex-felons in securing adequate housing. It aims to prohibit landlords from automatically disqualifying potential tenants based solely on their criminal history. Councilmember Lisa Herbold, a proponent of the bill, argues that such blanket exclusions amount to an "extrajudicial punishment," extending the sentence of the offender beyond what the courts have decreed. Herbold posits that once an individual has served their time and paid their debt to society, landlords should not be empowered to withhold housing as an additional form of penalty.\n\nThe council is also considering an even more restrictive measure: banning landlords from conducting criminal background checks altogether. This would significantly curtail landlords\' ability to assess the suitability of potential tenants, potentially shifting the balance of risk and responsibility in the landlord-tenant dynamic.\n\nOne unanswered question in this debate is whether the proposed restrictions would extend to individuals on the sex offender registry. This is a complex issue, as it involves balancing the rights of former offenders to secure housing with the safety and well-being of the community, particularly vulnerable populations such as children. As the Seattle City Council continues to deliberate, these nuances will likely be a key focus of their discussions.')]),
        '912f88dde57e1e99': ('912f88dde57e1e99798b49c33c489ccf07c9f20263eb25554fc1e5aa5a1c2281', [('Flossing is a crucial component of a daily dental hygiene routine, essential for keeping teeth strong and healthy. It cleans areas that toothbrushes cannot reach, helping to prevent plaque buildup and making teeth appear brighter.', 'Cleaning between teeth helps remove plaque and food particles from areas a toothbrush may miss. Flossing is one way to do this.'), ('Regular flossing prevents plaque formation, which can otherwise harden into tartar.', 'Regular cleaning removes plaque before it hardens into tartar.'), ('2. Enhances Brushing Effectiveness: Flossing before brushing removes sticky and hard substances between teeth, making brushing more effective and easier. It also complements the use of mouthwashes, enhancing overall oral hygiene.', '2. Complements Brushing: Flossing removes plaque and food particles between teeth. Hardened tartar requires professional cleaning; floss does not remove it.'), ('3. Cost-Effective: Regular flossing, taking only a minute or two each day, can prevent dental problems and reduce the need for costly dental treatments.', '3. Daily Cleaning: Cleaning between teeth regularly helps maintain oral hygiene.'), ('- Use a 15-18 inch piece of floss, sliding it between teeth in a C-shaped motion, and polish teeth in an upside-down motion.', '- Guide floss gently between the teeth, curve it around the side of each tooth, and move it up and down without forcing it against the gums.'), ('- If gums bleed, it indicates inflammation due to plaque buildup. Persistent bleeding may signal gum disease, requiring a dental visit.', '- Bleeding gums can be a sign of gum disease. A dental professional can assess persistent bleeding; the symptom alone does not establish its cause.'), ('- For those with limited hand movement, a floss holder can be used. There are two types: a wooden pick-like holder and a two-pronged plastic holder for single-handed use.', '- If holding floss is difficult, options include a floss holder, dental pick, interdental brush, or water flosser. These are distinct cleaning aids, not just two types of floss holder.')]),
        '7276bebe0e3db13a': ('7276bebe0e3db13af4bb339096cda8f732fbdc699c71d731aa86734e09df818e', [('Social Cognition and Interactions\n', 'Social Psychology\n'), ('Students will investigate various quantitative methods to examine psychological phenomena, employing core statistical techniques, data analysis software, and interpretation of results.', '\n\nQuantitative Research Methods\nStudents will investigate various quantitative methods to examine psychological phenomena, employing core statistical techniques, data analysis software, and interpretation of results.'), ('Individual Differences and Personality Biology', 'Personality and Individual Differences'), ('with a particular focus on biopsychological underpinnings', 'including the contributions of biological and environmental factors'), ("Psychology's Historicisation and Contemporary Issues", 'Psychology in Context'), ('It also explores contemporary social, cultural, political, and ethical issues in sport and physical education.', '\n\nContemporary Issues in Sport and Physical Education\nThis topic explores contemporary social, cultural, political, and ethical issues in sport and physical education.')]),
        '4670bc98d926df7e': ('4670bc98d926df7e1c1120a91ecbe3d59f5cf2b44c281a58b387c0b2e9b9ed76', [('By completing the P4T course, you not only gain valuable skills for your future career but also secure a guaranteed interview for one of our highly sought-after PGCE courses.', 'Preparation activities can help students develop skills for a future teaching application. Admission and interview arrangements depend on the relevant programme requirements.'), ('For undergraduate course applicants, the application process involves using the UCAS system with the relevant UCAS code.', 'In the source-era account, undergraduate course applicants use the UCAS system with the relevant UCAS code.')]),
        '55c6361188a8cd06': ('55c6361188a8cd06bba9d60efcaf49c6d19ebfcbdacaeac79f019330ad1c4cbf', [('On July 13, 2021, a dish will transmit data to both terrestrial communication hubs and orbital satellites around Earth. This network, known as a constellation, consists of 20 Medium Earth Orbit (MEO) satellites, with the first four launched in 2013.', 'Ground terminals exchange data through orbital satellites. By 2019, the O3b constellation had reached 20 Medium Earth Orbit (MEO) satellites, with the first four launched in 2013.'), ('The concept of satellite communications was first explored in the late 1950s. An experiment, launched on October 4, 1957, aimed to determine the feasibility of global broadcasts of telephone, radio, and television signals. The satellite, named Sputnik 1, successfully executed several real-time transmissions before its non-rechargeable batteries failed after eight hours of precise operation on December 30, 1958.', 'Sputnik 1, launched on October 4, 1957, was the first artificial Earth satellite. A separate mission, SCORE, launched on December 18, 1958, demonstrated active satellite communications, including relayed and recorded messages. SCORE ceased operating after about twelve days, with communications failing on December 30, 1958. The missions should not be conflated.'), ('The Birth of Satellite Relay Communications', 'Moon-Relay Communications Research'), ('The work that led to this experiment began in the field of electrical intelligence gathering at the United States Naval Research Laboratory in 1951. This research culminated in a project named "Communication Moon Relay," which laid the groundwork for future satellite communication systems.', 'The United States Naval Research Laboratory demonstrated the usefulness of Moon-reflected radio signals for communications in 1951. This separate line of research developed into Communication Moon Relay; it was not the same mission as Sputnik 1 or SCORE.')]),
        '04a38c0d50cff066': ('04a38c0d50cff066a95d83c27c0772e7e551c8403cb48561667595dff34e457f', [('The top number, known as systolic pressure, indicates the force at which blood is pumped out by the heart. The bottom number, called diastolic pressure, represents the resistance to blood flow in the arteries when the heart is at rest.', 'Systolic pressure is the pressure against artery walls when the heart beats. Diastolic pressure is the pressure against artery walls while the heart rests between beats.'), ('Anxiety does not directly cause high blood pressure, but it can contribute to its development and worsen existing hypertension.', 'Stress or anxiety can temporarily raise blood pressure. The relationship between chronic stress and sustained hypertension is more complex and remains under study.'), ('According to the American Heart Association, the stages are as follows:', 'The following American Heart Association categories apply to adults:'), ('The guidelines for blood pressure classification were updated in 2017. The new normal is less than 120/80, compared to the previous standard of less than 140/90. This change aims to reduce the risk of heart disease and stroke by encouraging earlier intervention and treatment.', 'The adult categories above were used in the 2017 guidelines. A reading below 140/90 is not automatically normal under this framework. Classification is distinct from an individual diagnosis or treatment target.'), ('Fruits and Blood Pressure\n\nSome studies suggest that consuming fruits, particularly those rich in potassium like lemons, may help lower blood pressure. However, more research is needed to confirm these potential benefits.\n\n', ''), ('Blood pressure can vary with age. In general, systolic blood pressure tends to increase with age, while diastolic blood pressure may decrease. Here are the approximate blood pressure ranges by age group:\n\n- Infants and Children: Less than 90/60\n- Adolescents: Less than 120/80\n- Adults: Less than 120/80 (Normal), 120-129/less than 80 (Elevated), 130-139/80-89 (High Blood Pressure Stage 1), 140 or higher/90 or higher (High Blood Pressure Stage 2)\n- Older Adults: Systolic blood pressure may be higher due to age-related changes in the arteries', 'Blood pressure assessment in younger children depends on age, sex, and height; a single threshold such as 90/60 is not a universal normal range. Adult categories should not be applied indiscriminately to all children. An age-related rise in blood pressure should not automatically be treated as normal.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Validation: exactly seven original source records changed by guarded rule; three batch texts unchanged; idempotent after repair; all other source records unchanged. CSV header, uniqueness, single-line notes and verdict/code consistency checked. Original source pack preserved. No full-pack cleaned export generated.

Harm test: pending Data A / Data B; no gold set supplied. Side effects: changes are substantive editorial revisions of source-derived summaries, not restorations of inaccessible pages. Row256 topic reorganization is not a claimed C4 diagnosis. Restrict to exact IDs/hashes; retain originals and before/after hashes for reversal.


## Rows261–270 — DONE: bounded repairs and classification

Added ten full-read records:265 fix/C1, other nine clean under existing reviewer-approved listed-taxonomy treatment. No generic code is forced for citation mistakes, semantic errors or closed-fence PHP bugs. Clean does not deny documented original editorial defects. Total labels: 233 unique records.

Row265 C1 medicine: document `2bb354e2f1b6cfa1`, original evidence at offset 1527; remove only final social-media promotion. Additional historical-date framing recorded as an editorial change. Sources and uncertainties are in the local audit.

Other repairs:262 scripture citation;263 rhetorical terminology;264 unverified setting and wildlife wording;266 prevalence/lab language;267 API names, sizing and performance claims;268 filename handling, unsupported benchmark table and audit failure criterion;269 Google Discover scope, responsive variants and compression qualification;270 hook schema and size-setting scope. Row261 unchanged.

```python
import hashlib

def repair_261_270(doc_id, text):
    rules = {
        '5c4ad19d0511f798': ('5c4ad19d0511f798ef9ae475fa2ec42d2b5f085b50829f100742ce2455de9d7d', [('(2 Peter 1:1)', '(Acts 3:1-10)')]),
        'e95af414b0f296e2': ('e95af414b0f296e26b6679df4fc57ce38ddcc1974080893ff87712b1a0d690ff', [('such as anadiplosis (repeating a clause for emphasis)', 'such as repetition and parallel phrasing')]),
        'f3f8643e47e7922c': ('f3f8643e47e7922c911ac009c000873ffd2466200693a59a423b6ad386ee48af', [("Setting of Brown Bear, Brown Bear, What Do You See\nUnlike many children's books that transport readers to fantastical or adventurous settings, Brown Bear, Brown Bear, What Do You See? is set in a classroom. This choice of setting makes the book relatable and accessible to its young audience, who are familiar with the school environment.\n\n", ''), ('Brown bears, also known as grizzly bears, are omnivorous,', 'Brown bears, including grizzly bears, are omnivorous,'), ('Unlike black bears, brown bears have evolved with long, strong claws that serve multiple purposes. These claws aid in digging for food, such as roots and tubers, picking fruits from trees, and even catching prey.', 'Brown bears have long claws adapted for digging for food such as roots and burrowing animals.')]),
        '2bb354e2f1b6cfa1': ('2bb354e2f1b6cfa1efcc51f276cf508786540bc1ddd6f86a498ed582ba0929f4', [('Scientists from The Mammoth Site in Hot Springs, South Dakota, are currently in Santa Barbara, California,', 'In this February 2017 report, scientists from The Mammoth Site in Hot Springs, South Dakota, are in Santa Barbara, California,'), ("\n\nThe Mammoth Site's Facebook page will provide regular updates on Bugbee's progress in the coming weeks.", '')]),
        '94e65065d76f1b44': ('94e65065d76f1b44c6f523ae9c7f376fe582d14043346475af7da86543ea173c', [('Vitamin C Deficiency, or Scurvy: A Historical and Clinical Overview', 'Vitamin C Deficiency and Scurvy: A Historical and Clinical Overview'), ('Vitamin C deficiency, commonly known as scurvy, is a nutritional disorder predominantly linked to socioeconomic status and access to adequate food.', 'Severe, prolonged vitamin C deficiency causes scurvy. Risk can reflect restricted intake, food access, and other factors affecting nutritional status.'), ('2. Laboratory Tests: A simple blood test can confirm a vitamin C deficiency. Low levels of ascorbic acid in the blood are indicative of the condition. However, more advanced tests, such as measuring ascorbic acid in white blood cells, may be necessary for accurate diagnosis.', '2. Laboratory Tests: Vitamin C measurements can support assessment alongside dietary history and clinical findings. A laboratory result should be interpreted in its clinical context rather than treated as the only basis for diagnosing scurvy.'), ('Today, widespread awareness of the importance of a balanced diet and ready access to fresh fruits and vegetables have largely eliminated scurvy in developed countries. However, it remains a concern in impoverished communities and areas affected by famine or conflict.', 'Scurvy is uncommon but can occur when vitamin C intake is severely restricted, including among people in high-income countries. Risk is not confined to famine or conflict settings.')]),
        'f160996522775c21': ('f160996522775c21c71e0f7f1baf74bbf96b645602bb67a5d82c49bc0b4054b2', [('WordPress, by default, generates four image sizes: thumbnail (150x150 pixels), medium (300x300 pixels), large (1024x1024 pixels), and full size (original uploaded dimensions). However, the full-size images cannot be altered in the Media Settings. To edit the original image, use external software before uploading.', 'WordPress generates intermediate image sizes according to its version, media settings, themes, and plugins. Full size refers to the uploaded image rather than a fixed intermediate size. The Media Library offers image-editing tools; Settings > Media configures default size limits.'), ("When uploading images, consider your theme's content width. For instance, if your theme's content width is 695 pixels, ensure your images are no wider than this to maintain quality. WordPress will scale down larger images to fit the content block, but this may lead to quality loss.", 'Match image delivery to the layout and display density. A higher-resolution source can be appropriate for a high-density screen; responsive image markup helps browsers select an appropriate file.'), ('For responsive design, consider device-specific image sizes. For example, an image designed for an iPhone 3 should be no wider than 320 pixels.', 'For responsive design, consider layout width and device pixel density rather than imposing one universal source-image width.'), ('To display full-size images without slowing down your site, use a lightbox.', 'A lightbox can display a larger image when a thumbnail is selected. Its performance depends on when the larger file is loaded.'), ('which open to full size when clicked. This method provides the best of both worlds: fast loading times and full-size image viewing.', 'which can open a larger version when clicked. This alone does not guarantee fast loading.'), ('maximum possible compression', 'a suitable balance of compression and visual quality'), ('`updateoption()`', '`update_option()`'), ("the'mediumlarge'", 'the `medium_large`'), ("updateoption('mediumlargesizew', 768 );\nupdateoption('mediumlargesizeh', 0 );", "update_option( 'medium_large_size_w', 768 );\nupdate_option( 'medium_large_size_h', 0 );"), ('You can adjust other default image sizes using the same function.', 'You can adjust other supported image-size options using the same function. Changed settings apply to newly generated sizes; existing uploads need regeneration to obtain revised variants.')]),
        '10ad4d83d447f5ec': ('10ad4d83d447f5ecbf872a26ab0476147eb8077df313ca1c860b896975a1f360', [("However, these automatically generated images have filenames with numbers, which aren't SEO-friendly. To improve SEO, consider renaming these files.", 'Generated filenames commonly include dimensions. Renaming generated files without updating their references and attachment metadata can break images; do not rename them merely to remove the dimension suffix.'), ('For instance, if your theme has a content area of 600 pixels, use images that are 600 pixels wide or smaller to prevent layout disruptions.', 'Choose source dimensions for the rendered layout and display density; responsive variants can serve different resolutions without forcing a universal 600-pixel source limit.'), ('2. Compress Images: Use plugins like ShortPixel, Smush, or Imagify to compress images without losing quality. These plugins can reduce file sizes significantly, as shown in the comparison table below:\n\n| Plugin         | Compressed File Size (avg. reduction) |\n|----------------|---------------------------------------|\n| ShortPixel     | 50-60%                                |\n| Smush          | 30-50%                                |\n| Imagify        | 40-60%                                |', '2. Compress Images: Compare output size and visual quality using consistent source images and settings. Lossless and lossy modes have different tradeoffs; no universal plugin compression percentages are established here.'), ("To pass Google's 'Properly size images' audit, ensure that the rendered size of each image is at least 4KiB smaller than the actual size. For background or hero images, consider using a size like 1920px or 2560px, which is suitable for most modern devices and screen resolutions.", 'The legacy Lighthouse audit estimates savings from serving images matched to their displayed size. Its 4 KiB threshold identifies potential byte savings that trigger a failure; it is not a requirement for passing. Choose dimensions for the actual layout and display density.')]),
        '6011e88c8cc9c77d': ('6011e88c8cc9c77db7722103b6d8d83729068f7f610ba07895a626e6c1104880', [("While WordPress can display images at smaller sizes, it doesn't reduce the underlying file size.", 'CSS scaling alone does not reduce the downloaded file size. WordPress can also generate smaller files and responsive markup so browsers can select an appropriate variant.'), ('To fix this, you can change the default image sizes set by WordPress.', 'Use appropriate generated variants and responsive image markup; changing default dimensions alone does not ensure responsive delivery.'), ("4. Follow Google's Image Requirements: According to Google, images should be at least 1200 pixels wide, and every page should contain at least one image. Images should be in.jpg,.png, or.gif format.", '4. Apply the Relevant Google Guidance: The 1200-pixel recommendation concerns large image previews in Discover, not a universal minimum for every image or a requirement that every page contain an image. Follow the requirements for the relevant feature rather than restricting all images to three formats.'), ('WebP images are around 30% smaller than their counterparts, leading to faster loading pages and improved user experience.', 'Compression savings depend on the source image, quality setting, and encoding method; no fixed percentage applies to every image.')]),
        '42d8419d13115795': ('42d8419d13115795c7fa78b6340cf3372571e14e9f4c8d31458d990823d9b59c', [("By default, images added using this block are centrally aligned. However, you can easily change this alignment to either left or right using the block's options.", 'Image alignment depends on the block settings and theme. The block toolbar provides available alignment options.'), ('WordPress automatically crops images to increase site speed.', 'WordPress generates intermediate image sizes; cropping depends on the size configuration.'), ('- Hero Images: For optimal sharing on social platforms, use hero images with dimensions of 1200×630 pixels. You can use the Yoast SEO plugin to associate your social posts with these hero images.', '- Hero Images: Choose dimensions for the page layout and the requirements of the intended social-preview platform; one dimension is not optimal for every use.'), ("addfilter('intermediateimagesizesadvanced','myintermediateimage_sizes');\nfunction myintermediateimage_sizes($sizes) {\n    unset($sizes['medium_large']);\n    $sizes['medium_large'] = 975;\n    return $sizes;\n}", "add_filter( 'intermediate_image_sizes_advanced', 'review_medium_large_size' );\nfunction review_medium_large_size( $sizes ) {\n    $sizes['medium_large'] = array(\n        'width' => 975,\n        'height' => 0,\n        'crop' => false,\n    );\n    return $sizes;\n}"), ('WordPress creates additional image sizes (like Medium, Large, etc.) upon upload. To change the default medium_large size, you can add the following code to your functions.php file:', 'WordPress creates intermediate sizes upon upload. This filter changes the medium_large configuration for newly generated images; existing images need regeneration:'), ('- Default Thumbnail Size: You can set a new default thumbnail size in WordPress, which will be used for all images. To do this, go to Settings > Media and choose your desired size.', '- Default Thumbnail Size: Settings > Media changes the thumbnail-size configuration for newly generated thumbnails. It does not replace every size used across the site; existing thumbnails may need regeneration.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Validation: exactly nine source records changed by rule, one unchanged; exact ID/hash guards; idempotent repaired text; all other records unchanged. Correct PHP API/hook names and array keys checked against official docs; fences and braces balanced. PHP executable/WordPress runtime unavailable: no PHP execution or integration success claimed. CSV header, uniqueness, single-line fields and verdict/code agreement checked. Full original/applied text and before/after hashes remain in local audit. Original pack preserved; no whole-pack cleaned export created.

Harm test: pending Data A / Data B; gold set not supplied. Side effects: substantive editorial changes alter source-derived content, remove an unverified setting and unmeasured plugin comparisons, and change future image-generation configuration in code examples. Regeneration requirements stated; do not apply as a generic corpus rule or run code on a live website.


## Rows271–280 — DONE: bounded repairs and classification

Added ten full-read records:272 fix/F1; other nine clean under existing reviewer-approved listed-taxonomy treatment. Original semantic errors remain documented; no generic factual-error code invented. Total labels: 243 unique records.

F1 medicine: document `12693fe362d15d0d`; evidence contact-name type at original offset 1087, phone type at original offset 1110; hex empty. Remove original span 1024:1166 (registration sentence). Actual values never included in rule/evidence, and originals masked in review artifacts. The email text was already a placeholder; do not invent an actual leaked email. Preserve dated2014 event logistics. Original notice unavailable; conservative removal follows public-repo privacy requirement, without asserting a private email was found.

Other editorial repairs:271 plugin-specific controls and unverified metrics;273 long-break timing, task applicability and brain-mechanism qualification;274 product/course scope and job guarantee;275 registration/keeper/owner distinctions and V888;276 lunar phase period and historical uncertainty;277 Roman letters/week length and deity naming;278 origin/cultural framing and Saturday;279 commerce/schedule scope;280 greater-of GCWR definition. Original pages often unavailable; authoritative corroboration and unresolved source details recorded per row.

```python
import hashlib

def repair_271_280(doc_id, text):
    rules = {
        '5c27d8262afe0d6d': ('5c27d8262afe0d6d90f36167f01f8953029b00f87c5254d9f242d750f456459f', [('replace', "This involves reducing file sizes, which can be achieved using plugins like the Prizm Image Optimization plugin. According to Accusoft, the plugin's developer, it can decrease image file sizes by up to 70% without compromising resolution or visual quality. With approximately 4,000 downloads and a rating of 3.5 out of 5.0, the plugin is steadily gaining traction.", 'Reducing image file sizes can help, but compression savings and visual quality depend on the source files and settings. Historical plugin download counts, ratings, and compression claims are not guarantees of present performance.'), ('replace', "To ensure the correct image size is preloaded, go to the 'Settings' menu, then 'Preload Featured Images', and select the image size used by your theme.", 'When the Preload Featured Images plugin is installed, its Settings page lets users choose an image size. This menu is a plugin feature, not a universal WordPress core setting.'), ('replace', "In the WordPress post editor, you can set a default featured image. There are two buttons next to the image: 'Select default featured image' (to choose a different image) and 'Don't use a default featured image' (to remove the previously selected default image without selecting a new one).", 'The Default Featured Image plugin can provide a fallback featured image. Its controls require that plugin and may differ by version; they are not universal core post-editor buttons.')]),
        '12693fe362d15d0d': ('12693fe362d15d0d98db6692a8a1f47d34481adf85677a5bf359aab419e51dca', [('remove_span', 1024, 1166)]),
        '55df24973aea7de6': ('55df24973aea7de6dfbc0fd87c0de591a1b7cf740d04b360aa2d94354edc0454', [('replace', 'after which a longer break of 20-30 minutes is taken.', 'after which a longer break, commonly 15-30 minutes, is taken.'), ('replace', '5. After the fourth Pomodoro, take an extended break of 20-30 minutes.', '5. After the fourth Pomodoro, take a longer break, commonly 15-30 minutes.'), ('replace', "The Pomodoro Technique works because it harnesses the brain's natural ability to focus and process information effectively. It allows the brain to 'background process' new information during breaks, enhancing learning and retention. Additionally, it helps to organize and structure breaks, preventing distractions and ensuring one stays on track.", 'The method provides a structure for focused work and planned breaks. Its usefulness can vary between people and tasks; this description does not establish a particular brain mechanism or guarantee improved learning.'), ('replace', 'but they only work for computer-based tasks.', 'and their alarms can also time tasks away from the computer.')]),
        '343383323ee7533b': ('343383323ee7533bfa1e6a249c729e9fce376c2ab0805641d4f79791854a85ca', [('replace', 'This comprehensive course is tailored for professionals, students, and hobbyists, covering a wide range of tools for image processing, 2D modeling, 3D modeling, and animation. It begins with foundational concepts in graphic design and progresses to advanced topics, ensuring that students are job-ready upon completion. The curriculum also covers related software such as Adobe PageMaker, Adobe InDesign, Corel Draw, and Adobe Photoshop.', 'The course description is aimed at professionals, students, and hobbyists interested in Illustrator and graphic design. Illustrator is primarily a vector-graphics application. Broader claims about an extensive multi-application, 3D, or animation curriculum and guaranteed job readiness require confirmation from the training provider.'), ('replace', "It's essential for users to understand that the software has a function for almost every need, but finding the right tool can be challenging.", 'Learning the relevant tools and workflows helps users navigate the application; it should not be presented as a tool for every need.')]),
        '8d108ac128e2941d': ('8d108ac128e2941df2454770f157dcbf438b3410f13761cf60d0b7e1acfc0b17', [('replace', 'Number plates follow a specific format: the first two letters are a DVLA memory tag, followed by numbers indicating the month and year of manufacture, and ending with three randomly allocated letters.', 'The current standard registration format used in Great Britain has two memory-tag letters, a two-digit age identifier indicating the registration period, and three other letters. The age identifier is not the month and year of manufacture, and other registration formats also exist.'), ('replace', "A number plate check does not reveal the owner's details, but it provides comprehensive vehicle-related data.", "A public vehicle check does not disclose a registered keeper's identity. Registered keeper and legal owner are not necessarily the same person."), ('replace', 'Organizations registered with the International Parking Community or British Parking Community can also request owner information for issuing parking notices. Individuals can apply using a V88 form, with charges displayed on the form itself.', 'Parking companies can apply for registered-keeper information under DVLA disclosure conditions; access is not automatic. An individual can apply using form V888, with any applicable fee and evidence requirements stated by DVLA.')]),
        '0844c74e3bab12fc': ('0844c74e3bab12fcefcd8af6ba7308e76ab5bfdf85483c0ea754e3ed3484412d', [('replace', 'The seven-day week is deeply rooted in ancient astronomical observations and cultural beliefs. To understand its origins, we must journey back to early Mesopotamia, around the first millennium BCE, where the Babylonians were making significant strides in astronomy.', 'The seven-day week reflects several historical traditions, including the biblical Sabbath cycle and the planetary week of ancient astrology. A simple account attributing it to one culture or one astronomical observation is disputed.'), ('replace', "The Babylonians were meticulous observers of the heavens. They noted that the Earth's rotation on its axis marks a day, and its revolution around the sun marks a year. They also observed the moon's phases, which occur roughly every 27 days. To manage time more effectively, they divided the lunar cycle into four equal parts, each consisting of about seven days. This division led to the concept of the week.", "Babylonian astronomy carefully tracked celestial cycles. The visible lunar phase cycle lasts about 29.5 days, not 27 days. Four seven-day periods total 28 days, so the week is not an exact quarter of that cycle. Modern explanations of Earth's rotation and orbit should not simply be attributed to ancient Babylonian observers."), ('replace', 'The Babylonians observed seven significant celestial bodies with the naked eye: the five known planets (Mercury, Venus, Mars, Jupiter, and Saturn), the sun, and the moon. They believed these celestial bodies to be gods and named the days of the week after them. Thus, the number seven became deeply significant in their culture and was adopted as the basis for the week.', 'The planetary week associated days with the Sun, Moon, and five planets visible without a telescope: Mercury, Venus, Mars, Jupiter, and Saturn. Babylonian astrology influenced later Hellenistic traditions, but that does not establish a single direct account of how every seven-day week originated.')]),
        'be8e89471a939e9c': ('be8e89471a939e9c5799d0a12cf1be18a492e6faa4377606f0284c050eb5eff4', [('replace', "The Egyptians tracked the phases of the moon over approximately ten-day periods, which likely influenced their initial concept of a ten-day week. However, the Egyptians were not the only civilization to deviate from the seven-day week. The Babylonians, for instance, also employed an eight-day week. Despite the Egyptians' influence, the Babylonian seven-day week eventually became the global standard.", 'Ancient Egyptian calendars included ten-day groupings. The Roman market cycle used eight days. These should not be confused with the lunar phase cycle or attributed interchangeably to Babylonian practice. The seven-day week developed through several interacting traditions.'), ('replace', "They identified days using dominical letters (A-G) instead of proper names. However, this system began to shift around the 3rd century AD when the Romans started adopting the seven-day week from Alexander the Great's territory. In 321 AD, Emperor Constantine declared the seven-day week the standard, with Sunday as a holiday.", "Roman calendars marked the eight-day market cycle with letters A-H. The planetary seven-day cycle spread gradually in the Roman world alongside other traditions; it did not begin through a simple third-century transfer from Alexander's territory. Constantine's Sunday legislation in 321 AD should not be treated as the creation of the seven-day week."), ('replace', 'Tiw, the sky god', 'Tiw'), ('replace', 'Woden, the god of war', 'Woden'), ('replace', 'Friday, Frigedæg, was dedicated to Freyja, the goddess of love.', 'Friday, Frigedæg, preserves the name of a Germanic goddess, commonly identified as Frigg.')]),
        '87f15ef68ab716f6': ('87f15ef68ab716f6d2b435f071799b6aa5a5d954b3277ef311235e0ddcfffa59', [('replace', 'The Nordics and Anglo-Saxons, both Germanic peoples, initially practiced Paganism, worshipping the ancient Germanic gods. While they shared many cultural aspects, they differed in their religious practices. The Nordics continued to venerate the original Pagan gods, such as Odin, Thor, and Frigg, while the Anglo-Saxons adopted these gods before converting to Christianity. This religious divergence is reflected in the names of the days of the week.', 'Norse and Anglo-Saxon peoples had related Germanic religious and linguistic traditions. Weekday names reflect adaptations of the planetary week and inherited local names, rather than a simple division between one group keeping original gods and another adopting them.'), ('replace', 'The seven-day week originated in early AD Babylonia, where astrologers observed the solar cycle and associated it with human concepts of time. They named the days after celestial bodies and deities, influencing the Roman, Germanic, and Anglo-Saxon calendars.', 'The planetary week of ancient astrology and the biblical seven-day cycle contributed to the history of the week. Its origin should not be reduced to an early-AD Babylonian observation of the solar cycle.'), ('replace', "- Monday (Moon's day) and Sunday (Sun's day) are remnants of the Babylonian influence.\n- Tuesday through Saturday are named after Norse and Anglo-Saxon gods, reflecting the Germanic influence.", '- Monday and Sunday refer to the Moon and Sun.\n- Tuesday through Friday preserve Germanic deity names.\n- Saturday retains the name of Saturn, rather than a Germanic deity.')]),
        'ff3d7d0aed0b3f38': ('ff3d7d0aed0b3f38287fecf85d024c494d104dacd4bedf5b3f6af550d1619e2f', [('replace', '- Intrastate Commerce: This refers to transportation that does not involve crossing state lines.', '- Intrastate Commerce: This is transportation outside the federal definition of interstate commerce. A journey wholly within one state can still be interstate commerce if it is part of a qualifying movement originating or ending outside that state.'), ('replace', 'If you operate in interstate commerce, you must comply with the Federal hours-of-service regulations while doing so and for a short period afterward, depending on the schedule used (60-hour/7-day or 70-hour/8-day).', 'Applicable hours-of-service rules depend on the operation, prior duty history, and relevant exceptions. The 60-hour/7-day and 70-hour/8-day limits do not by themselves resolve every compliance question.'), ('replace', "Even when a truck is empty, it may still be considered in commerce if it's used to support a business. If you're operating an empty truck outside your state or recently carried cargo in interstate commerce within your state, it's considered interstate commerce. However, if the truck is empty and you're operating within your state without recent interstate cargo, it's considered intrastate commerce.", 'An empty truck can still be operating in commerce when supporting a business. Interstate status depends on the nature of the transportation, not simply whether the vehicle is empty or its current leg stays within one state. Personal use unrelated to business is a separate question.')]),
        '40ba838c6dd54c09': ('40ba838c6dd54c09bde1e2bfaed7dc0921a51fe163272466c50befe7f88c430b', [('replace', 'Your truck may also display a gross combination weight rating (GCWR) in the same manner as the GVWR. If not, to calculate the GCWR, add the highest GVWR or gross vehicle weight of the power unit (the truck or tractor) and the towed unit (the trailer or other vehicle being towed).', "For the federal commercial-motor-vehicle definition, GCWR is the greater of the manufacturer's power-unit value displayed on the required certification label and the highest combined total obtainable from the ratings or actual weights of the power unit and towed units. It is not only a fallback sum used when a manufacturer value is absent.")]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for operation in rule[1]:
        if operation[0] == "remove_span":
            text = text[:operation[1]] + text[operation[2]:]
        else:
            old, new = operation[1:]
            if text.count(old) != 1:
                raise ValueError("Unexpected reviewed source text")
            text = text.replace(old, new, 1)
    return text
```

Validation: exactly ten original source records changed by the guarded rule, all others unchanged; repaired text idempotent. F1 values absent from revised record, evidence, rule and appended audit. CSV header, uniqueness, single-line fields and verdict/code consistency checked. Before/after hashes and full masked-original/applied text recorded locally. Original pack preserved; no full-pack cleaned export generated.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial changes alter meaning/structure; removing direct contact loses historical registration instructions. Legal corrections use primary definitions checked during review, not a full compliance determination or restoration of all source-era law. Old Norse forms and some historical details remain source-derived rather than independently collated. Exact IDs/hashes only; no generic corpus cleaning or contacting anyone.


## Rows281–290 — DONE: source review and bounded repairs

Added ten full-read records:284 fix/C1; other nine clean under existing reviewer-approved listed-taxonomy treatment. Do not force a generic factual-error or F3 code merely for medical/occupational discussion; the original bandana assertion was contradictory with its own counterwarning and has been explicitly corrected as an editorial safety error. Clean does not deny original errors or certify all source claims. Total labels: 253 unique records.

C1 medicine: document `df85305d590a5f2b`; closing conversion-page/bookmark promotion begins at original offset 1052. Remove only the CTA. Additional source-specific conversion corrections recorded separately:453.59237g per pound divided by198g/US cup rounds to2.3 cups, and divided by113g/US cup rounds to4.0 cups. Do not generalize these densities to all sugars/recipes.

Other repairs:281 qualify club terms/remove unsupported FAQ topics;285 replace unsafe PPE assertion and qualify cancer evidence, remove unverified unrelated figures;287 restore aquarium scope and power/energy distinction;288 organization identity and undated statistics attribution;289 research/model scope;290 historical frame/remove reader-comment conflation and unsupported climate explanation.282,283,286 unchanged. Source limits and references recorded per row locally.

```python
import hashlib

def repair_281_290(doc_id, text):
    rules = {
        'ae3d3636b3deb187': ('ae3d3636b3deb187476edd5de2f8fbe38cc95c23e38a7152573895e8c4cf3b11', [('It offers a unique, convenient membership option with over 2,000 locations worldwide, providing 24/7 access to its facilities. Upon signing up, members pay a one-time program fee of $49 (standard fee is $129), after which they receive a membership card in the mail, granting them immediate access to any Snap Fitness gym.', 'Membership, access arrangements, equipment, joining fees, and other terms depend on the club and market. The chain offers access across its club network, subject to the applicable membership conditions.'), ('Snap Fitness does not charge a cancellation fee if a member agrees to transfer their membership to a non-member who pays the standard joining fee. To cancel a membership, one can simply contact their local club directly, either by phone, email, or in person during staffed hours.', 'Cancellation and transfer arrangements depend on the membership agreement and local club. Members should use the cancellation process specified by their home club; a fee-free transfer is not a universal guarantee.'), ('\n\nSnap Fitness and Snapchat Age Limits\n\nIt is essential to note that Snap Fitness and Snapchat are two separate entities. While Snap Fitness has no age restrictions for membership, the minimum age for Snapchat users is 13 years old.\n\nRecent Events\n\nIn recent news, a Snap Fitness franchise closed suddenly, leaving a note on its door stating "Sorry but I\'m broke." The franchise owner\'s unexpected closure left many members without access to their gym, highlighting the importance of understanding a business\'s financial stability and cancellation policies before committing to a membership.', '\n\nAge Requirements\n\nAge restrictions vary by club and market. Membership is not universally available without age restrictions.')]),
        'df85305d590a5f2b': ('df85305d590a5f2bb2dd073a916c3a9793f98d92b2ab7d402135a68305532473', [("The conversions provided here assume you're spooning the sugar into your measuring cup and leveling it off with a knife, without compacting it. Scooping sugar directly from the jar or bag with your cup may lead to different measurements.", "Cup-to-weight conversions depend on the sugar and measuring method. The examples below use US cups and King Arthur Baking's ingredient weights; they are approximate, not universal density constants."), ("- Granulated Sugar: 1 pound is approximately 4 cups. However, if you pack the sugar firmly into the cup, you'll get about 3.5 cups in a pound.", '- Granulated Sugar: Using 198 grams per US cup and about 453.6 grams per pound, 1 pound is approximately 2.3 US cups.'), ('- Powdered Sugar (Icing Sugar): 1 pound of unsifted powdered sugar is roughly 3.75 cups. Sifting the sugar before measuring can increase this to about 4 cups.', "- Powdered Sugar (Icing Sugar): Using 113 grams per US cup for unsifted confectioners' sugar, 1 pound is approximately 4 US cups. Sifting changes volume, so follow the recipe's measuring instructions."), ('- Raw Cane Sugar: 1 pound of this type of sugar is approximately 3.5 cups.', '- Raw Cane Sugar: Crystal size and product type vary; use a weight specified by the recipe or product rather than a universal cups-per-pound conversion.'), (' Additionally, you can find conversions for other ingredients, including flour, on our baking conversions page. For a more comprehensive guide, bookmark our overall cooking recipe converter.', '')]),
        'bb69c46d1b1f15c5': ('bb69c46d1b1f15c598fae38a7f65b2314318366027667b4f43de684e2a17cd69', [('The most commonly reported cancers among firefighters include:\n- Brain\n- Colorectal\n- Prostate\n- Urinary\n- Thyroid\n- Testicular\n- Blood cancers', "IARC's 2022 evaluation classified occupational exposure as a firefighter as carcinogenic to humans. It found sufficient evidence for mesothelioma and bladder cancer, and limited evidence for several other cancers. These evidence categories are not a ranking of the most common cancers in every firefighter population."), ("Life Expectancy and Salary\n\nThe average life expectancy of a firefighter at age 60 is 24 years for men and 26 years for women, according to a study. This is comparable to the general population, with a difference of just one year.\n\nThe salary of a firefighter in the United States varies, with the median salary being around $48,208 per year. Here's a breakdown of the salaries at different percentiles:\n- 25th percentile: $36,157\n- 50th percentile (median): $48,208\n- 75th percentile: $60,261\n- 90th percentile: $71,234\n\n9/11 Tragedy\n\nThe September 11 attacks in 2001 claimed the lives of 415 emergency workers in New York City, including 343 firefighters from the New York City Fire Department (FDNY).\n\n", ''), ('Despite the risks, firefighters are only recommended to carry a bandana as respiratory protective equipment. However, the Environmental Protection Agency (EPA) and other health agencies warn that bandanas do not effectively reduce particulate exposure.', 'Bandanas are not a substitute for approved respiratory protection. Structural and wildland firefighting involve different protection requirements; respiratory protection must be selected for the hazards and the applicable respiratory-protection programme. NIOSH specifically advises against using non-approved face coverings in place of approved respirators during wildland fire suppression.')]),
        'dd66702c1d2b71b1': ('dd66702c1d2b71b1c7ce0a65bb089d0b444ae679d60060c2b91af0b364747e48', [('Electric Heater Efficiency and Lifespan\n\nWhen choosing an electric heater to reduce electricity bills, considering wattage alone is not sufficient. Heaters do not run continuously, and their cycling on and off can impact their lifespan. A 150-watt heater might maintain the same temperature as a 200-watt one, but the latter will cycle on and off more frequently, potentially leading to earlier failure due to contact issues.\n\nHeater Contact Failure\n\nMost heater failures are due to contact problems, not coil malfunctions. Heater contacts, typically found inside a glass or metal tube, open and close based on temperature changes, controlled by a bi-metal spring. Larger heat coils cause more frequent cycling, leading to quicker contact failure. This is because the contacts are tiny and the frequent opening and closing wears them out. Thus, a smaller heater coil may last longer, despite requiring more time to heat up.\n\nPractical Considerations\n\nIn practice, finding a heater that stays on just long enough to maintain a constant temperature is ideal but unlikely. The goal should be to find the smallest heater that can effectively heat the room under various conditions. This approach balances energy efficiency with heater lifespan, as replacing heaters frequently can offset the savings from using a smaller, less efficient model.\n\nAlternative Energy Savings\n\nWhile choosing an efficient heater is important, other measures can also significantly reduce electricity bills. For instance, turning off a computer for an hour daily can result in more substantial savings than switching to a more efficient heater. However, this option might not be practical for everyone.', "Aquarium Heater Power, Energy Use, and Sizing\n\nA heater's wattage is its power while running, not its total energy use. Energy depends on power multiplied by operating time. For example, 150 watts for one hour and 200 watts for three-quarters of an hour each use 0.15 kilowatt-hours.\n\nAquarium Context\n\nThe original discussion concerns a 55-gallon aquarium, not room heating. Heater selection depends on aquarium volume, the required water temperature, surrounding conditions, and the manufacturer's sizing guidance.\n\nEfficiency and Cycling\n\nElectric resistance heating converts electrical input to heat. A lower wattage does not by itself demonstrate lower energy consumption when maintaining the same conditions; runtime also matters.\n\nReliability\n\nWattage alone does not establish how often a thermostat will cycle or how long a heater will last. The specific heater design and operating conditions matter. The smallest available heater is not automatically suitable for an aquarium.\n\nComparing Other Energy Uses\n\nSavings from switching off another appliance depend on that appliance's power and operating time. No universal comparison follows from a heater's rated wattage alone.")]),
        'be3d178d1dc3e00d': ('be3d178d1dc3e00d7c672f1586669729130228459baef593e1c1cf260cbd0b3b', [('Myanmar Agriculture and Agribusiness Research Institute (MAGRI)', 'Myanmar Agriculture Network (MAGri)'), ("This focus on GAPs is particularly significant in Myanmar, where the agriculture sector contributes a substantial 30% to the nation's Gross Domestic Product (GDP).", "The network's undated sector page reports agriculture as contributing 30% of national GDP; this is a source-reported figure, not an independently dated current estimate."), ('Approximately 68% of the rural population depends on crop husbandry and livestock for their income and sustenance.', 'The same page reports that approximately 68% of the rural population relies on crop husbandry and livestock for livelihoods and income.'), ('MAGRI is dedicated', 'MAGri is dedicated')]),
        '221ac460c036788a': ('221ac460c036788a89f3d0200ea3fb1fde154538503fc6e86c4008f88adb03d2', [('Pearls, renowned for their flawless, spherical beauty, pose an intriguing question: how do these macroscopic biological structures achieve such perfect roundness?', 'Some pearls are nearly spherical, while others are irregular or have different symmetries. The round examples pose an intriguing question: how do these macroscopic biological structures achieve their shape?'), ("The answer lies in the unique texture of a pearl's surface, which resembles a ratchet.", "In the researchers' model, the texture of a pearl's surface acts like a ratchet."), ("This self-organized rotational process is responsible for the pearl's spherical shape.", 'The authors propose this self-organized rotational process as a mechanism shaping nearly spherical pearls.')]),
        '6f1ae1cd04963fe8': ('6f1ae1cd04963fe8b7c40cb1ec993079504b03b49447b9ef7f3919bf990d3a71', [("Sochi's Unusual Climate: A Subtropical Oasis in Russia\n\nSochi, a city in western Russia, is renowned for its unique climate, which is quite different from what one might expect in a country known for its harsh winters. Stretching along the Black Sea for 147 kilometers, Sochi enjoys a subtropical climate, making it one of the warmest cities in its latitude, comparable to Boston, Massachusetts in the United States.\n\nYear-Round Mild Temperatures\n\nSochi's weather is particularly appealing to vacationers. Between June and October, water temperatures average a comfortable 76°F (24.4°C), while air temperatures range between 79°F and 81°F (26°C - 27°C). Conversely, during the cooler months from January to April, daytime temperatures average a mild 52°F (11°C), with nighttime temperatures dropping to around 39°F (4°C). While these figures might seem chilly to residents of warmer climates, they are considered mild winter temperatures by European and Russian standards.\n\nCauses of Sochi's Mild Climate\n\nSochi's mild year-round temperatures can be attributed to two primary factors. Firstly, its location on the Black Sea, a warmer inland sea, provides a buffer against colder ocean temperatures. Secondly, its proximity to warmer regions like Turkey and the Middle East also contributes to its mild climate. Additionally, the surrounding mountains act as a shield, protecting the city from the freezing winds and temperatures of its northern neighbors.\n\nHosting the Winter Olympics\n\nDespite its subtropical climate, Sochi was chosen as the host for the XXII Olympic Winter Games. The nearby Greater Caucasus Mountains provide the necessary cold temperatures for winter sports, with average temperatures ranging from 35°F to 32°F (2°C - 0°C) between January and April. These temperatures, along with the availability of snow at higher elevations, make the mountains suitable for winter sporting events.\n\nComparisons with Other Winter Olympics Destinations\n\nWhile Sochi's climate is mild, it is not the warmest among Winter Olympics destinations. For instance, the Lighthouse Park region of Metro Vancouver, where the last Winter Olympics were held, has normally warmer winter seasons than some suburbs of Dallas, Texas, as defined by mean air temperature.", "Historical travel account: September 2012, before the 2014 Winter Olympics\n\nSochi's Unusual Climate: A Subtropical Oasis in Russia\n\nSochi, a city in western Russia, is renowned for its unique climate, which is quite different from what one might expect in a country known for its harsh winters. Stretching along the Black Sea for 147 kilometers, Sochi enjoys a subtropical climate, making it one of the warmest cities in its latitude, comparable to Boston, Massachusetts in the United States.\n\nYear-Round Mild Temperatures\n\nSochi's weather is particularly appealing to vacationers. Between June and October, water temperatures average a comfortable 76°F (24.4°C), while air temperatures range between 79°F and 81°F (26°C - 27°C). Conversely, during the cooler months from January to April, daytime temperatures average a mild 52°F (11°C), with nighttime temperatures dropping to around 39°F (4°C). While these figures might seem chilly to residents of warmer climates, they are considered mild winter temperatures by European and Russian standards.\n\nCauses of Sochi's Mild Climate\n\nSochi's mild year-round temperatures can be attributed to two primary factors. Firstly, its location on the Black Sea, a warmer inland sea, provides a buffer against colder ocean temperatures. Secondly, its proximity to warmer regions like Turkey and the Middle East also contributes to its mild climate. Additionally, the surrounding mountains act as a shield, protecting the city from the freezing winds and temperatures of its northern neighbors.\n\nHosting the Winter Olympics\n\nDespite its subtropical climate, Sochi was chosen as the host for the XXII Olympic Winter Games. The nearby Greater Caucasus Mountains provide the necessary cold temperatures for winter sports, with average temperatures ranging from 35°F to 32°F (2°C - 0°C) between January and April. These temperatures, along with the availability of snow at higher elevations, make the mountains suitable for winter sporting events.\n\nComparisons with Other Winter Olympics Destinations\n\nWhile Sochi's climate is mild, it is not the warmest among Winter Olympics destinations. For instance, the Lighthouse Park region of Metro Vancouver, where the last Winter Olympics were held, has normally warmer winter seasons than some suburbs of Dallas, Texas, as defined by mean air temperature."), ("\n\nComparisons with Other Winter Olympics Destinations\n\nWhile Sochi's climate is mild, it is not the warmest among Winter Olympics destinations. For instance, the Lighthouse Park region of Metro Vancouver, where the last Winter Olympics were held, has normally warmer winter seasons than some suburbs of Dallas, Texas, as defined by mean air temperature.", ''), ("Causes of Sochi's Mild Climate", 'Coastal and Mountain Settings'), ("Sochi's mild year-round temperatures can be attributed to two primary factors. Firstly, its location on the Black Sea, a warmer inland sea, provides a buffer against colder ocean temperatures. Secondly, its proximity to warmer regions like Turkey and the Middle East also contributes to its mild climate. Additionally, the surrounding mountains act as a shield, protecting the city from the freezing winds and temperatures of its northern neighbors.", "Sochi's coastal setting differs from the higher-elevation Caucasus venues used for outdoor winter sports. The presence of snow on the mountain slopes does not imply the same conditions at the coast.")]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Validation: exactly seven original source records changed; three batch texts unchanged; exact ID/hash guards; repaired text idempotent; all other source records unchanged. Arithmetic checked for sugar conversions and150W×1h =200W×0.75h =0.15kWh. Unsafe bandana claim absent; corrected organization name present. CSV header, uniqueness, single-line notes and verdict/code agreement pass. Original pack preserved; full original/applied text and before/after hashes in audit. No whole-pack cleaned export created.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: substantive editorial changes remove source-derived details and introduce clearly documented authoritative corrections. They are not restorations of inaccessible pages, a full historical/climate fact check, or individualized PPE instruction. Exact IDs/hashes only.


## Rows291–300 — DONE: listed-taxonomy review and editorial repairs

All ten full-read records labelled clean under the existing reviewer-approved listed-taxonomy policy; no supported disease code was established. Seven source-specific texts repaired separately. Clean does not deny original editorial errors, certify inaccessible sources, or imply gold-test success. Total labels:263 unique documents.

Repairs:291 total-area ranking and article/comment distinction;292 machinery safeguarding/noise-control qualifications;294 confidentiality limits and outside-perspective wording;295 narrow art-history claim;296 crop-specific conditions and duplicated wording;297 remove unidentified solution and unverified seed-treatment recipes rather than guess missing chemicals, clarify soil and local sowing guidance;299 attribute manufacturer claims and remove undated emissions percentage.293,298,300 unchanged with explicit source limits in the audit. Do not infer F1 from generic names, C4 from related headed crop sections, D4 from unproven translation origin, or C3 from a complete sentence with an unidentified ingredient.

```python
import hashlib

def repair_291_300(doc_id, text):
    rules = {
        '5453a6810e86c467': ('5453a6810e86c467dc1825d2a2b8e0b82ad85bc118db154a16b4545f6271d06d', [('Canada, the second-largest country by land area', 'Canada, the second-largest country by total area'), ('Despite its reputation for good weather, Sochi can experience dark and dreary days, as seen by visitors from Seattle during a cruise ship stop. However, as shared by a user, Sochi can also be quite pleasant, with warm temperatures and beautiful beaches, making it an attractive holiday destination. This varied weather pattern is typical of coastal cities with a humid subtropical climate, where summers are hot and winters are mild, but occasional storms can bring temporary gloom.', 'The original travel post describes Sochi as a mild coastal resort and discusses its selection for the 2014 Winter Olympics. Reader comments describe different experiences of the weather. These anecdotes are individual observations, not climate measurements.')]),
        'a99df847e362d6be': ('a99df847e362d6be795b6d4770c11646e59d37d6d4a46e2abb7bcfb71623c62c', [('To mitigate this risk, operators should use earplugs or industrial-grade hearing protection, and machines should be equipped with sound barriers whenever possible.', 'Noise exposure should be assessed and reduced through appropriate engineering and administrative controls. Suitable hearing protection and a hearing-conservation programme may also be required; earplugs alone are not a substitute for controlling the noise hazard.'), ('While machinery has made sheet metal bending easier and more efficient, it is crucial for operators to be aware of these potential hazards and take appropriate safety precautions to ensure a safe working environment.', 'This is an overview, not a complete operating procedure. Point-of-operation hazards require suitable safeguarding and trained operators. Servicing and maintenance require the applicable energy-isolation procedures. Follow the machine instructions and applicable workplace safety requirements.')]),
        'fe66f059184ad7bb': ('fe66f059184ad7bb5ac8dbdbc78e6314e3778f144521781cbbbd182ba9a303f3', [('It provides a safe, non-judgmental, and confidential space where they can openly discuss their feelings and challenges.', 'Counselling aims to provide a supportive space to discuss feelings and challenges. Confidentiality has limits, including safeguarding and serious-risk situations; the practitioner should explain the applicable policy at the outset.'), ('can offer an unbiased perspective, helping them identify and overcome obstacles', 'can offer an outside perspective and help them explore obstacles')]),
        '1590536a76ea433c': ('1590536a76ea433c85539a795c7ebf37917beaee4819640c2a301e7a8959ef4d', [('began his artistic journey in the 1950s by creating unique sculptures from crushed and twisted automobile wrecks', 'became known in the late 1950s for sculptures incorporating crushed and twisted automobile parts'), ("Chamberlain's work marked a significant turning point in the history of modern art. Before him, industrial materials were largely overlooked or considered inappropriate for artistic expression. However, Chamberlain's sculptures not only made these materials acceptable but also desirable as artistic mediums. This shift in perspective continues to influence contemporary artists and designers, who continue to explore and incorporate industrial materials into their work.", 'Chamberlain developed an influential sculptural practice using industrial materials, especially discarded automobile parts. Museum accounts situate his work in relation to Abstract Expressionism and earlier welded-steel sculpture; they do not establish him as the first artist to use industrial materials.')]),
        '3c68bf650835bd5a': ('3c68bf650835bd5a6c02ff5df527f6e20ab5b144e7b1ccdb12455d33a5bd15f7', [('However, there are exceptions:\n\n- Fine dusty seeds like celery, strawberries, petunias, eustoma, and lobelia require gradual acclimatization. Instead of immediate removal, open the container lid briefly (2-3 times a day for a few minutes) to allow the small shoots to get used to the room temperature and humidity. Once the first true leaves appear, keep the container lid slightly open at all times, gradually increasing the duration of ventilation each day.\n\n- If the container is covered with plastic wrap, create small punctures in the film using a toothpick to allow air circulation. Increase the number of holes daily to help the seedlings adapt to the environment.\n\n', 'Follow species-specific germination guidance rather than a universal schedule for keeping seedlings covered.\n\n'), ('Immediately after germination, lower the temperature around the seedlings to 18-20°C (64-68°F) to promote healthy growth. This can be achieved by:', 'After germination, use the temperature recommended for the crop and avoid excessive heat. A single temperature range is not appropriate for every species. Possible adjustments, where suitable for the crop, include:'), ('- Use LED bicolor lamps or other suitable devices to provide 12 hours of light daily.\n- Position the light source 30 cm (12 inches) away from the tops of the seedlings. Adjust the height as needed.', "- Use suitable fluorescent or LED lighting. Required duration and intensity depend on the crop and growing conditions.\n- Follow the light manufacturer's guidance on distance and adjust as plants grow; 30 cm is not a universal setting."), ('root and root rot', 'root rot'), ('Water seedlings only with settled water at a temperature of 20-25°C (68-77°F).', 'Use water suitable for the crop and avoid temperature extremes; settling water and using exactly 20-25°C are not universal requirements.'), ('ensuring a successful transplant and a bountiful harvest', 'supporting their development before transplanting')]),
        '3d938ecc8bc299c4': ('3d938ecc8bc299c4cbfff74773d309e577be9336fec08a049fd30ed78559a340', [(' and weakened immunity', ''), ('Alternatively, opt for early-maturing broccoli varieties and plant them in early to mid-March, taking advantage of the naturally lengthening daylight hours.', 'Select varieties and sowing dates for the local climate and intended harvest season; early to mid-March is not a universal planting window.'), ('Broccoli thrives in well-lit areas, preferably where cereals, legumes, cucumbers, or potatoes have previously grown. Loamy soil, which retains moisture, is most suitable. To prevent kale attacks, ensure the soil is not acidic. Add dolomite flour or wood ash during garden preparation to normalize acidity.\n\nDig pits for broccoli planting 45-50 cm apart and at least 20 cm deep to allow for future soil additions. Broccoli loves fertile soil; enrich each pit with a handful of vermicompost or compost, some wood ash, and mix well with the soil.\n\n', 'Broccoli grows best in fertile, well-drained soil that retains moisture. University of Minnesota Extension recommends pH 6 to 7 and soil testing to guide amendments. Avoid treating unexplained "kale attacks" as a diagnosis or adding lime or ash without an appropriate soil recommendation.\n\n'), ('While it may seem that home conditions are ideal for seedlings, they are not. This increases the risk of diseases like black leg and pest infestations. Preventive measures and timely intervention are crucial:\n\n- Preventive Measures: Use a 0.01% solution (0.1g per liter of water) to spray seedlings before and two weeks after planting to prevent fungal diseases.\n- Treatment: If signs of disease appear, spray the plants and soil with the solution 6-12 hours after picking.\n\n', 'Identify the disease or pest before choosing treatment. The original text gives a 0.01% solution without identifying the substance, so that recipe and its spray timings have been removed. Any pesticide treatment must be suitable for the crop, target problem and location and follow the approved product label.\n\n'), ('Pepper seeds are sold coated or uncoated. Coated seeds (pelleted) do not require preparation, as they contain growth stimulants and trace elements. Other seeds need pre-sowing preparation:\n\n1. Sorting: Remove dark, damaged, or empty seeds.\n2. Salt Solution: Soak seeds in a 3% salt solution, remove floating seeds, wash, and dry the heavier ones.\n3. Fungicide Treatment: Treat seeds with a fungicide or soak in a 1% potassium permanganate solution for 20 minutes to reduce fungal risks.\n4. Growth Stimulant: Soak seeds in a growth stimulant (like Epin or Zircon) or aloe juice solution for 4-20 hours to enhance germination.\n\n', "Follow the seed supplier's preparation and sowing instructions. This review could not verify the original salt, potassium permanganate, commercial stimulant or aloe-soaking recipes, so those prescriptions have been removed rather than reconstructed.\n\n"), ("After germination, transfer seedlings to a cool, lighted area with a temperature of 17-18°C. This'stress' hardens the plant and promotes better root development. Maintain this temperature for 7-10 days to prevent seedlings from stretching and becoming thin and brittle.", 'Use crop-specific temperature guidance after germination. A cool indoor holding period is not a substitute for gradually acclimatizing seedlings to outdoor conditions before transplanting.')]),
        '41edaf529c7ea5de': ('41edaf529c7ea5de6548dee16ac71878ca45278411333c119de6558b86bd6dc3', [('Buildings contribute nearly 40% of these emissions, making them a crucial factor in combating climate change.', 'Buildings contribute to emissions through construction, energy use and other life-cycle stages. Any percentage requires a specified year, emissions measure and system boundary; the undated nearly-40% figure is omitted here.'), ('FEVE (fluoroethylene vinyl ether) resins offer a sustainable solution for reducing the environmental impact of structures throughout their life cycle. They provide cost reductions in various stages, including coating production, transportation, waste reduction, traffic, and volatile organic compounds (VOCs) emitted during repainting or recoating.', 'The manufacturer promotes FEVE (fluoroethylene vinyl ether) resins for durable coatings and potential maintenance and life-cycle cost benefits. Environmental and cost comparisons depend on the coating system, application, service life and assessment boundaries; they are not universal guarantees.'), ('Utilizing FEVE resins over PVDF offers numerous benefits, including aesthetic improvements, easier application, and enhanced sustainability.', 'The manufacturer discusses FEVE and PVDF options in terms of finish, application and performance. Choice of a coating system requires project-specific specifications; this review does not independently establish environmental superiority.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: seven changed source records, three unchanged batch records, all other493 source records unchanged; exact ID/hash guards, altered-source negative controls, and idempotence pass. Removed chemical recipe assertions absent. CSV exact header,263 unique IDs, single-line notes and verdict/code agreement pass. Original source pack unchanged; before/after hashes and full comparisons retained in local audit. No cleaned full-pack export produced.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial deletion removes unverified details and replaces some prescriptions with qualified guidance. These are bounded editorial changes, not reconstructions of inaccessible sources, comprehensive safety instructions, or independent clinical/product validation. Apply only the reviewed IDs with matching original hashes.


## Rows301–310 — nine DONE,308 pending

305,307,310 fix/C1: remove app-download CTA, unrelated sample-course material and contractor advertisement respectively. Evidence quotes point to original text offsets.301,302,303,304,306,309 clean under existing reviewer-approved listed-taxonomy treatment; editorial errors and source gaps recorded separately.308 unlabelled and pending: source list missing but no confirmed sentence truncation; no guessed eight-item replacement. Total272 unique labels.

Seven repaired texts:301 distinguish ethical stance and empirical sexuality-education claim;302 synchronization IDs versus CSRF token;303 remove false Indonesian six-volcano ranking;304 qualify compounding and investment guarantees, remove unverified stock examples;305 remove CTA;307 remove template outcomes/biography/endorsement;310 remove contractor ad and qualify terminology.306,308,309 unchanged. Exact source status, primary references and before/after text are retained in local audit. C1 rule307 is specific to a demonstrably unrelated sample-course block; do not generalize removal of genuine instructor biographies.

```python
import hashlib

def repair_301_310(doc_id, text):
    rules = {
        '32565fe6325d9bbd': ('32565fe6325d9bbda619b7340d9c69062052e589a4818d153da3b1cc1236e5d6', [("Premarital Abstinence: On medical grounds alone, truly premarital sexual abstinence is considered the most ethical choice to respect both oneself and one's partner.", 'Premarital Abstinence: The supplied text advocates waiting until marriage as an ethical position. Medical evidence alone does not establish a uniquely ethical marital status for sexual activity.'), ('Chastity Education: Chastity education has proven successful where comprehensive sexuality education has failed, as it promotes self-control and the application of core ethical values.', 'Evidence on Education: WHO supports age-appropriate comprehensive sexuality education and reports that abstinence-only programmes have not demonstrated the claimed superiority. Ethical advocacy and evidence about health outcomes should be distinguished.')]),
        '1783d679ed057b53': ('1783d679ed057b53e957c53cbd24572202250bd0d947f293d940feebb04b0097', [('syncId:\n- Used in the CSRF token placed in HTTP requests.\n- Acts as a typical identifier for each request.\n- Ensures that no requests are dropped between other requests.\n\nclientId:\n- Also used in the CSRF token.\n- Increases only from client-side interactions.\n- Unlike syncId, its role in preventing dropped requests is not explicitly stated.', 'Vaadin Synchronization Identifiers\n\nsyncId:\n- A server-to-client synchronization identifier, incremented with server responses.\n- The client includes its last received value in subsequent requests to track communication state.\n\nclientId:\n- A client-to-server message identifier used to track request ordering and expected messages.\n\nThese are synchronization identifiers, not the CSRF token. Vaadin carries a separate security token for CSRF protection. Exact retry and re-synchronization behaviour depends on the framework version; this summary does not establish that all requests are guaranteed to be delivered.')]),
        'c9f74b49a0d76112': ('c9f74b49a0d76112ad15f509662e1d9984cbac5e78ca39ffed8800a0f4d142f8', [('many earthquakes exceeding 4.5 on the Richter scale between 1976 and 2002', 'a cited map of earthquakes above magnitude 4.5 between 1976 and 2002'), ("- National Geographic's list of the top six life-threatening volcanoes on the Ring of Fire includes:\n- Mount Merapi (last erupted in 2018, 500,000 people within 60 miles)\n- Mount Sinabung (last erupted in 2019, 1.5 million people within 60 miles)\n- Mount Kelud (last erupted in 2014, 1.4 million people within 60 miles)\n- Mount Agung (last erupted in 2018, 1.7 million people within 60 miles)\n- Mount Rinjani (last erupted in 2019, 2.2 million people within 60 miles)\n- Mount Semeru (last erupted in 2021, 1.3 million people within 60 miles)", "- The June2019 source distinguishes its Indonesian-volcano map from National Geographic's six-volcano list; it explicitly says those Indonesian volcanoes are not in that list. The supplied six entries and their eruption/population figures are therefore removed rather than presented as that ranking.")]),
        'cd7dcea5823d3544': ('cd7dcea5823d3544e8d965fd5085d4a4bdde433736a7ca05aff6ef159714a9a5', [('Power of Daily 1% Improvement: Compounded daily, 1% improvement results in a 37.78x increase over a year, compared to a 4.65x increase on a simple interest basis.', 'Mathematical Illustration: A hypothetical1% increase on each of365 days gives1.01**365, about37.78 times the starting amount. Adding1% of the starting amount each day gives4.65 times the starting amount. This arithmetic is not an achievable-return forecast.'), ("Titan Stock Example:\n- Buying at the peak in June 2017 and holding until June 2019: 151% return.\n- Buying after the market crash in 2020 and holding until March 2021: 78% return.\n- Buying in April 2021 and holding until January 2022: 80% return.\n- Compounded gains over three years: 392% (a nearly 5x gain, qualifying it as a'multibagger').\n", 'Stock Example: The supplied Titan price-return examples were not verified and their periods cannot simply be treated as three consecutive years. They are omitted rather than used to substantiate a compounded return.\n'), ('Tax Implications: Long-term investors save on taxes each year, effectively receiving interest-free loans from the government by not selling stocks.\nLong-term Investing Benefits:\n- Accumulation of compounded wealth over decades.\n- Consistent receipt of mouth-watering dividends.\n- Reinvestment of taxes on notional gains at zero cost.\nBest Way to Allocate Capital: Long-term investing, as it maximizes gains and minimizes taxes.', 'Risk and Tax Context: Long-term holding can be a strategy, but it does not guarantee gains or dividends. Tax consequences depend on jurisdiction, account and transaction details; not selling is not a universal tax-free government loan. Capital allocation depends on goals, time horizon, risk tolerance and diversification.')]),
        'a4732de1e364228c': ('a4732de1e364228cadf0a53732154345f7e23eb7fb6909468e680f79cf380dcd', [(' It can be downloaded from app stores or accessed at bemc.org.', '')]),
        '373e0f0dd5699ac0': ('373e0f0dd5699ac072b4134d16f7f9d9360e35b72ec4168ae10729ea36c06e69', [("Learning Outcomes:\n- Understanding how classical artists use space in their work\n- Learning to draw inspiration from one's environment and cultural context\n- Exploring art as a means of expression, exploration, and questioning\n- Recognizing art's response to social, cultural, and political issues\n- Understanding the use of materials in art\nInstructor:\n- Name: Penelope Goddard\n- Background: Painter, instructor, writer\n- Education: Attended Johnson School of Art at age 10, studied classical art, accepted to St. Markus Institute of Art on a full scholarship at age 18\n- Exhibitions: Art series exhibited in the US and Europe\nPlatform: LearnWorlds (recommended by a senior product manager, Walter White)", '')]),
        'f57e3be39b6f12f7': ('f57e3be39b6f12f764c64af73809c56bbb941f50f9101cddeab5f7734d78f44c', [('Recommended Contractor: Ocmulgee Concrete Services is recommended for constructing sub walls due to their understanding of the necessary construction techniques.\n', ''), ('Definition of Sub Wall:', 'Contractor-specific Description of Sub Wall:'), ('Purpose of Sub Walls: Sub walls are used to bring foundations up to the same level as the rest of the house, ensuring a uniform grade, especially on sloping lots or when dealing with soft soils.', "Scope: This summarizes the contractor's terminology, not a general engineering prescription for soft soils. Foundation design requires site-specific assessment.")]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: seven original records changed, three batch records unchanged, other493 source records unaffected; exact ID/hash guard, changed-input negative control and idempotence pass. Compound arithmetic1.01**365 rounds37.78 and1+.01*365 rounds4.65; these are hypothetical multiples, not investment forecasts. CSV exact header,272 unique IDs, single-line notes, evidence length and verdict/code agreement pass. Original pack preserved; no whole-pack cleaned export generated.

Harm tests: pending Data A / Data B; no gold set supplied. Editorial corrections are not restorations of unavailable originals or full factual/tax/security reviews. Source-specific removal can omit unverified detail; apply only exact reviewed IDs/hashes.


## Row308 — superseding resolution: DONE/clean

DONE: clean under listed taxonomy. Removed empty types heading and missing-content placeholder as an editorial repair. Original article unavailable on recheck; previously indexed same-site tag excerpt corroborates title/opening only. No eight-item list reconstructed. Original ends in a complete sentence, so C3 is unsupported by the strict repo definition. Clean does not certify original completeness or medical claims; it records no confirmed listed disease. This decision supersedes the earlier pending308 entry.

Source rechecked: [article](https://en.spress.net/8-types-of-fruits-and-vegetables-dispel-summer-thirst/); [same-site tag index](https://en.spress.net/tag/muggy/). Both unavailable on latest fetch; earlier indexed excerpt did not provide the promised list. Preserve the two introductory statements and remove only the empty section. Do not invent a disease code or infer body truncation from an explicit synthetic missing-content placeholder alone. This is a source-specific editorial rule, not a general removal of informative statements about missing data.

```python
import hashlib

def repair_308(doc_id, text):
    if doc_id != '6a2017ab99292d5b' or hashlib.sha256(text.encode("utf-8")).hexdigest() != '6a2017ab99292d5bcd8b7dcd52139e421482bc9ebc04afe2eb286ec0fb1cf483':
        return text
    suffix = '\nTypes of Fruits and Vegetables:\n- Not specified in the given text.'
    if not text.endswith(suffix):
        raise ValueError("Unexpected reviewed source text")
    return text[:-len(suffix)]
```

Validation: exactly one source text changed;499 others unaffected. Exact ID/hash guard, changed-input and unrelated-ID negative controls, idempotence, CSV header/273 unique IDs and verdict/code agreement pass. Original pack unchanged; no whole-pack cleaning performed. Harm/gold tests pending Data A / Data B, not run. Retained introductory health statements remain source-derived rather than independently medically validated.


## Rows311–320 — all ten DONE;314 drop/F3

314 drop/F3 for an explicit sexual-violence catalogue. Full original read; adult site not fetched because supplied text suffices for this diagnosis. The rule returns None as an explicit drop sentinel. Callers must exclude that document; do not stringify None or retain an empty replacement as a cleaned article.

Other nine full/clean under existing reviewer-approved listed-taxonomy policy, with separate editorial repairs:311 reactor chemistry and unsupported explosive claims;312 historical frame;313 Calculator mode/history qualifications;315 remove unverified1970 date/time without guessing replacement;316 wages/benefits/overtime and policy generalizations;317 benefit/pay qualifications and central-tendency claim;318 historical IAEA mission frame and rhetoric;319 individualized dental choices/duration;320 genetic-engineering distinction, qualified evidence/claims and unverified statistics removal. Clean does not assert originals error-free or unavailable sources verified. Total283 unique labels.

```python
import hashlib

def repair_311_320(doc_id, text):
    # None means drop this document; callers must remove it from retained output.
    if doc_id == '06cfe5719541e863' and hashlib.sha256(text.encode("utf-8")).hexdigest() == '06cfe5719541e863bed260475ed925693ed9a8f4ece5161b3c5b58c8ad3ffaf8':
        return None
    rules = {
        '48c7db777194752d': ('48c7db777194752d981ef484358575014fba987b137c79313f388f5c4105972b', [("Causes of Fukushima Explosion: Hydrogen build-up caused by a loss-of-coolant accident.\nHydrogen Explosion Precedent: Similar hydrogen explosion occurred at the Three Mile Island plant in 1979.\nZirconium's Role: Zirconium, used to clad fuel rods, reacts with heat and releases hydrogen, which can explode.\nZirconium's Volatility: Zirconium is highly volatile and can explode spontaneously upon contact with air, water, or steam.\nZirconium's Explosive Power: Pound for pound, zirconium has the explosive power of nitroglycerine and can ignite at 2,000°F (1,093°C).\nZirconium in Nuclear Plants: Around 30,000 to 40,000 fuel rods, composed of twenty tons of zirconium, are used in an average nuclear power plant.\nZirconium's Other Use: Zirconium is also used in flashbulbs for photography.\nZirconium's Reaction: In a loss-of-coolant accident, zirconium draws oxygen from water and steam, releasing hydrogen gas.\nPotential Meltdown: A hydrogen explosion can generate additional heat, bringing zirconium closer to its explosive level and potentially triggering a meltdown.\nSpent Fuel Pools: There are large numbers of additional fuel rods in spent fuel pools next to nuclear power plants. A 'loss-of-water' accident in these pools could lead to zirconium cladding explosions, releasing nuclear poisons into the environment.", 'Hydrogen and Fuel Cladding in Severe Reactor Accidents\n\nWhen cooling is lost and zirconium-alloy fuel cladding overheats, reaction with steam can generate hydrogen and additional heat. Hydrogen accumulating with air can burn or explode. This chemical reaction contributed to the hydrogen explosions at Fukushima Daiichi.\n\nThis is not a claim that bulk zirconium cladding spontaneously explodes on ordinary contact with air or water. The supplied nitroglycerine comparison, universal ignition temperature and average-plant inventory figures were not verified and are omitted.\n\nSpent Fuel Pools\n\nLoss of adequate cooling or water coverage can create serious fuel-damage risks. Assessments consider heating, cladding oxidation and possible hydrogen generation; these are distinct from treating the zirconium inventory as a high explosive. The outcome depends on the specific fuel, cooling and accident conditions.')]),
        '4571e1192051211e': ('4571e1192051211e50bcc05311016f7b807769db312aa83c0d7ce32cc515b079', [("Events:\n- Art Talk Live: October 28, 5:30pm, featuring NMWA Educator Ashley W. Harris and Gadsden Arts Curator Angie Barry, discussing women artists in 'Women Artists: Four Centuries of Creativity' and NMWA collection.\n- Florida Artists Happy Hour: November 9, 5:30pm, featuring Florida women artists from Gadsden Arts Permanent Collection, free for GACM and NMWA members.\n- Presentation on Marjorie Harris Carr and Beth Appleton: November 18, 1pm, featuring Dr. Peggy Macdonald, presented by Florida Humanities.\n\nArtists Discussed/Highlighted:\n- Rosa Bonheur, Käthe Kollwitz, Élisabeth Louise Vigée-LeBrun, Françoise Gilot, Joan Mitchell, Adrian Piper, and others in Art Talk Live.\n- Beth Appleton, Alyne Harris, Marisa Tellería-Díez, Mildred Thompson, Ruby C. Williams in Florida Artists Happy Hour.\n- Marjorie Harris Carr and Beth Appleton in the final presentation.\n\nTopics Covered:\n- Barriers faced by women artists throughout history and their fight for recognition.\n- Life and legacy of Marjorie Harris Carr, a prominent environmental activist.\n- Environmental concerns addressed through Beth Appleton's art.\n\nRegistration and Admission:\n- Registration required for all three virtual programs.\n- Free admission for members and children under 17, $5 for non-member adults, $3 for adult students at Gadsden Arts.\n\nLocation and Hours:\n- Gadsden Arts located in Quincy, 20 miles northwest of Tallahassee.\n- Open Tuesday through Saturday, 10am-5pm (exhibitions, shop), Saturdays 10am-5pm (ArtZone drop-in studio).", "Historical programme summary associated with an October2021 article; dates, fees and hours below are not current visitor information.\n\nEvents:\n- Art Talk Live: October 28, 5:30pm, featuring NMWA Educator Ashley W. Harris and Gadsden Arts Curator Angie Barry, discussing women artists in 'Women Artists: Four Centuries of Creativity' and NMWA collection.\n- Florida Artists Happy Hour: November 9, 5:30pm, featuring Florida women artists from Gadsden Arts Permanent Collection, free for GACM and NMWA members.\n- Presentation on Marjorie Harris Carr and Beth Appleton: November 18, 1pm, featuring Dr. Peggy Macdonald, presented by Florida Humanities.\n\nArtists Discussed/Highlighted:\n- Rosa Bonheur, Käthe Kollwitz, Élisabeth Louise Vigée-LeBrun, Françoise Gilot, Joan Mitchell, Adrian Piper, and others in Art Talk Live.\n- Beth Appleton, Alyne Harris, Marisa Tellería-Díez, Mildred Thompson, Ruby C. Williams in Florida Artists Happy Hour.\n- Marjorie Harris Carr and Beth Appleton in the final presentation.\n\nTopics Covered:\n- Barriers faced by women artists throughout history and their fight for recognition.\n- Life and legacy of Marjorie Harris Carr, a prominent environmental activist.\n- Environmental concerns addressed through Beth Appleton's art.\n\nRegistration and Admission:\n- Registration required for all three virtual programs.\n- Free admission for members and children under 17, $5 for non-member adults, $3 for adult students at Gadsden Arts.\n\nLocation and Hours:\n- Gadsden Arts located in Quincy, 20 miles northwest of Tallahassee.\n- Open Tuesday through Saturday, 10am-5pm (exhibitions, shop), Saturdays 10am-5pm (ArtZone drop-in studio).")]),
        '572805b8a9e4839b': ('572805b8a9e4839b5dc1297678a39fe9859983c1293e22c1b491609c5160990b', [('Windows 10 Calculator Modes: The calculator app has multiple modes including Standard, Scientific, and Programmer mode.\n- Standard Mode: Performs basic algebraic operations like addition, multiplication, etc.\n- Scientific Mode: Allows calculations of logarithms, scientific formulas, and conversions between different units of measurement.\n- Programmer Mode: Enables digital logic operations (AND, OR, XOR, etc.) and represents numbers in binary, octal, or hexadecimal.\n\nCalculator History Feature:\n- Purpose: Keeps track of previous calculations for review, correction, or reference purposes, such as for accounting reports or solving complex problems.\n- Accessing History:\n1. Open the Calculator app from the Start menu.\n2. Perform calculations in the desired mode (Standard, Scientific, or Programmer).\n3. Click on the clock icon next to the Calculator mode title to view the history.\n- Navigation and Actions:\n- Use the arrow buttons to move up and down in the history.\n- Right-click to copy and paste figures from the history.\n- Click the Delete icon at the bottom to clear all previous operations.', 'Windows Calculator Modes and History\n\nStandard mode handles basic arithmetic, Scientific mode supports advanced calculations, and Programmer mode provides base conversions and bitwise operations. Unit conversions are available through separate converter options.\n\nOpen Calculator from Start and choose the appropriate mode. Where History is available, use it to review previous calculations. The controls, layout and supported actions vary with the app version and window size.\n\nThe supplied instructions did not establish history support in Programmer mode or verify the exact clock position, arrow navigation and right-click paste behaviour. Those universal steps have been removed rather than presented as tested Windows10 instructions. Use the controls shown by the installed app to manage its history.')]),
        '57922931d471745c': ('57922931d471745cf87e605644da97dceb9bb309fde48de7e4c4f840cb4b2fc5', [('- Date: Saturday, January 17, 1970\n- Time: 12:00 AM CET', '- Date and time: Unverified. The supplied1970 timestamp could not be corroborated and is omitted; no replacement date is inferred.')]),
        'ac42e3ccd8fb70ea': ('ac42e3ccd8fb70ea8a1d74fc381d37fcee1ce45136777f30c5fcc9a471e41520', [('- Wage employment: Work provided by an employer, wage calculated per day, controlled work environment.', '- Wage employment: Work for an employer in exchange for agreed pay. Pay rates and payment intervals vary; wages are not universally calculated per day.'), ('- Advantages: Reduces tax burden, helps with poverty reduction, provides social stability.', '- Possible aims: Raise earnings for low-paid workers and reduce poverty. A higher minimum wage does not automatically reduce the tax burden.'), ('- Disadvantages: Can disrupt economic system, may hurt the poor, can lead to unemployment.', '- Possible trade-offs: Employment, hours and business costs may be affected; the direction and size of effects depend on the setting and policy.'), ('- Advantages: Job security, benefits (holiday entitlements, pension, sick pay, parental leave, life insurance), flexibility for personal commitments.\n- Disadvantages: No overtime pay, longer work hours expected.', '- Possible advantages: Regular pay and benefits, depending on law, eligibility and contract; job security and flexibility are not guaranteed.\n- Hours and overtime: Entitlement depends on the jurisdiction and employment status. For example, covered non-exempt US employees have statutory overtime rights; lack of overtime pay is not a defining feature of wage employment.'), ('- Social stability: Workers remain in jobs, employers avoid high turnover.', '- Possible social benefit: Higher pay may support retention; lower turnover is not guaranteed.'), ('- Higher labor costs for companies, loss of competitiveness, potential unemployment.', '- Potential costs: Higher wage bills may affect prices, staffing or competitiveness; outcomes are not uniform across employers and labour markets.')]),
        'acaa21c09564a4bf': ('acaa21c09564a4bf59af434bbb12c8b35fd6fc566626e8dc65258eaf4e0be311', [('Employee Advantages:', 'Possible Employee Advantages (subject to law, eligibility and contract):'), ('- Guaranteed Regular Income (Paycheck)', '- Agreed Pay for Work Performed'), ('- Fixed Work Hours', '- Agreed Work Schedule, Which May Vary'), ('- Regular Payment Interval (Daily or Weekly)', '- Agreed Payment Interval, Which May Be Daily, Weekly, Monthly or Otherwise'), ('- Mean is the most commonly used and generally considered the best measure of central tendency.', '- Mean, median and mode summarize different features of a distribution. The mean is sensitive to extreme values; no one measure is universally best.')]),
        'bcd63a567d168dd2': ('bcd63a567d168dd253435d62fbd1a4ebe72cb8804cf1fed617604fee5a207dfd', [("Location: Zaporizhzhia nuclear power plant in Ukraine, one of the world's largest nuclear facilities.\nConflict: Ongoing war between Ukraine (Kyiv) and Russia (Moscow).\nIssue: Attacks on the Zaporizhzhia nuclear power plant, with both sides blaming each other.\nConcerns:\n- United Nations (UN) chief Antonio Guterres warned that any attack on a nuclear plant is suicidal.\n- Employees at the plant face a dilemma: stay and work under constant bombardment or flee, risking a nuclear catastrophe.\n- Escalating attacks on the nearby Ukrainian city of Enerhodar have sparked global panic.\n- Ukraine accuses Russia of shelling the city from the plant grounds, while Ukrainian soldiers claim they avoid targeting the plant.\nProposed Solutions:\n- Both Ukraine and Russia have agreed to allow International Atomic Energy Agency (IAEA) inspectors to visit the plant.\n- IAEA chief Rafael Grossi is ready to lead a mission to the plant.\n- Ukrainian authorities have performed disaster response drills.", "Historical context: reporting before the IAEA mission announced on29August2022; not a current plant-status report. The original article date has not been recovered.\n\nLocation: Zaporizhzhia nuclear power plant in Ukraine, one of the world's largest nuclear facilities.\nConflict: Ongoing war between Ukraine (Kyiv) and Russia (Moscow).\nIssue: Attacks on the Zaporizhzhia nuclear power plant, with both sides blaming each other.\nConcerns:\n- United Nations (UN) chief Antonio Guterres warned that any attack on a nuclear plant is suicidal.\n- Employees at the plant face a dilemma: stay and work under constant bombardment or flee, risking a nuclear catastrophe.\n- Escalating attacks on the nearby Ukrainian city of Enerhodar have sparked global panic.\n- Ukraine accuses Russia of shelling the city from the plant grounds, while Ukrainian soldiers claim they avoid targeting the plant.\nProposed Solutions:\n- Both Ukraine and Russia have agreed to allow International Atomic Energy Agency (IAEA) inspectors to visit the plant.\n- IAEA chief Rafael Grossi is ready to lead a mission to the plant.\n- Ukrainian authorities have performed disaster response drills."), ('have sparked global panic', 'raised international concern, according to the supplied report')]),
        '24b789949cbe0ad6': ('24b789949cbe0ad6ac6148ea00b033845cb0a5aec184dc64c3386944042d9f5b', [('and usually both teeth on either side need to be restored for symmetry', 'with the choice and extent of restoration determined by individual assessment rather than a universal requirement to restore both teeth'), ('- Usually results in a shorter treatment time compared to other methods.', '- Treatment duration depends on the cause, periodontal health and chosen procedure; no generally shorter treatment time is established here.')]),
        '0cf707e49b1fddfd': ('0cf707e49b1fddfd738c5fe0d6208e2ad9ef63863742fb21a3690d6c95c91cef', [('- Donated $15 million to global campaigns promoting GMO use.\n- Hired 400 "science ambassadors" to influence agricultural policy in 35 countries.', '- The supplied funding and ambassador figures were not verified against a dated primary record and are omitted.'), ('- Most scientists, including those from the National Academy of Sciences, AAAS, and European Commission, declare genetically modified foods safe.\n- A large 2013 study found no significant hazards connected with the use of genetically engineered crops.', "- The National Academies'2016 review found no evidence implicating greater human-health risk from then-commercialized genetically engineered foods than from non-GE counterparts. This is not a guarantee about every future product or every agricultural practice. The unidentified2013 study claim is omitted."), ('- Most foods consumed today have been genetically modified in some way.\n- Agricultural crops have been selectively bred for thousands of years.\n- Many products, including insulin and common crops like soybeans and corn, are made with GMO ingredients.', '- Selective breeding and modern genetic engineering are distinct approaches. In common regulatory usage, GMO does not simply mean any organism changed by traditional breeding.\n- Genetic engineering is used in some crop varieties and in pharmaceutical production; medicine production should not be described as evidence that most foods contain GMO ingredients.'), ('- Over 43,000 products have been submitted to the Non-GMO Project.\n- Annual sales of non-GMO products total over $22 billion.', '- The supplied product-count and annual-sales figures lack a verified reference year and are omitted.'), ('- Monocropping with GMOs can ruin soil, cause bees and other pollinators to leave, and lead to barren lands.\n- Farmers have committed suicide due to failed GMO crops.', '- The supplied criticism alleges environmental and social harms. These claims are not established as general consequences of genetic engineering: effects depend on crop traits, farming practices and local circumstances. No simple causal claim about farmer suicide is substantiated in this review.'), ("Mark Zuckerberg's Criticism:", 'Additional Criticism Attributed to Vandana Shiva:')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: one exact-ID/hash drop, nine editorial repairs, other490 source documents unchanged; altered-input and unrelated-ID negative controls pass; nine retained texts idempotent. CSV exact header,283 unique IDs, single-line notes and verdict/code agreement pass. F3 evidence is a short original quote with offset. Original pack unchanged; no whole-pack cleaned export generated. Before/after text and hashes or drop action recorded in audit.

Harm tests: pending Data A / Data B; gold sets not supplied. Source gaps recorded per row. Primary references support bounded corrections, not restoration of inaccessible articles, software execution or complete medical/legal/scientific fact checks. Exact guards limit applicability to reviewed originals.


## Rows331–340 — DONE: listed-taxonomy review and editorial repairs

All ten full-read records labelled clean under the existing reviewer-approved listed-taxonomy policy; no supported disease code was established. Seven source-specific texts repaired separately. Clean does not deny original editorial errors, certify inaccessible sources, or imply gold-test success. Total labels:293 unique documents.

Repairs:331 qualify unverified AI-speaker attribution;333 entertainment frame;334 residence-name qualification;336 historical interview attribution;338 sleep/mattress guidance;339 medical misinformation and urgent warning-sign distinction;340 Mamluk chronology and unrelated geography.332,335,337 unchanged.321–330 remain preview-only and are not added to labels by this review.

```python
import hashlib

def repair_331_340(doc_id, text):
    rules = {
        '0c03a4c096276336': ('0c03a4c0962763366dae08238c52e4cf62845985db083fc68d32bd614e38bd90', [("AI's Advice:", 'Advice Reported in the Supplied Summary (speaker not verified):'), ("AI's Opinion: The AI has no sympathy", 'Reported Opinion: The speaker has no sympathy')]),
        '4b7a08cb5e8c4ce2': ('4b7a08cb5e8c4ce29b4cfe3fe6c1d3dd4863f9dac33198d18d663cb87091f399', [("Character: Leland Townsend\n- Personality Traits: Manipulative, charming, versatile, quick-witted, analytical, insecure\n- Goals: Seeks power and knowledge, hates being outmaneuvered\n- Background: Bonds with the devil in season one\n\nZodiac Sign: Gemini\n- Dates: May 21 to June 20\n- Traits: Versatile, quick-witted, analytical, good communicators, learn quickly from environment\n- Potential: Can manifest positively or negatively depending on the person\n\nConnection: Leland Townsend is used as an example of a Gemini with dark manifestations of the sign's traits", "Entertainment interpretation of a fictional character, not an evidence-based personality assessment.\n\nCharacter: Leland Townsend\n- Personality Traits: Manipulative, charming, versatile, quick-witted, analytical, insecure\n- Goals: Seeks power and knowledge, hates being outmaneuvered\n- Background: Bonds with the devil in season one\n\nZodiac Sign: Gemini\n- Dates: May 21 to June 20\n- Traits: Versatile, quick-witted, analytical, good communicators, learn quickly from environment\n- Potential: Can manifest positively or negatively depending on the person\n\nConnection: Leland Townsend is used as an example of a Gemini with dark manifestations of the sign's traits")]),
        '6cd1d63b503f318e': ('6cd1d63b503f318e6c48f814a47bb9d7827329fbb962d12955fa6feab7e618c9', [('Reference: The house is referred to as "The Magic Mirror on the wall" residence', 'Source Phrase: The supplied summary uses "The Magic Mirror on the wall"; this has not been verified as the residence\'s formal name.')]),
        'f8138f8110f21cc1': ('f8138f8110f21cc1cd2dac662f16f9f1675cf09768aa9f7003185141f8560a10', [("AI and Quantum Computing Race: The United States and China are in a race for advancements in Artificial Intelligence (AI) and quantum computing.\nMilitary AI: Both countries are focusing on military applications of AI, with China particularly prioritizing it for battlefield advantage and military deterrence.\nChinese Civilian-Military Fusion: China's policy of civilian-military fusion grants automatic access to its government and military from commercial AI developments, providing a significant advantage.\nU.S. Challenges in Military Cooperation: In the U.S., companies and individuals may not want to cooperate with the government or military, hindering progress in military AI applications.\nPotential Battlefield Disparity: Due to its civilian-military fusion policy, China may have smarter, more autonomous weapons and more pervasive AI applications in its weapons systems compared to the U.S.\nImpact on Other Technologies: China's lead in AI could also provide an advantage in developing other technologies.", "Historical interview commentary: Arthur Herman, December2021. The following is an attributed assessment, not a verified current US-China capability comparison.\n\nAI and Quantum Computing Race: The United States and China are in a race for advancements in Artificial Intelligence (AI) and quantum computing.\nMilitary AI: Both countries are focusing on military applications of AI, with China particularly prioritizing it for battlefield advantage and military deterrence.\nChinese Civilian-Military Fusion: China's policy of civilian-military fusion grants automatic access to its government and military from commercial AI developments, providing a significant advantage.\nU.S. Challenges in Military Cooperation: In the U.S., companies and individuals may not want to cooperate with the government or military, hindering progress in military AI applications.\nPotential Battlefield Disparity: Due to its civilian-military fusion policy, China may have smarter, more autonomous weapons and more pervasive AI applications in its weapons systems compared to the U.S.\nImpact on Other Technologies: China's lead in AI could also provide an advantage in developing other technologies."), ("Chinese Civilian-Military Fusion: China's policy of civilian-military fusion grants automatic access to its government and military from commercial AI developments, providing a significant advantage.", 'Chinese Civilian-Military Fusion: Herman argued that the policy gives the government and military access to commercial AI developments and considered this an advantage.'), ('Potential Battlefield Disparity: Due to its civilian-military fusion policy, China may have smarter, more autonomous weapons and more pervasive AI applications in its weapons systems compared to the U.S.', 'Potential Battlefield Disparity: Herman predicted that China could gain an advantage in autonomous weapons and military AI deployment; this summary does not establish that prediction as an observed result.')]),
        '47b782c9ac128aff': ('47b782c9ac128aff3b2a036851392d85f661fcb84e9ab1de898791111dc1168a', [('A bad mattress may worsen or contribute to sciatica symptoms, especially for those with back problems or who sleep on their stomach.\nAn unsupportive mattress can stress the spine, potentially leading to further injury or illness, such as exacerbating scoliosis, lordosis, or kyphosis.\nLying down on a soft mattress may put more pressure on the sciatic nerve, exacerbating pain.\nPoor posture, stress, and inflammation can also contribute to sciatic nerve pain when lying down.\nSleeping on a mattress that is too firm, too soft, or provides insufficient support can cause or aggravate sciatica symptoms like pain, numbness, and tingling.\nA comfortable yet supportive mattress that maintains the natural curvature of the spine is recommended for people with sciatica.\nTo relieve sciatic discomfort while sleeping, consider the following:\n- Avoid soft mattresses.\n- Try a medium to firm mattress.\n- Place plywood under the mattress.\n- Use a body pillow.\n- Practice stretching or yoga.\n- Take a warm bath.\n- Maintain good sleep hygiene.\nLying down can make sciatica worse, causing intense pain and difficulty sleeping.', 'Sciatica and Sleep Comfort\n\nSciatica can cause pain, tingling, numbness or weakness along the affected leg. Pain may disturb sleep. Mattress comfort, pillows, room conditions and other sleep factors may be worth discussing with a clinician.\n\nThe supplied claims that a bad mattress causes spinal deformities or that one firmness level or plywood support treats sciatica were not substantiated and are removed. A mattress change is not a diagnosis or a substitute for assessing persistent symptoms. Comfort and support needs vary between individuals.')]),
        '0797c93bf413915b': ('0797c93bf413915b502e5f3f12915a92c54b346fc750bf2c559f534356691d90', [("Sciatica Symptoms:\n- Sleeping with the light on, especially if it's not a usual habit.\n- Frequent nighttime awakenings (every hour or so) to use the bathroom.\nPotential Causes:\n- Sleeping with the light on: Pain caused by nerve root compression along the spine, which is relieved when the light is turned off (reducing gravitational force).\n- Frequent bathroom trips: Walking through pain to reach the bathroom, which is usually located away from the bedroom.\nImportance: It's crucial to inform a doctor if these behaviors are repeated, as they can help guide a diagnosis.", 'Sciatica Symptoms and Bladder Changes\n\nSciatica commonly affects the buttock and leg, with pain, tingling, numbness or weakness. Sleeping with a light on and frequent bathroom trips are not diagnostic signs of sciatica. Turning off a light does not reduce gravity or relieve nerve compression by that mechanism.\n\nPersistent sleep or urinary changes need appropriate clinical assessment rather than an assumed cause. With sciatica, new difficulty starting urination, inability to urinate, loss of bladder or bowel control, numbness around the genitals or bottom, or severe or worsening symptoms in both legs require urgent emergency assessment. Frequent urination alone is not equivalent to those specific warning signs.')]),
        'bb556c926bbac777': ('bb556c926bbac7770278ce6fb30a3c63ed01ef4391160a3737dc07236f5f7293', [('- Maluks, the first Muslim rulers in India, established the first Muslim Indian Empire.', '- The Mamluk dynasty began rule in Delhi in1206. This was not the beginning of all Muslim contact or rule in the subcontinent; earlier trade and political connections preceded it.'), ('- The Great Wall of China was a barrier to its spread.', '- The Great Wall of China is not an explanation of the spread of Islam on the East African coast; that unrelated geographical claim is removed.'), ('- It resulted in the establishment of the first Muslim Indian Empire under the Maluks.', '- The Delhi Sultanate began with the Mamluk dynasty; trade, political rule and religious conversion were distinct processes with varied regional histories.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: seven changed source records, three unchanged batch records, all other493 source records unchanged; exact ID/hash guards, altered-source negative controls, and idempotence pass. False gravity mechanism absent from applied text. CSV exact header,293 unique IDs, single-line notes and verdict/code agreement pass. Original source pack unchanged; before/after hashes and full comparisons retained in local audit. No cleaned full-pack export produced.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial deletion removes unverified details and replaces some prescriptions with qualified guidance. These are bounded editorial changes, not reconstructions of inaccessible sources, comprehensive safety instructions, or independent clinical/product validation. Apply only the reviewed IDs with matching original hashes.


## Rows341–350 — DONE: listed-taxonomy review and editorial repairs

All ten full-read records DONE:346 fix/C1 for confirmed author-box biography, other nine clean under existing reviewer-approved listed-taxonomy policy. Nine source-specific texts repaired;343 unchanged. Clean does not deny original editorial errors or certify inaccessible sources. Total labels:303 unique documents.

Repairs:341–342 date-relative historical framing;344 qualified nutrition;345 theological interpretation/source dependence;346 author-box removal, attribution and seasonal precision;347 individualized IDEA placement;348 historical royal-use and unsupported inference;349 abbey/palace chronology, historical monarch wording and verified architect qualifications;350 clinical-sample estimate and prevention/diagnosis qualification.321–330 remain preview-only. Source statuses and before/after text are recorded per row in audit. C1 removal must not generalize to biographies that are the actual subject of an article.

```python
import hashlib

def repair_341_350(doc_id, text):
    rules = {
        'cbfd41d519db7b0c': ('cbfd41d519db7b0ca5e4f50f4e2d1b0bd694b30f8003d3aa34165d2cfbd72cd9', [("Magaly Miralles, a teacher of Venezuelan origin, helps English language learners transition to school and American culture in the Rosemount-Apple Valley-Eagan (RAVE) district. She has received two human-rights education awards for her work with diverse children and families.\nMinnesota's student population: Around 24% (around 197,000) of the approximately 823,200 students in Minnesota schools are non-white, according to the state Department of Education.\nTeachers of color in Minnesota: The percentage of teachers of color has never risen above 3.3%, with about 1,560 teachers of color in 2009, as reported by the Minnesota Minority Education Partnership (MMEP).\nPotential benefits of a diverse teaching pool:\n- Cultural relevance: Students may learn better when their teachers share their cultural background.\n- Role models: Students benefit from seeing teachers who look like them and can empathize with their life stories.\n- Broadened perspectives: White students also benefit from seeing teachers of diverse backgrounds, reflecting the broader world they will encounter.\nEducator perspectives:\n- Sheila Wright (Hamline University): Increasing the number of teachers of color adds richness to teaching and learning.\n- Karen Hynick (Minneapolis Community and Technical College): It's important for students to see teachers who look like them and can empathize with their life stories.\n- Art Coulson (RAVE school board member): The workforce should reflect the community being served, and it's important for all students to see diversity.\n- Cindy Gustafson (Roseville Area Schools): Districts are actively seeking well-qualified teachers with diverse life experiences.\nMMEP's stance: Attracting more diverse and effective teachers is key to the academic success of all youth.", "Historical, source-reported education statistics; original publication date not recovered. These figures are not current Minnesota workforce estimates.\n\nMagaly Miralles, a teacher of Venezuelan origin, helps English language learners transition to school and American culture in the Rosemount-Apple Valley-Eagan (RAVE) district. She has received two human-rights education awards for her work with diverse children and families.\nMinnesota's student population: Around 24% (around 197,000) of the approximately 823,200 students in Minnesota schools are non-white, according to the state Department of Education.\nTeachers of color in Minnesota: The percentage of teachers of color has never risen above 3.3%, with about 1,560 teachers of color in 2009, as reported by the Minnesota Minority Education Partnership (MMEP).\nPotential benefits of a diverse teaching pool:\n- Cultural relevance: Students may learn better when their teachers share their cultural background.\n- Role models: Students benefit from seeing teachers who look like them and can empathize with their life stories.\n- Broadened perspectives: White students also benefit from seeing teachers of diverse backgrounds, reflecting the broader world they will encounter.\nEducator perspectives:\n- Sheila Wright (Hamline University): Increasing the number of teachers of color adds richness to teaching and learning.\n- Karen Hynick (Minneapolis Community and Technical College): It's important for students to see teachers who look like them and can empathize with their life stories.\n- Art Coulson (RAVE school board member): The workforce should reflect the community being served, and it's important for all students to see diversity.\n- Cindy Gustafson (Roseville Area Schools): Districts are actively seeking well-qualified teachers with diverse life experiences.\nMMEP's stance: Attracting more diverse and effective teachers is key to the academic success of all youth.")]),
        'cb62dd92c74e2a8e': ('cb62dd92c74e2a8e7e9252dbdf3f4972edf5baa27411aa9943bae86019dbe67c', [("Roseville Schools:\n- Percentage of students of color: 25% in 2005, 37% currently\n- Students eligible for free or reduced-fee lunch: over one-third\n- Teachers of color: 41, representing 9% of the teaching staff\n- Multi-lingual teacher hires: 5 in the last year\n- Diversity initiatives: yearly all-staff workshops, equity in integration specialists in each school\n\nRAVE District:\n- Teachers of color: less than 2% of the teaching staff\n- Diversity coordinator: Scott Thomas, who focuses on cultural competence in hiring\n- Diversity training: over 1,000 teachers have participated in two-day trainings\n- Pilot program with St. Mary's University: teachers can earn cultural competency teaching certification\n\nChallenges and Solutions:\n- Last hired, first fired: teachers of color often laid off due to budget shortfalls and tenure rules\n- Long-term view: districts aim to rehire quality teachers of color post-recession\n- Different approaches: districts, colleges, and universities address diversity needs in various ways", "Historical district snapshot; original publication date not recovered. Relative dates below refer to that unavailable article, not the present.\n\nRoseville Schools:\n- Percentage of students of color: 25% in 2005, 37% currently\n- Students eligible for free or reduced-fee lunch: over one-third\n- Teachers of color: 41, representing 9% of the teaching staff\n- Multi-lingual teacher hires: 5 in the last year\n- Diversity initiatives: yearly all-staff workshops, equity in integration specialists in each school\n\nRAVE District:\n- Teachers of color: less than 2% of the teaching staff\n- Diversity coordinator: Scott Thomas, who focuses on cultural competence in hiring\n- Diversity training: over 1,000 teachers have participated in two-day trainings\n- Pilot program with St. Mary's University: teachers can earn cultural competency teaching certification\n\nChallenges and Solutions:\n- Last hired, first fired: teachers of color often laid off due to budget shortfalls and tenure rules\n- Long-term view: districts aim to rehire quality teachers of color post-recession\n- Different approaches: districts, colleges, and universities address diversity needs in various ways"), ('37% currently', '37% at the time of the source report'), ('5 in the last year', '5 in the year preceding the source report')]),
        'de7cd24288cb96b5': ('de7cd24288cb96b55db77a6aaaa72b68428aac2d27a6b69144079f0fbb77c54a', [('Eating Timing: To manage weight, avoid waiting until feeling hungry to eat.\nFood Selection: Opt for foods that occupy a large volume in the stomach but have fewer calories (low caloric density).\nHydration and Snacking: If experiencing hunger but having sufficient energy, consider consuming vegetables or water as a snack.', 'Weight Management and Food Choices\n\nLower-energy-density dietary patterns can support weight management in adults, but the supplied blanket instruction to eat before feeling hungry is not established here.\n\nVegetables can be part of a balanced eating pattern, and water supports hydration. Feeling energetic does not establish that someone has eaten enough; water should not be treated as a universal substitute for food when hungry. Meal timing and nutritional needs vary.')]),
        'd670e2c9a4487338': ('d670e2c9a4487338b200923535f372ed54434f9d1e6e94d12222bdd2e211587b', [("Purpose of Christmas Stories: To reveal truths about existence in relation to God and others, not to serve as literal historical reporting or scientific assertions.\nAuthorship and Timeline: Written by Matthew and Luke around 80-90 CE, relying on Mark (70 CE) and another source, Q.\nNature of Gospels: Not eye-witness accounts, but descriptions of early Christian communities' beliefs about Jesus.\nSimilarities and Differences: Both Matthew and Luke have genealogies, annunciations, virgin conception, Bethlehem birth, birth announcements, and Herod's actions, but with variations.\nDemythologizing: Interpreting the form of the stories as concealing a deeper truth, not dismissing them as untrue.\nAnnunciation: A common theme in the Old Testament and other religions, signifying a significant person, not literal angel visits.\nVirgin Conception: Not unique, symbolizing a remarkable conception, not biological anomaly. It signifies divine collaboration and destined greatness.\nBethlehem Birth: Either due to a census or refugee flight, symbolizing human vulnerability, humiliation, and oppression.\nWise Men and Shepherds: Inclusivity of Jesus' message for all people, symbolized by light vs. darkness metaphor.", "Summary of a December2020 theological commentary. Interpretations below belong to the author; they are not statements of universal religious agreement.\n\nPurpose of Christmas Stories: To reveal truths about existence in relation to God and others, not to serve as literal historical reporting or scientific assertions.\nAuthorship and Timeline: Written by Matthew and Luke around 80-90 CE, relying on Mark (70 CE) and another source, Q.\nNature of Gospels: Not eye-witness accounts, but descriptions of early Christian communities' beliefs about Jesus.\nSimilarities and Differences: Both Matthew and Luke have genealogies, annunciations, virgin conception, Bethlehem birth, birth announcements, and Herod's actions, but with variations.\nDemythologizing: Interpreting the form of the stories as concealing a deeper truth, not dismissing them as untrue.\nAnnunciation: A common theme in the Old Testament and other religions, signifying a significant person, not literal angel visits.\nVirgin Conception: Not unique, symbolizing a remarkable conception, not biological anomaly. It signifies divine collaboration and destined greatness.\nBethlehem Birth: Either due to a census or refugee flight, symbolizing human vulnerability, humiliation, and oppression.\nWise Men and Shepherds: Inclusivity of Jesus' message for all people, symbolized by light vs. darkness metaphor."), ('Authorship and Timeline: Written by Matthew and Luke around 80-90 CE, relying on Mark (70 CE) and another source, Q.', 'Authorship and Timeline: The author gives approximate dates for the Gospels and invokes Mark and Q. These are scholarly-source claims presented by the commentary; Q is a proposed source, not a surviving manuscript, and this does not establish that the infancy narratives came from Mark or Q.'), ('Bethlehem Birth: Either due to a census or refugee flight, symbolizing human vulnerability, humiliation, and oppression.', 'Bethlehem and Vulnerability: The author interprets the birth and refugee motifs as symbols of vulnerability, humiliation and oppression; this summary does not treat the flight into Egypt as the cause of birth in Bethlehem.')]),
        'adf6bfbff3fb41c6': ('adf6bfbff3fb41c6461884638a8e1828ed20829a7732add6eaae46f18433d447', [("\n\nJody Cramsie's Background:\n- Background in history, theology, ethics, and law.\n- Enjoys music, reading, and hosting dinner parties.\n- Lives in Spokane but prefers the Olympic Peninsula or the south of France.", ''), ('- Coincide with the darkest day of the year, representing Jesus as the new light.', "- In the author's Northern Hemisphere seasonal symbolism, Christmas falls near the winter solstice and represents Jesus as a new light; Christmas Day is not the solstice itself."), ("- The stories' existential claims (e.g., Jesus revealing God's compassion and love) are true.", "- The author regards the stories' existential claims, such as Jesus revealing divine compassion and love, as true.")]),
        'c3972e25e2bd6dc8': ('c3972e25e2bd6dc844463d0001ca38057b1fd4e1b1891706b0b28df8e25df455', [("LRE (Least Restrictive Environment) in Arizona is governed by the federal IDEA statute.\nLRE has two main components:\n- 1. Schools must provide services, aids, or accommodations to keep students in the general education setting as much as possible.\n- Example: Using reading aids to help a student in a general education classroom.\n- 2. If the student cannot succeed in the general education setting despite these efforts, they may be moved to a more restrictive environment, typically special education.\n- Example: A fifth-grader reading at a kindergarten level may require special education services.\nLRE standard in a 504 plan extends to areas outside the classroom, such as transportation, recess, lunches, and extracurricular activities.\nAn IEP uses LRE while tracking the student's progress towards specific goals, and may require more accommodations or separate instruction if necessary.", 'Least Restrictive Environment in Arizona\n\nUnder IDEA, children with disabilities should be educated with nondisabled children to the maximum extent appropriate. Removal from regular classes is considered only when education there cannot be achieved satisfactorily with supplementary aids and services.\n\nSpecial education is a set of services, not automatically a separate classroom. Placement is an individualized decision based on the IEP, made by a group including parents and people knowledgeable about the child, the evaluation and placement options. Reading below grade level alone does not require removal from an age-appropriate regular classroom.\n\nSupports may apply in classrooms, other educational settings, and extracurricular and nonacademic activities. Section504 has its own requirements; its relationship to placement should not be reduced to the supplied two-step rule. An IEP records individualized goals and services, and placement must be reviewed as required.')]),
        '83079ceb151c7b9f': ('83079ceb151c7b9f3329476e2e8767f420599d00f7eeb855ba5ce9b018b2b56d', [('most recently residence of King Charles III and Queen Consort Camilla', 'used by Charles and Camilla when they were Prince of Wales and Duchess of Cornwall; this summary does not verify their present living arrangements'), ("Palace of Holyroodhouse: Not explicitly mentioned in the text, but implied as a place where the Queen's remains were brought before leaving Scotland for London.", 'Palace of Holyroodhouse: The supplied text does not explicitly identify this location; the inferred identification is omitted rather than represented as a verified source statement.')]),
        '38d977e9bc6a89a1': ('38d977e9bc6a89a1546d1415d1e5d99d09cc1dacd3b84b2f604dc8ff82585854', [('- Originally conceived as an Augustinian monastery in 1128.', '- Holyrood Abbey was founded in1128; the later palace developed from royal accommodation associated with the abbey.'), ('- Designed by Sir William Bruce between 1671 and 1678.', '- Almost completely rebuilt between1671 and1679, with designs by Sir William Bruce incorporating earlier structures.'), ("- Designated as the monarch's official residence in Scotland in 1920.", "- Recognised as the sovereign's official Scottish residence following improvements during GeorgeV's reign; the supplied exact1920 designation date was not corroborated."), ('- Queen Elizabeth II spends one week of the year at Holyroodhouse (Holyrood Week).', '- During her reign, Queen ElizabethII used Holyroodhouse for official engagements, including Holyrood Week; the supplied present-tense statement is historical.'), ('- Functionalist views originate from Gothic Revival architects, emphasizing necessity and minimal ornamentation.', '- The supplied claim that Functionalism originated with Gothic Revival architects was not substantiated and is omitted; no replacement genealogy is inferred.'), ('- Tang joined as a partner in 2015, with degrees from Yale University and Asian studies.', '- Dana Tang, with the firm since1995, became a partner in2015; she holds a Master of Architecture from Yale and a Master of Regional Studies–East Asia from Harvard.')]),
        '1d64bfe14631f221': ('1d64bfe14631f22190e8de5ffa5a19796268a695f2f530c54da5bdd681260dbd', [('Prevalence:\n- 66-75% of babies evaluated over 10 years had torticollis and/or plagiocephaly.\n- Both conditions are preventable and treatable with coaching and commitment.', 'Source-specific Observation:\n- The clinician informally estimated that66–75% of babies they evaluated over ten years had torticollis and/or plagiocephaly. This selected clinical experience is not a population-prevalence study.\n- Prevention and treatment depend on the cause and individual assessment; these conditions are not universally preventable or guaranteed to resolve through parental commitment.'), ('- Can be identified using the Turn-Tilt-Top (T-T-T) method.', '- The source clinician proposes Turn-Tilt-Top (T-T-T) as a way for caregivers to notice possible concerns, not as a validated stand-alone diagnosis.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: nine changed source records, one unchanged batch record, all other491 source records unchanged; exact ID/hash guards, altered-source negative controls, and idempotence pass. Author-box span absent; legal-placement and clinical-statistic qualifications present. CSV exact header,303 unique IDs, single-line notes and verdict/code agreement pass. Original source pack unchanged; before/after hashes and full comparisons retained in local audit. No cleaned full-pack export produced.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial deletion removes unverified details and replaces some prescriptions with qualified guidance. These are bounded editorial changes, not reconstructions of inaccessible sources, comprehensive safety instructions, or independent clinical/product validation. Apply only the reviewed IDs with matching original hashes.


## Rows351–360 — DONE: listed-taxonomy review and editorial repairs

All ten full-read records labelled clean under the existing reviewer-approved listed-taxonomy policy; no supported disease code was established. Seven source-specific texts repaired separately. Clean does not deny original editorial errors, certify inaccessible sources, or imply gold-test success. Total labels:313 unique documents.

Repairs:351 individualized observation/exercises and safe sleep;352 developmental/helmet and sleep qualifications;354 unsupported conversion and off-page claims plus redundant summary;355 PPC scope and link-scheme qualification;358 commercial/clinical claims;359 historical frame, arithmetic and observational-study scope;360 historical operational rules.353,356,357 unchanged.321–330 remain preview-only. No listed disease code established; do not force C1 for substantive community-service information, C2 for limited paraphrase, C5 for explanatory SEO, or F1 for a public facility address.

```python
import hashlib

def repair_351_360(doc_id, text):
    rules = {
        '96785e5657cfa43e': ('96785e5657cfa43efb1de81f778f3a0ecaf7c69c33e4a0266d1b3cc2a83ecc8d', [('Head and Neck Movement Goals:\n- Baby\'s head and neck should turn freely to both directions, with a 90° turn, allowing chin to touch both shoulders.\n- Baby should initially depend on caregivers for positioning, then gain strength and learn to control it independently.\n- Turning action is first observed when baby is on their back or playing/resting on their tummy (supervised).\n\nIdentifying Persistent Turn (or Lack of Turn) Issues:\n- When baby sleeps on their back, head is usually turned more to one side.\n- When baby sleeps on their back, head stays still and centered, not turning at all.\n- When baby rests on their tummy, head is usually turned more to one side.\n- Baby develops a flat spot on the back of their head on the same side they prefer to turn.\n- Baby moves eyes more than head when turning, or hinges at the back of their neck instead of turning chin over shoulder.\n- Baby favors the same hand as the direction their head is mostly turned toward.\n- Baby prefers to be nursed or held on one side more than the other.\n\nNeck Tilt:\n- Neck should tilt comfortably to both directions.\n- Ear can touch the shoulder when tilted, and one ear will be closer to the same shoulder on the side they tilt toward.\n\nIdentifying Persistent Tilt Issues:\n- When baby travels in their car seat or lays on their back, head always falls/tilts to the same side.\n- Baby\'s body curves in one direction, making a "C" shape.\n- One side of baby\'s neck has more skin creases, is harder to clean, or has pink shiny skin.\n- One side of baby\'s neck collects more formula, breast milk, or dirt.\n- Baby has been diagnosed with a yeast infection on one side of their neck.\n- Baby favors one hand more than the other.', 'Caregiver observations, not a diagnostic checklist or instructions to force neck movement. Ask a pediatrician or pediatric physiotherapist about persistent asymmetry; any stretching should be taught for the individual baby.\n\nHead and Neck Movement Goals:\n- Baby\'s head and neck should turn freely to both directions, with a 90° turn, allowing chin to touch both shoulders.\n- Baby should initially depend on caregivers for positioning, then gain strength and learn to control it independently.\n- Turning action is first observed when baby is on their back or playing/resting on their tummy (supervised).\n\nIdentifying Persistent Turn (or Lack of Turn) Issues:\n- When baby sleeps on their back, head is usually turned more to one side.\n- When baby sleeps on their back, head stays still and centered, not turning at all.\n- When baby rests on their tummy, head is usually turned more to one side.\n- Baby develops a flat spot on the back of their head on the same side they prefer to turn.\n- Baby moves eyes more than head when turning, or hinges at the back of their neck instead of turning chin over shoulder.\n- Baby favors the same hand as the direction their head is mostly turned toward.\n- Baby prefers to be nursed or held on one side more than the other.\n\nNeck Tilt:\n- Neck should tilt comfortably to both directions.\n- Ear can touch the shoulder when tilted, and one ear will be closer to the same shoulder on the side they tilt toward.\n\nIdentifying Persistent Tilt Issues:\n- When baby travels in their car seat or lays on their back, head always falls/tilts to the same side.\n- Baby\'s body curves in one direction, making a "C" shape.\n- One side of baby\'s neck has more skin creases, is harder to clean, or has pink shiny skin.\n- One side of baby\'s neck collects more formula, breast milk, or dirt.\n- Baby has been diagnosed with a yeast infection on one side of their neck.\n- Baby favors one hand more than the other.'), ("- Baby's head and neck should turn freely to both directions, with a 90° turn, allowing chin to touch both shoulders.", '- Observe whether the baby turns comfortably in both directions. Do not use a universal angle or forced chin-to-shoulder movement as a home diagnostic test.'), ('- Ear can touch the shoulder when tilted, and one ear will be closer to the same shoulder on the side they tilt toward.', '- Observe persistent head tilt without forcing the ear toward the shoulder; assessment and exercises should be individualized.'), ('- Turning action is first observed when baby is on their back or playing/resting on their tummy (supervised).', '- Observe movement when the baby is awake. Tummy time should be awake and supervised; place the baby on their back for sleep.')]),
        'b268a3bd0ac54923': ('b268a3bd0ac5492356618a53723b6eb7130af9270c898a576c44eae8be8dc63a', [('- Can affect physical development and functional cosmetics (e.g., helmet and eyeglass wear)', '- Developmental or functional concerns should be assessed individually; this summary does not establish that positional flattening itself causes developmental delay.'), ("- Change baby's position frequently to avoid prolonged pressure on one area of the head", '- Vary positions during awake care and supervised tummy time. Continue placing the baby on their back for sleep; do not use sleep positioning changes to treat a flat head.'), ('- Positioning helmets can be used for infants 6 months and older to assist with changing the head shape, if needed', '- Helmet therapy is not determined by a universal minimum age alone. Suitability depends on diagnosis, severity, growth and response to other care; discuss options with the pediatric team, as recommendations vary by health system.')]),
        '52e959deb6f93f5b': ('52e959deb6f93f5be038678bbe1b132545b350d5493137e94cfb49c24edcd4bb', [('Organic Traffic Benefits: Organic traffic has a higher conversion rate than other traffic sources like paid ads, making SEO beneficial for generating leads and sales online.', 'Organic Traffic Benefits: Conversion performance varies by audience and campaign; organic traffic is not universally superior to paid traffic.'), ('Importance of SEO: Both on-page and off-page SEO are important for driving traffic and improving rankings, with off-page SEO being generally more impactful.', 'Importance of SEO: There is no universal rule that off-page work outweighs on-page or technical work. Outcomes depend on the site and query.'), ('\nSEO Types: The main types of SEO are on-page SEO and off-page SEO.\n- On-Page SEO: Activities done on the website to improve ranking, such as optimizing title tags, meta descriptions, and creating keyword-rich content.\n- Off-Page SEO: External activities to promote the website, including link building and social media engagement.', ''), ('SEO Results: Correct SEO practices can significantly increase organic traffic and lead to higher conversion rates, but results take time to manifest.', 'SEO Results: Improvements may take time and do not guarantee ranking, traffic or sales outcomes.')]),
        '27a1cdb2a1aa69fc': ('27a1cdb2a1aa69fc9cf80658347b01d518163562ff840a555ff4690873cc4745', [('- Ads are placed on Search Engine Results Pages (SERPs)', '- Search advertising can place ads on results pages; PPC is a payment model also used in other advertising contexts.'), ('- Building backlinks through guest posts and social media updates', '- Promote useful content to relevant audiences; avoid creating guest-post links primarily to manipulate ranking.'), ('- Backlink building through guest posting and social media activity', '- Earn relevant attention and links without paid or manipulative link schemes; social promotion is not a guaranteed ranking boost.')]),
        '8a28c9608e2c4628': ('8a28c9608e2c46281f56a9528c1efc7fdca56b5f15590b2cb92e74e7c9978463', [('Sam Lehrer is a professional with decades of experience in medical sales, management, and business development.', "The supplied business article presents a medical-equipment professional's perspective; the biographical credential claim has not been independently verified."), ('- A source of income for medical facilities.\n- Likely to have high patient costs.', '- Used for facility-specific services; revenue and patient costs depend on use, payment arrangements and other factors, not capital classification alone.'), ('Hyperbaric oxygen therapy (HBOT) machines are an example of capital medical equipment gaining popularity for various procedures.', 'Hyperbaric oxygen chambers may be purchased as capital equipment. This does not establish effectiveness for every promoted use: FDA guidance distinguishes supported indications from unapproved treatment claims.'), ("Impact on facilities: Purchasing capital equipment can:\n- Enhance a medical facility's reputation.\n- Improve overall performance.\n- Bring the facility to the cutting edge of technology.", 'Impact on facilities: Benefits depend on clinical need, evidence, staffing, maintenance and implementation. Buying an expensive device alone does not establish improved performance or reputation.')]),
        'cbdaf922f7711506': ('cbdaf922f7711506180b3976a258d2be3729bc01569b526d01f4a04ffbbe93f8', [('Community Fridge Initiative in Burnaby:\n- First fridge set up in late June, located outside Burnaby Youth Hub on Imperial St.\n- "Wildly successful" with consistent use and positive community feedback.\n- PCN considering opening more fridges: at SFU, a local temple, and with businesses or groups willing to host/maintain one.\n- Aimed at addressing food insecurity, with a focus on providing fresh produce.\n\nFood Insecurity in British Columbia:\n- Pre-pandemic, nearly 13% of British Columbians lacked consistent access to adequate nutrition.\n- During the pandemic, food bank usage in Burnaby increased by over 50% (from 2,000 to 3,200 people).\n\nImpact of Food Insecurity on Mental Health:\n- Food insecurity can worsen mental health issues, including anxiety, depression, and suicidal thoughts.\n- A study by UBC found that food worry during the pandemic was linked to increased mental health concerns.\n- Shame, stigma, and lack of support can exacerbate mental health issues related to food insecurity.\n\nRole of Nutrition in Mental Health:\n- Poor nutrition can negatively impact mental health and overall functioning.\n- Access to nutrient-rich foods, like those provided through the community fridge, can help mitigate these effects.\n\nBurnaby Primary Care Network (PCN) Efforts:\n- PCN has been accepting donations of fresh fruit and vegetables for the community fridge since its opening.\n- They aim to make nutrient-rich foods easily accessible to community members.', 'Historical report: September27,2021. Project plans and statistics below describe the source report, not current services.\n\nCommunity Fridge Initiative in Burnaby:\n- First fridge set up in late June, located outside Burnaby Youth Hub on Imperial St.\n- "Wildly successful" with consistent use and positive community feedback.\n- PCN considering opening more fridges: at SFU, a local temple, and with businesses or groups willing to host/maintain one.\n- Aimed at addressing food insecurity, with a focus on providing fresh produce.\n\nFood Insecurity in British Columbia:\n- Pre-pandemic, nearly 13% of British Columbians lacked consistent access to adequate nutrition.\n- During the pandemic, food bank usage in Burnaby increased by over 50% (from 2,000 to 3,200 people).\n\nImpact of Food Insecurity on Mental Health:\n- Food insecurity can worsen mental health issues, including anxiety, depression, and suicidal thoughts.\n- A study by UBC found that food worry during the pandemic was linked to increased mental health concerns.\n- Shame, stigma, and lack of support can exacerbate mental health issues related to food insecurity.\n\nRole of Nutrition in Mental Health:\n- Poor nutrition can negatively impact mental health and overall functioning.\n- Access to nutrient-rich foods, like those provided through the community fridge, can help mitigate these effects.\n\nBurnaby Primary Care Network (PCN) Efforts:\n- PCN has been accepting donations of fresh fruit and vegetables for the community fridge since its opening.\n- They aim to make nutrient-rich foods easily accessible to community members.'), ('increased by over 50% (from 2,000 to 3,200 people)', 'increased by60% using the reported rounded counts (from2,000 to3,200 people)'), ('- Food insecurity can worsen mental health issues, including anxiety, depression, and suicidal thoughts.', '- The cited study found associations between food worry and mental-health concerns, including anxiety, depression and suicidal thoughts; its design does not establish a simple causal pathway.'), ('- Access to nutrient-rich foods, like those provided through the community fridge, can help mitigate these effects.', '- The fridge aims to improve access to food. The cited study did not test fridge access as a treatment for mental illness.')]),
        'a34291af2a578913': ('a34291af2a578913786041e7efc1eaf91c7888be849db3d8ab9857dc6acb51d7', [("Support for Stigma-Aware Access: The community fridge is open 24/7 and doesn't require standing in line, helping people who may feel stigmatized about accepting charity or have scheduling constraints.\nAcceptable Donations: The fridge at 4750 Imperial St accepts fresh fruit and vegetables, dairy products, and fresh produce from backyard gardens. It also accepts prepared meals from commercial kitchens.\nNon-Acceptable Donations: The fridge does not accept homemade meals or leftovers due to health and safety reasons.\nDonation Location: The fridge is located at 4750 Imperial St.\nInvolvement Opportunities: Business owners can host a community fridge on their property, maintain one on someone else's property, or donate an extra fridge by reaching out to Burnaby PCN.", "Historical donation/access information reported on September27,2021. Verify present rules, hours and location with the operator before donating; this review does not confirm current operations.\n\nSupport for Stigma-Aware Access: The community fridge is open 24/7 and doesn't require standing in line, helping people who may feel stigmatized about accepting charity or have scheduling constraints.\nAcceptable Donations: The fridge at 4750 Imperial St accepts fresh fruit and vegetables, dairy products, and fresh produce from backyard gardens. It also accepts prepared meals from commercial kitchens.\nNon-Acceptable Donations: The fridge does not accept homemade meals or leftovers due to health and safety reasons.\nDonation Location: The fridge is located at 4750 Imperial St.\nInvolvement Opportunities: Business owners can host a community fridge on their property, maintain one on someone else's property, or donate an extra fridge by reaching out to Burnaby PCN.")]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: seven changed source records, three unchanged batch records, all other493 source records unchanged; exact ID/hash guards, altered-source negative controls, and idempotence pass. Rounded source-count increase verified as60%; safe-sleep and historical-operation qualifiers retained. CSV exact header,313 unique IDs, single-line notes and verdict/code agreement pass. Original source pack unchanged; before/after hashes and full comparisons retained in local audit. No cleaned full-pack export produced.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial deletion removes unverified details and replaces some prescriptions with qualified guidance. These are bounded editorial changes, not reconstructions of inaccessible sources, comprehensive safety instructions, or independent clinical/product validation. Apply only the reviewed IDs with matching original hashes.


## Rows361–370 — DONE: listed-taxonomy review and editorial repairs

All ten full-read records DONE:365 fix/F1 for a private child name; other nine clean under existing reviewer-approved listed-taxonomy policy. Nine source-specific texts repaired;368 unchanged. Clean does not deny original editorial errors or certify inaccessible sources. Total labels:323 unique documents.

Repairs:361 remove missing-answer placeholder and attribute beliefs;362 anatomy and medication qualification;363 unsupported bacterial-route explanation;364 psoriasis sites, scratching and course;365 mask/remove private identifier and replace conflated professional identities using independently verified accounts;366 distinguish conflicting planned figures from completed15-mural tally;367 omit stale undated award counts;369 David-house reference and speculative inferences;370 mixin/extend tradeoffs.321–330 remain preview-only. F1 evidence contains only type and original offset; no original private name is embedded in rule, preview or audit.

```python
import hashlib

def repair_361_370(doc_id, text):
    if doc_id == '6392c94fc52d201e' and hashlib.sha256(text.encode("utf-8")).hexdigest() == '6392c94fc52d201ee7d5cf53e8325c095cbf272705dd53592d962fb83d9d330c':
        return 'Chris Distefano and Chris DeStefano: Distinct Professional Identities\n\nChris Distefano is a comedian whose official website describes his comedy and podcast work. Chris DeStefano is a songwriter credited by professional music organizations with country songs including That’s My Kind of Night.\n\nThe supplied biography conflates these careers. Its unverified birth, physical measurements, family and relationship details, follower counts and net-worth estimate are omitted. A private child’s name is removed rather than copied into evidence or repair code.'
    rules = {
        'cdbdeae840411721': ('cdbdeae840411721ddff33c6aa0eba4451556d67b60ba02642b18cced9a42c9e', [('Biblical Perspective on Spiritual Goals: Not explicitly stated in the provided text.\n', ''), ('Spiritual Goals: Objectives set to draw closer to God or a higher power, with variations based on individual religious beliefs.\nPurpose of Spiritual Development: Pursuit of true worship within imperfections, offering oneself to God through work, play, and imitation of Christ.\nSpiritual Health: Balance between physical, psychological, and social aspects, including purposeful life, transcendence, and actualization of human capacities.\nBenefits of Spiritual Life: Hope, positive outlook, forgiveness, commitment, meaning, self-worth, clear values, and peace.\nValue of Spirituality: Enhances understanding and appreciation of diverse social classes, races, and cultures.\nFive Spiritual Values: Love, Generosity, Restraint, Truth, and Clarity, which foster spiritual growth.\nPersonal Goal Examples: Health-related (diet, exercise, overcoming diseases), educational (learning new skills, career advancement).', 'The following combines source-reported religious perspectives and general wellbeing claims; spiritual practices and values vary by tradition.\n\nSpiritual Goals: Objectives set to draw closer to God or a higher power, with variations based on individual religious beliefs.\nPurpose of Spiritual Development: Pursuit of true worship within imperfections, offering oneself to God through work, play, and imitation of Christ.\nSpiritual Health: Balance between physical, psychological, and social aspects, including purposeful life, transcendence, and actualization of human capacities.\nBenefits of Spiritual Life: Hope, positive outlook, forgiveness, commitment, meaning, self-worth, clear values, and peace.\nValue of Spirituality: Enhances understanding and appreciation of diverse social classes, races, and cultures.\nFive Spiritual Values: Love, Generosity, Restraint, Truth, and Clarity, which foster spiritual growth.\nPersonal Goal Examples: Health-related (diet, exercise, overcoming diseases), educational (learning new skills, career advancement).')]),
        '16bebda4445a97ef': ('16bebda4445a97efa32f38d4c48b3f13f7e2d29c3290813cd3849cb869403f63', [('- Other organs around the stomach include the small and large intestines, liver, spleen, gallbladder, pancreas, and parts of the urinary and reproductive systems.', '- The stomach is part of the digestive tract in the upper abdomen. The supplied inclusion of urinary and reproductive organs as immediate stomach neighbours is omitted.'), ('Stomach Pain Relief:\n- For cramping from diarrhea, loperamide (Imodium) or bismuth subsalicylate (Kaopectate or Pepto-Bismol) can help.\n- For other types of pain, acetaminophen (Aspirin Free Anacin, Liquiprin, Panadol, Tylenol) might be helpful.\n- To ease upper stomach pain, try applying heat, soaking in a warm bath, drinking clear fluids, and reducing intake of coffee, tea, and alcohol.\n\n', 'Abdominal Pain and Symptom Relief:\n- Treatment depends on the cause; a blanket painkiller or antidiarrhoeal recommendation is inappropriate. Loperamide treats diarrhoea and is not suitable for every patient or illness. Ask a clinician or pharmacist about suitability.\n- Sudden severe abdominal pain, bloody diarrhoea or signs of significant illness warrant prompt medical assessment rather than relying on home treatment.\n\n')]),
        '7437773cc59115f8': ('7437773cc59115f8262456538fc89fb4a6a9bf8b7318e2267e029fc99350c0f6', [('Source of Abdominal Pain from Infections: Infections in the throat, intestines, or blood can cause bacteria to enter the digestive tract, resulting in abdominal pain.', 'Infection and Abdominal Pain: Infection may be associated with abdominal pain, but the mechanism depends on the illness. The supplied universal account of bacteria entering the digestive tract from throat or blood infections is not established and is omitted.')]),
        'e52b546b23a714ef': ('e52b546b23a714ef45e0755e221aaba0553392c0250bc7099d0df7d0f6096365', [("Psoriasis Severity Variation: Psoriasis can become more or less severe over time and may appear to spread due to flare-ups, but it's not actually spreading to new locations.", 'Psoriasis Severity Variation: Psoriasis can flare or improve over time and can develop at new skin sites; it is not contagious.'), ("Remission Duration: Psoriasis treatment can lead to clear skin and symptom-free periods (remission), lasting from 1 to 12 months on average, but it's unpredictable.", 'Remission Duration: Treatment may produce clear skin or remission, but duration varies. The supplied universal1-to12-month average is not substantiated here.'), ('Psoriasis Flare-ups: Flare-ups can start small, spread, and then gradually improve. They are usually triggered by something, and scratching does not cause spread but may slow healing.', 'Psoriasis Flare-ups: Scratching or skin injury can worsen psoriasis and may trigger lesions at injured sites, known as the Koebner phenomenon. Triggers and response vary.'), ('4) No joint pain,', '4) Joint symptoms need separate assessment and are not a stand-alone measure of skin remission,')]),
        '4454ce48da5fd547': ('4454ce48da5fd5475ffd86ccc3df01fd78b3a49705733524d5c8ab28c5626909', [('- Dates: September 9-19, 2022', '- Historical2022 project: the organizer page gives September9–19 in its introduction and September12–18 in its heading; these date ranges are not silently reconciled.'), ('- Number of Murals: 16 ocean advocacy murals', '- Murals: the completed-project tally and gallery list15. Planning prose elsewhere on the page says16 or10; use the final tally while recording the source inconsistency.'), ('- Dates: September 12-17, 2022', '- Planned mural creation: September12–17,2022; this is the schedule in planning prose, not an independently verified completion timeline.')]),
        '41610d579b2603f0': ('41610d579b2603f04c9d27c8bb6a4bbb27a9246ab7fb757b1e5896e09a86cdb3', [('Awards:\n- Received four Academy Award nominations\n- Received five Golden Globe Award nominations\n- Won one Golden Globe\n', 'Awards: The supplied nomination/win totals are undated and unverified; they are omitted rather than presented as current counts.\n'), ('Incident:', 'Source-attributed Anecdote (original quotation not recovered):')]),
        'f3011171442ed668': ('f3011171442ed668239980ed0b7968b4b6bc3a54743edf5ad4f1d2090f5cc44b', [('- Mentioned in 2 Samuel 5:11 as the source of wood for the Temple.', '- In2Samuel5:11, Hiram of Tyre sends materials and workers to build David a house; this verse is not about construction of the Temple.'), ("- Tyre's prosperity 70 years after the prophecy suggests Alexander the Great's defeat as a likely candidate.", " - The prophecy's seventy-year language does not by itself establish which historical siege it describes; the supplied identification with Alexander's campaign is not verified here."), ("- Purple dye for Jesus' crucifixion robe likely came from Tyre.", " - The provenance of dye in the crucifixion robe is not established by the supplied references and is omitted rather than inferred from Tyre's trade reputation.")]),
        '0d500c26f5a85ad0': ('0d500c26f5a85ad02e76b08f492477dd43d471000559bf186c53fc40b9b4d9cc', [("- Use mixins only when they take in an argument; otherwise, extend selectors' rules.", '- Choose mixins or extends according to the relationship being expressed. Mixins can reuse declarations with or without arguments; extends are suited to semantic selector relationships and can affect the cascade. Inspect the generated CSS rather than treating arguments as the sole decision rule.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation: nine changed source records, one unchanged batch record, all other491 source records unchanged; exact ID/hash guards, altered-source negative controls, and idempotence pass. Private name absent from applied text and runnable rule; exact original hash retained. Public comparisons mask the private-name span; original source pack remains untouched. CSV exact header,323 unique IDs, single-line notes and verdict/code agreement pass. Original source pack unchanged; before/after hashes and full comparisons retained in local audit. No cleaned full-pack export produced.

Harm tests: pending Data A / Data B; no gold set supplied. Side effects: editorial deletion removes unverified details and replaces some prescriptions with qualified guidance. These are bounded editorial changes, not reconstructions of inaccessible sources, comprehensive safety instructions, or independent clinical/product validation. Apply only the reviewed IDs with matching original hashes.


## Rows371–400 — all30 DONE, two privacy/promotion interventions

371 fix/C1;F1 for trailing named testimonial;374 fix/F1;C1 for named direct-contact email and subscription footer. Evidence follows code order using space-bar-space, under300 characters; F1 values never copied, hex blank. Exact ID/hash full-output rules avoid storing sensitive original strings; comparison/audit originals mask values while retaining original offsets and hashes. Other28 full/clean under established reviewer-approved listed-taxonomy treatment; factual/editorial errors are documented separately. Total353 unique labels.321–330 remain preview-only.

27 texts repaired,379–381 unchanged with explicit source limits. Supported corrections include historical/date framing, political act attribution, circuit terminology, workload-dependent SER, flavoring-experiment scope, attributed climate economics, recovered Scott target areas, melatonin evidence limits, course table scope/arithmetic, React custom API/memoization, mutation versus title, pneumonia escalation/classification, Perl/PHP semantics, coolant-mist claims, observational pedagogy, scanner branding and measured/model-specific pool chemistry. No whole-pack cleaned export created.

```python
import hashlib

def repair_371_400(doc_id, text):
    reviewed_outputs = {
        'd75b731d61eb6433': ('d75b731d61eb64339140e502e6399133daccf255e1db55799a97b537ce98bef7', 'Purpose: A Notion database template for benchmarking analysis to help solopreneurs, entrepreneurs, startups, and SMEs track performance and identify improvement areas.\nCreator\'s Motivation: "What cannot be measured cannot be improved."\nBenefits:\n- Identify improvement areas in the company/project.\n- Learn from the competition.\n- Monitor company/project performance.\n- Identify productivity points.\nTarget Users:\n- Entrepreneurs, startups, and SMEs.\n- Professionals in finance, marketing, sales, operations, human resources, or internationalization departments.\nFeatures:\n- Over 600 indicators and metrics for benchmarking.\n- Onboarding guide and creation hub for customizing metrics.\n- Introductory guide to benchmarking and use cases.'),
        '6700f6a8840e9c51': ('6700f6a8840e9c5116d5f8cb805209b7e0c158d012717164e872d5dd93657a3b', "Transition Process:\n- Involves stopping harvesting in one area and preparing for the new area within 24-56 hours.\n- Requires coordination among multiple departments: drivers, sanitation, food safety, maintenance, etc.\n- Happens twice a year for many row-crop growers.\n- Logistically challenging due to simultaneous moves and varied customer needs.\n\nD'Arrigo Brothers:\n- First grower to ship fresh produce from the West Coast to the East Coast (1926).\n- Moves entire growing operations, including equipment and staff, from Yuma, Arizona to Salinas, California annually.\n- Equipment moved: 34 buses, 70 harvesters, 30 forklifts and tractors, 12 storage trailers, and 20 heavy trucks.\n- Transition process spans 1.5 months.\n\nPRO*ACT's Role:\n- Assists suppliers and customers with communication during the transition process.\n- Helps D'Arrigo Brothers communicate with the entire supply chain about product timing, availability, and quality.\n\n2021 Transition Dates:\n- Spring move starts on April 5, 2021.\n- Suppliers loading in Salinas on April 5: Mann Packing, Duda, Church Brothers, Nunes, Salad Farms.\n- Later transitions: Ippolito (April 12), Taylor Farms (April 19), Greengate (April 26)."),
    }
    direct = reviewed_outputs.get(doc_id)
    if direct is not None and hashlib.sha256(text.encode("utf-8")).hexdigest() == direct[0]:
        return direct[1]
    rules = {
        'e8ee3f749c1643f4': ('e8ee3f749c1643f41069c85553c9398ff330cd7a36a3ef6a6040315e6aa88528', [("Year-Round Growing in Salinas Valley: Salinas Valley has a Mediterranean-like climate that allows for nearly year-round food production, with some varieties of produce growing between 200 to 350 days.\nTransition to Yuma, Arizona: Growers, including PRO*ACT's network, move their operations to Yuma, Arizona, between November and April due to freezing winter temperatures in Salinas Valley.\nOptimizing Land Usage: Growers follow the sun to ensure crops grow in optimal conditions, with some moving inland to Huron, California, or to Yuma, Arizona.\nIppolito's Move: Ippolito, a major grower, moves to Yuma, Arizona, during winter due to cold temperatures in Salinas Valley, with a few exceptions like Brussels sprouts.\nPROACT's Role: PROACT keeps buyers informed about each company's moving schedule to minimize supply chain disruptions.\nMann Packing's Dual Transition: Mann Packing uses a two-tiered transition, moving their Better Burger lettuce two weeks before their Arcadian Blend variety, to maximize output and cater to each commodity's needs.", "Historical supplier transition summary:2021. Dates and practices below are source-reported and crop-specific, not a current shipping schedule.\n\nYear-Round Growing in Salinas Valley: Salinas Valley has a Mediterranean-like climate that allows for nearly year-round food production, with some varieties of produce growing between 200 to 350 days.\nTransition to Yuma, Arizona: Growers, including PRO*ACT's network, move their operations to Yuma, Arizona, between November and April due to freezing winter temperatures in Salinas Valley.\nOptimizing Land Usage: Growers follow the sun to ensure crops grow in optimal conditions, with some moving inland to Huron, California, or to Yuma, Arizona.\nIppolito's Move: Ippolito, a major grower, moves to Yuma, Arizona, during winter due to cold temperatures in Salinas Valley, with a few exceptions like Brussels sprouts.\nPROACT's Role: PROACT keeps buyers informed about each company's moving schedule to minimize supply chain disruptions.\nMann Packing's Dual Transition: Mann Packing uses a two-tiered transition, moving their Better Burger lettuce two weeks before their Arcadian Blend variety, to maximize output and cater to each commodity's needs."), ('between November and April due to freezing winter temperatures in Salinas Valley', 'for winter production, according to crop and seasonal conditions; freezing is not a description of the entire Salinas winter')]),
        '722d685d277ea67e': ('722d685d277ea67e06807df6d6d821df40a23d039c4321f04949a808e2396dfa', [("PRO*ACT's Role in Food Safety and Quality Control:\n- Monitors certifications, groundwater contamination readings, and mildew pressure.\n- Assesses soil conditions to predict potential supply interruptions.\n- Works with growers to ensure soil readiness for crops.\n\nSeasonal Produce Supply Management:\n- Large produce suppliers plan future seed planting based on historical data, experience, and intuition.\n- PRO*ACT's sourcing team aligns expectations with suppliers and customers during conference calls.\n- Taylor Farms uses advanced technology (26 robots, new harvesting machinery) to expedite produce refrigeration and supply chain entry.\n\nTransition Season Challenges and Solutions:\n- Transition season poses challenges due to harsh winter conditions.\n- Taylor Farms and other suppliers work to maintain seamless, consistent product quality despite seasonal changes.\n- PRO*ACT assists suppliers during Transition by providing insights and facilitating communication.\n\nBest Organizational Practices During Transition:\n- Church Brothers Farms, a processor and row crop supplier, coordinates multiple departments for a smooth Transition:\n- Ag-Ops team verifies harvest timelines.\n- HR hires labor force.\n- Shipping facilities are prepared.\n- Field harvesting equipment is transported.\n- Processing facility is relocated (over 60 trucks, 580 miles, in less than 60 hours).\n\nKey Players in Transition:\n- Over 200 people are involved in Church Brothers' processing facility relocation alone.\n- People's efforts and coordination are crucial for a successful Transition.", "Historical2021 supplier report. Equipment counts and logistics are reported examples, not current fleet figures or independent food-safety certification.\n\nPRO*ACT's Role in Food Safety and Quality Control:\n- Monitors certifications, groundwater contamination readings, and mildew pressure.\n- Assesses soil conditions to predict potential supply interruptions.\n- Works with growers to ensure soil readiness for crops.\n\nSeasonal Produce Supply Management:\n- Large produce suppliers plan future seed planting based on historical data, experience, and intuition.\n- PRO*ACT's sourcing team aligns expectations with suppliers and customers during conference calls.\n- Taylor Farms uses advanced technology (26 robots, new harvesting machinery) to expedite produce refrigeration and supply chain entry.\n\nTransition Season Challenges and Solutions:\n- Transition season poses challenges due to harsh winter conditions.\n- Taylor Farms and other suppliers work to maintain seamless, consistent product quality despite seasonal changes.\n- PRO*ACT assists suppliers during Transition by providing insights and facilitating communication.\n\nBest Organizational Practices During Transition:\n- Church Brothers Farms, a processor and row crop supplier, coordinates multiple departments for a smooth Transition:\n- Ag-Ops team verifies harvest timelines.\n- HR hires labor force.\n- Shipping facilities are prepared.\n- Field harvesting equipment is transported.\n- Processing facility is relocated (over 60 trucks, 580 miles, in less than 60 hours).\n\nKey Players in Transition:\n- Over 200 people are involved in Church Brothers' processing facility relocation alone.\n- People's efforts and coordination are crucial for a successful Transition.")]),
        '778979be5b554b91': ('778979be5b554b9130db03f662f7c09760f9cb5f94218ecf33ec6ac75dfccaf7', [('Femi Fani-Kayode, former Minister of Aviation, has expressed support', 'Historical pre-2023-election endorsement: Femi Fani-Kayode, former Minister of Aviation, expressed support'), ("- Ensuring the National Stadium in Abuja was named after MKO Abiola and declaring June 12th as Democracy Day, 20 years after Abiola's death.", '- The supplied summary credits Tinubu in connection with honours for Abiola. Official records identify President Muhammadu Buhari as announcing June12 Democracy Day and the2019 Abuja stadium renaming; an endorsement does not transfer credit for those official acts.')]),
        '3785c00316f84fb2': ('3785c00316f84fb281742ccb49db9032cac64a32088ebf84024d36b89ce287a8', [('- Passive Components: Resistors, capacitors, inductors (expend, store, release energy).', '- Passive Components: Resistors dissipate energy, while capacitors and inductors can store and release it.'), ('- Active Components: Batteries, transistors, diodes, integrated circuits (computer chips) (provide power, process electrical signals).', '- Sources and Devices: Batteries supply energy; semiconductor devices such as transistors and integrated circuits process or control signals. Active/passive terminology depends on the circuit convention; not every listed device is a power source or amplifier.')]),
        '1aa44f38aab9f966': ('1aa44f38aab9f96669b5db82a087bb16825fb3d6b5bf78732749eb50908fb674', [('introduced by Nvidia, likely to be a part of the RTX 40 series', "introduced with NVIDIA's Ada/RTX40 generation"), ('rather than relying on manual ordering by developers', 'while still requiring application/API integration to use the feature effectively'), ("SER's implementation results in twice the shader efficiency and a 25% increase in framerates with ray tracing enabled.", 'NVIDIA reports workload-dependent speedups and up-to performance figures. Neither doubled shader efficiency nor a25% frame-rate increase is a universal result; measure the specific application.')]),
        'be71aae2f5844693': ('be71aae2f584469330f5dc978e70cb019cf74975ffaacf488d7f7af299fca931', [('Scientists propose insect-based food as a solution to world hunger and environmental issues.\n- Insects considered: Mealworms and beetle larvae.\n- Proposed dish: Insect-based "meat" mixed with sugar, resembling a burger.\n- Nutritional benefits of insects:\n- High in protein, similar to meat.\n- Rich in fatty acids, vitamins, minerals, and fiber.\n- Mealworms contain beneficial essential amino acids and are high in unsaturated fatty acids.\n- Driving factors for the proposal:\n- Growing global population (predicted 9.7 billion by 2050).\n- High cost of animal protein and environmental issues related to livestock farming, such as methane gas production by cows.\n- Challenges:\n- Insects\' repellent appearance and unique flavor characteristics may hinder universal acceptance in food cultures.\n- Research findings:\n- A "meat-like" substitute was created using sugar and worms, giving off a "meat-like" scent.', 'Mealworm Flavoring Research:2022\n\nResearchers presented experiments heating mealworm material with sugars to create meat-like reaction flavors. The reported result was a potential savory flavoring or seasoning, not a demonstrated ready-made burger or a proven solution to world hunger.\n\nEdible-insect foods were discussed in the context of protein supply and environmental pressures. Nutrition, safety, acceptability and environmental performance depend on the product and production system; the flavor experiment alone does not establish those outcomes at population scale.')]),
        '4cca1f02b4e29153': ('4cca1f02b4e291537684da9610751e40862be6d3f0c2929984930fabeb26f99b', [("Investment in Energy Technology R&D:\n- Invest 0.5% of GDP in energy technology RD&D.\n- Returns $11 for every dollar spent.\n- Allows nations to continue economic development until cost-effective low-carbon technologies are available.\n- Should be used with incentives like intensity targets or carbon pricing.\n\nInvestment in Adaptation:\n- Invest 0.05% of GDP in adaptation.\n- Has both specific location-based benefits and costs, but benefits outweigh costs.\n- Essential for hard-hit areas to prevent damage and continue economic development.\n\nIneffective or Uncertain Targets:\n- Global annual carbon emission reduction targets and emission intensity targets are relatively ineffective.\n- Returns less than one dollar for every dollar spent due to lack of low-carbon energy sources.\n\nGaliana's Assessment:\n- Best target: Invest 0.5% of GDP in green energy R&D.\n- 2°C target has lower benefits than costs.\n- Failure of international climate agreements due to energy technology problem.\n\nFischer's Perspective:\n- Agrees with Galiana on energy RD&D investment.\n- Stresses the need for carbon pricing to align social costs with market prices.\n- Integrated assessment models may not capture all warming costs and distributional impacts.\n\nMendelsohn's Perspective:\n- 2°C target is too demanding and costly.\n- Proposes a modest target of 4°C with immediate action and adaptation strategies.\n\nForumCC's Viewpoint:\n- Praises Galiana's evidence-based arguments.\n- Criticizes the lack of inclusion of adaptation, climate finance, and loss and damage, especially for developing countries.", "Historical Post-2015 policy debate: the figures and temperature proposals below are attributed economic assessments with model-dependent assumptions, not guaranteed returns or scientific declarations of safe warming.\n\nInvestment in Energy Technology R&D:\n- Invest 0.5% of GDP in energy technology RD&D.\n- Returns $11 for every dollar spent.\n- Allows nations to continue economic development until cost-effective low-carbon technologies are available.\n- Should be used with incentives like intensity targets or carbon pricing.\n\nInvestment in Adaptation:\n- Invest 0.05% of GDP in adaptation.\n- Has both specific location-based benefits and costs, but benefits outweigh costs.\n- Essential for hard-hit areas to prevent damage and continue economic development.\n\nIneffective or Uncertain Targets:\n- Global annual carbon emission reduction targets and emission intensity targets are relatively ineffective.\n- Returns less than one dollar for every dollar spent due to lack of low-carbon energy sources.\n\nGaliana's Assessment:\n- Best target: Invest 0.5% of GDP in green energy R&D.\n- 2°C target has lower benefits than costs.\n- Failure of international climate agreements due to energy technology problem.\n\nFischer's Perspective:\n- Agrees with Galiana on energy RD&D investment.\n- Stresses the need for carbon pricing to align social costs with market prices.\n- Integrated assessment models may not capture all warming costs and distributional impacts.\n\nMendelsohn's Perspective:\n- 2°C target is too demanding and costly.\n- Proposes a modest target of 4°C with immediate action and adaptation strategies.\n\nForumCC's Viewpoint:\n- Praises Galiana's evidence-based arguments.\n- Criticizes the lack of inclusion of adaptation, climate finance, and loss and damage, especially for developing countries."), ('- Returns $11 for every dollar spent.', '- The Galiana assessment summary reports an estimated$11 benefit per dollar under its assumptions.'), ('- Global annual carbon emission reduction targets and emission intensity targets are relatively ineffective.', '- The Galiana summary rates the assessed annual-emissions target poorly; its table lists emission-intensity target returns as uncertain.')]),
        '281de2e3af773d76': ('281de2e3af773d764be0ff1e5bbe2c348126212e64e1885b63823de1398ace79', [('- Three areas of targets are suggested to better integrate climate change into the post-2015 development agenda:\n- Specific details on these targets are not provided in the given text.', '- In her15October2014 working paper, Scott discusses low-carbon development, adaptation and resilience, emphasizing outcome-based resilience rather than only spending targets.'), ('involving 60 teams of economists', 'described in this paper as involving62 teams of economists'), ("Climate change is a global challenge affecting all countries, but the impact varies due to differences in adaptive capacity, which is often linked to a country's level of development.\nUN's past approach to climate change: The UN has addressed climate change in the past, but the focus needs to shift to how it can be incorporated into international development.\nLucy Scott's viewpoint paper:\n- Author: Lucy Scott, Research Officer at the Overseas Development Institute\n- Key points:\n- The UN should adapt its approach to incorporate climate change into the post-2015 development agenda.\n- In her15October2014 working paper, Scott discusses low-carbon development, adaptation and resilience, emphasizing outcome-based resilience rather than only spending targets.\nThe Post-2015 Consensus project:\n- A collaborative effort described in this paper as involving62 teams of economists, NGOs, international agencies, and businesses.\n- Aim: To identify the targets with the greatest benefit-to-cost ratio for the UN's post-2015 development goals.\nKey question: The question is no longer whether climate change should be incorporated into the international development agenda, but rather how it should be incorporated.", "Historical2014 viewpoint, written before adoption of the post-2015 goals.\n\nClimate change is a global challenge affecting all countries, but the impact varies due to differences in adaptive capacity, which is often linked to a country's level of development.\nUN's past approach to climate change: The UN has addressed climate change in the past, but the focus needs to shift to how it can be incorporated into international development.\nLucy Scott's viewpoint paper:\n- Author: Lucy Scott, Research Officer at the Overseas Development Institute\n- Key points:\n- The UN should adapt its approach to incorporate climate change into the post-2015 development agenda.\n- In her15October2014 working paper, Scott discusses low-carbon development, adaptation and resilience, emphasizing outcome-based resilience rather than only spending targets.\nThe Post-2015 Consensus project:\n- A collaborative effort described in this paper as involving62 teams of economists, NGOs, international agencies, and businesses.\n- Aim: To identify the targets with the greatest benefit-to-cost ratio for the UN's post-2015 development goals.\nKey question: The question is no longer whether climate change should be incorporated into the international development agenda, but rather how it should be incorporated.")]),
        'd828c8d4ffabf49c': ('d828c8d4ffabf49c53d3f74550575df1254fb2f56202ce04cdbe7685c878ec6d', [("- High stress levels, which increase cortisol production and interfere with melatonin's function.\n- Alcohol consumption, which reduces melatonin production and negates its benefits.", '- Stress and alcohol may affect sleep; the supplied absolute hormone-mechanism and negated-benefit claims are not substantiated here.'), ("Melatonin is not addictive, unlike alcohol. However, it's important to use it moderately and as part of a healthy lifestyle.", 'Melatonin should not be assumed harmless from an addiction comparison. Indications, timing, interactions and individual risks matter; long-term supplement safety remains uncertain.')]),
        '555d2d15fcb87926': ('555d2d15fcb87926f96a26195f4df74915315ac6efc075178b49aef1d54ac0cd', [("Light Exposure at Night Inhibits Melatonin Production: Light, especially from screens like mobile devices, suppresses the body's natural melatonin production, disrupting the circadian rhythm and sleep.\nHealth Impacts of Nighttime Light Exposure: Nighttime light exposure can increase the risk of illness, weaken the immune system, and hinder bone growth. It is also linked to an increased risk of cancer and metabolic diseases like diabetes.\nMelatonin Supplement Dosage and Timing: For adults, the recommended dosage of melatonin supplements ranges from 0.5 to 5 milligrams. It is suggested to take melatonin an hour before bedtime, with potential effects felt within half an hour.\nMelatonin's Benefits Beyond Sleep: Apart from improving sleep quality, melatonin supports the immune system, aids in cellular repair, and may help prevent cancer.\nImproving Overall Health Starts with Better Sleep: Rebalancing sleep patterns, potentially with the help of melatonin, can significantly contribute to enhancing overall health and wellness.", 'Melatonin, Light and Evidence Limits\n\nLight at night can suppress natural melatonin production and affect circadian timing. This does not establish that ordinary screen exposure universally impairs immunity or bone growth, or that taking melatonin prevents cancer.\n\nDose and timing depend on the indication, formulation, other medicines and individual circumstances. The supplied0.5–5mg bedtime instruction is not retained as a universal prescription. Discuss supplement use with an appropriate clinician; evidence for long-term safety and many claimed benefits is limited.')]),
        'd501a8a7c9c57061': ('d501a8a7c9c57061589bf018fdd58c9fba45d2f7e6533c0b689217b2c3bdb746', [("Course Details:\n- Semester 1 Credit Value: 15\n- Semester 2 Credit Value: 5\n- ECTS Credits: 10.0\n\nCourse Aims:\n- To demonstrate the intrinsic link and complementarity of 'design' and 'technology'.\n- To emphasize the impact and relevance of sustainable building technologies, construction methods, materials, and design on natural and man-made environments, considering the current climate emergency.\n- To introduce the concept of architectural design focusing on both the 'whole' (building responding to place and context) and the 'parts' (constructional materials, elements, components, junctions, and details).\n- To familiarize students with common constructional systems, their sequences, and the importance of constructional junctions and details.\n- To reinforce previously learned structural principles, increase understanding of structural orders, and consider the spatial implications of different structural systems.\n- To introduce relevant construction legislation, codes of practice, and health and safety issues.\n- To highlight the collaborative nature of building and construction realization.\n\nSyllabus Outline:\n- Lecture 1: Explain the overlap and integration of design and technology, introduce 'details and detailing', address students' fears about construction and technology learning, and suggest strategies to help.\n- Lecture 2: Encourage students to realize their potential to make a difference in environmental and climate emergency-related issues, contextualize this within broader architectural student skills and attributes, and present examples and issues for students to consider.\n- Lecture 3: Propose that impactful architectural design should consider both the 'whole' and the 'parts', allowing each to inform the other.\n- Lectures 4 and 5: Consider various common constructional systems, their history, material implications, dimensional issues, reasons for choice, and current and future applications. Examine the bringing together of elements and materials, including 'Fabric-First' thinking and 'tectonics'.\n- Lectures 6 and 7: Consider structural assembly processes and sequences, alongside the spatial implications that structural systems often suggest or dictate.", "Historical module context: Newcastle2022/23 regulations corroborate ARC2009 as20 credits split15/5 across semesters. Syllabus text below remains the supplied historical account, not a current course guarantee.\n\nCourse Details:\n- Semester 1 Credit Value: 15\n- Semester 2 Credit Value: 5\n- ECTS Credits: 10.0\n\nCourse Aims:\n- To demonstrate the intrinsic link and complementarity of 'design' and 'technology'.\n- To emphasize the impact and relevance of sustainable building technologies, construction methods, materials, and design on natural and man-made environments, considering the current climate emergency.\n- To introduce the concept of architectural design focusing on both the 'whole' (building responding to place and context) and the 'parts' (constructional materials, elements, components, junctions, and details).\n- To familiarize students with common constructional systems, their sequences, and the importance of constructional junctions and details.\n- To reinforce previously learned structural principles, increase understanding of structural orders, and consider the spatial implications of different structural systems.\n- To introduce relevant construction legislation, codes of practice, and health and safety issues.\n- To highlight the collaborative nature of building and construction realization.\n\nSyllabus Outline:\n- Lecture 1: Explain the overlap and integration of design and technology, introduce 'details and detailing', address students' fears about construction and technology learning, and suggest strategies to help.\n- Lecture 2: Encourage students to realize their potential to make a difference in environmental and climate emergency-related issues, contextualize this within broader architectural student skills and attributes, and present examples and issues for students to consider.\n- Lecture 3: Propose that impactful architectural design should consider both the 'whole' and the 'parts', allowing each to inform the other.\n- Lectures 4 and 5: Consider various common constructional systems, their history, material implications, dimensional issues, reasons for choice, and current and future applications. Examine the bringing together of elements and materials, including 'Fabric-First' thinking and 'tectonics'.\n- Lectures 6 and 7: Consider structural assembly processes and sequences, alongside the spatial implications that structural systems often suggest or dictate.")]),
        '29f82ccee275b72f': ('29f82ccee275b72fbba9afee8fe641e8805237ed70ab1ff6c89b56621ba15b87', [('- Guided Independent Study: Assessment preparation and completion (4 hours, 16 student hours).\n- Scheduled Learning: Lectures (4 hours, 8 student hours), Large Group Seminars/Symposiums (live and recorded, 12 student hours).', '- The supplied assessment/lecture entries mix session counts and hours ambiguously. Their exact table interpretation could not be recovered, so the4/16 and4/8 numerical descriptions are omitted rather than reconstructed.\n- Large Group Seminars/Symposiums: the supplied historical account reports12 student hours; not independently verified.'), ("Course Focus: ARC2009, focusing on constructional roles, responsibilities, communication, coordination, and building regulations (Parts M, K, B).\nSymposium Themes: Climate Emergency related themes in the 2021/22 session, with integration of core syllabus into studio design projects.\nTeaching Methods:\n- The supplied assessment/lecture entries mix session counts and hours ambiguously. Their exact table interpretation could not be recovered, so the4/16 and4/8 numerical descriptions are omitted rather than reconstructed.\n- Large Group Seminars/Symposiums: the supplied historical account reports12 student hours; not independently verified.\n- Learning Materials: Lecture materials (36 x 20-minute videos, 12 student hours).\n- Assessment: Portfolio format compiled throughout the semester.\nTeaching Rationale:\n- 2022/23 Proposal: Redeployment of video resources, non-synchronous delivery, live 'in-person' sign-posting talks and Symposiums, increased workshops, student-led preparatory sessions.\n- Symposium Details: 4 in total, addressing current technology and design issues, guest speakers, student group discussions, and presentations.\nTeaching Schedule:\n- ARC2009 taught on Thursdays during Semester 1.\n- ARC2001 (Architectural Design 2.1) jointly taught with ARC2009.", "Historical module proposal/account referring to2021/22 and2022/23; these descriptions are not a current timetable.\n\nCourse Focus: ARC2009, focusing on constructional roles, responsibilities, communication, coordination, and building regulations (Parts M, K, B).\nSymposium Themes: Climate Emergency related themes in the 2021/22 session, with integration of core syllabus into studio design projects.\nTeaching Methods:\n- The supplied assessment/lecture entries mix session counts and hours ambiguously. Their exact table interpretation could not be recovered, so the4/16 and4/8 numerical descriptions are omitted rather than reconstructed.\n- Large Group Seminars/Symposiums: the supplied historical account reports12 student hours; not independently verified.\n- Learning Materials: Lecture materials (36 x 20-minute videos, 12 student hours).\n- Assessment: Portfolio format compiled throughout the semester.\nTeaching Rationale:\n- 2022/23 Proposal: Redeployment of video resources, non-synchronous delivery, live 'in-person' sign-posting talks and Symposiums, increased workshops, student-led preparatory sessions.\n- Symposium Details: 4 in total, addressing current technology and design issues, guest speakers, student group discussions, and presentations.\nTeaching Schedule:\n- ARC2009 taught on Thursdays during Semester 1.\n- ARC2001 (Architectural Design 2.1) jointly taught with ARC2009.")]),
        'cb4930e9378f7560': ('cb4930e9378f7560b36e310feb4433906f487c18eabd57102960b5a490b8fd58', [('Non-Synchronous Learning: 31 hours in total, including core lecture material delivered as 45 x 20-minute videos.', 'Non-Synchronous Learning: The supplied account reports31 hours overall. Its45 videos of20 minutes account for15 hours; the wider total may include other tasks and is not an arithmetic contradiction by itself.'), ('Total Live Contact Time: 33 hours, spread across two semesters.\nLive Session Types: Drop-in/Surgeries, Module Talks, Symposium Sessions, Assessment Workshops.\nNon-Synchronous Learning: The supplied account reports31 hours overall. Its45 videos of20 minutes account for15 hours; the wider total may include other tasks and is not an arithmetic contradiction by itself.\nFall-Back Plan for Lockdowns: All live sessions can be moved online via Teams/Zoom if needed.\nResit Format: Determined by the Board of Examiners.\nAssessment Details:\n- Semester 1: Case study (75%), illustrated report focusing on detailed construction and design.\n- Semester 2: Case study (25%), illustrated report focusing on the relationship between design and technology and the application of UK Building Regulation.\nAssessment Rationale: 100% coursework assessment to link technology understanding with personal design work. Semester 1 assessment is a portfolio submission with sections submitted at different points.', 'Historical supplied module account; exact live-hours and assessment table not recovered.\n\nTotal Live Contact Time: 33 hours, spread across two semesters.\nLive Session Types: Drop-in/Surgeries, Module Talks, Symposium Sessions, Assessment Workshops.\nNon-Synchronous Learning: The supplied account reports31 hours overall. Its45 videos of20 minutes account for15 hours; the wider total may include other tasks and is not an arithmetic contradiction by itself.\nFall-Back Plan for Lockdowns: All live sessions can be moved online via Teams/Zoom if needed.\nResit Format: Determined by the Board of Examiners.\nAssessment Details:\n- Semester 1: Case study (75%), illustrated report focusing on detailed construction and design.\n- Semester 2: Case study (25%), illustrated report focusing on the relationship between design and technology and the application of UK Building Regulation.\nAssessment Rationale: 100% coursework assessment to link technology understanding with personal design work. Semester 1 assessment is a portfolio submission with sections submitted at different points.')]),
        '544fbb011fda1027': ('544fbb011fda1027c05c1512dce8fdbbd37572c3aca2445f29091a1b366b3a61', [('Function to get form data: `this.getFormData()` retrieves an object containing the current state of the form.', 'Example-specific method: `this.getFormData()` is a method described by the supplied example, not a built-in React Component API.'), ('Caching the function: The alternative solution creates the function once and stores it, preventing excessive re-renders and improving performance.', 'Caching the function: Stable callback identity can help an optimized child skip some renders when other relevant props are unchanged. It does not universally prevent re-renders; correctness and measured performance depend on the component and dependencies.')]),
        '057134082816d6b6': ('057134082816d6b65e9f8ad77a8d1cfae3535898a4a1900b0382e996ebc01953', [('Mutation: Details sent to Tehsil/Taluka office for mutation (transfer of title) and update in Record of Rights (ROR).', 'Mutation: Revenue records may be updated to reflect a transaction. Mutation serves fiscal/record purposes and does not itself confer legal title; procedures vary by state.'), ('- Patta: State government provides documentary evidence of right over land (patta) to the buyer.', '- Patta and other records: Names, evidentiary status and processes vary by state; not every transaction produces the same document or proves title conclusively.'), ('- Conflict: Property-related conflicts account for almost 2/3rds of all civil litigation in India.', '- Conflict: The source reports a nearly two-thirds litigation figure without a verified dataset/year here; it is omitted as a quantitative conclusion.')]),
        '6a473fe19500e060': ('6a473fe19500e060bb0823b7e36809507da989dc3d2a911b9ab12e4efa97ab9e', [('Symptoms of Pneumonia:\n- In infants and newborns: Restlessness, lethargy, fever, cough, vomiting.\n- In older adults and those with weak immune systems: Fever (lower temperature), confusion, change in mental status.\n- In all ages: Difficulty breathing, rapid breathing, bluish or gray color to fingernails or lips, fever (over 102°F in children over 6 months, over 100.4°F in infants under 6 months).\n\nWhen to Call the Doctor:\n- For children: Trouble breathing, rapid breathing, bluish or gray fingernails/lips, fever over 102°F (over 6 months) or 100.4°F (under 6 months), fever lasting after antibiotics.\n\nCauses of Pneumonia:\n- Viruses or bacteria.\n- Spread from other parts of the body (e.g., nose, sinuses, mouth) or directly breathed into lungs.\n\nTreatment of Pneumonia:\n- Antibiotics for bacterial pneumonia.\n- Antifungal drugs for fungal pneumonia.\n- No antibiotics for viral pneumonia.\n\nWhen to Visit the Emergency Room:\n- Bluish fingernails/lips, gurgling sounds from the throat, confusion, labored/fast breathing, whistling/wheezing sounds while breathing.\n\nPneumonia Classification:\n- Community-acquired pneumonia (CAP).\n- Hospital-acquired pneumonia (HAP).\n\nHome Remedy:\n- Drinking hot peppermint tea to help clear nasal pathways.', 'Pneumonia: Symptoms, Urgency and Treatment\n\nPneumonia is a lung infection or inflammation that can cause cough, breathlessness, fever and fatigue. Older people may become confused; babies and vulnerable patients can become seriously ill.\n\nSevere breathing difficulty, blue or blotchy lips/skin, sudden confusion or a baby who is floppy or cannot be woken require emergency help. Do not wait for a particular fever threshold when serious signs are present.\n\nTreatment depends on clinical assessment and cause. Bacterial pneumonia usually requires antibiotics; antibiotics do not treat viruses themselves, although clinicians may treat suspected bacterial coinfection. Antifungals are used for appropriate fungal infections. A warm drink may be comforting but is not pneumonia treatment; the specific peppermint remedy is omitted.')]),
        '47457fba904dcf32': ('47457fba904dcf32c6cba55c79f22da5db0f8cabd2a99585caf204db49b4bc63', [('- Affects approximately 4 million people a year in the U.S.', '- The supplied annual US incidence figure has no verified reference year and is omitted.'), ('Healthcare-Associated Pneumonia:\n- Acquired from other healthcare settings like dialysis centers, outpatient clinics, or nursing homes.', 'Historical Healthcare-Associated Pneumonia Category:\n- Older classifications used this label for exposure to settings such as nursing homes or dialysis. It should not be treated as an automatic current rule for broad-spectrum antibiotic selection; assess individual risk factors and applicable guidelines.')]),
        '532d7f663cb0d73a': ('532d7f663cb0d73a6777a1907ec11890a025c4acbc2ebf267b5f1bcd04725b5b', [('- Every "array" is both an array and a hash, with an additional ordering array.', '- PHP defines an array as an ordered map. This abstraction does not require the literal extra-array implementation implied by the supplied explanation.'), ('- This approach provides both behaviors but incurs overhead most of the time.', '- Implementation costs and performance depend on version, representation and workload; no universal benchmark conclusion is established here.')]),
        '5fc74912249d091b': ('5fc74912249d091b52123ceb6ebc4c231b164a85c1ead91a21f7337deb979073', [('- Pseudohash: An array with a hash glued on for key access.', '- Historical Perl pseudohashes provided array-backed keyed access; the experimental feature was removed in Perl5.10. Do not recommend it as a current built-in option.'), ('- PHP automatically protects users from unexpected issues when using arrays, treating them as associative hashes.', '- PHP arrays are ordered maps; this does not protect a programme from all memory, key-conversion or logic errors.'), ('- Using a hash in Perl avoids memory allocation issues that can occur with arrays.', '- A hash may suit sparse keyed data, but either arrays or hashes can exhaust resources; choose according to access and storage requirements.')]),
        '026f11ff38c8d653': ('026f11ff38c8d6538432c9458c08ba84194e85f2f62a3a10341b06b91511ac28', [('Causes of Misting: Poor air quality, heat, and extreme agitation of cutting fluid.', 'Causes of Misting: Heat and agitation can generate cutting-fluid aerosol; poor air quality is a possible consequence, not a separate aerosol-generation mechanism.'), ('SPIDERCOOL: A system that reduces misting by automatically adjusting to keep coolant precisely targeted at the cutting edge of each tool, not the holders.', 'SPIDERCOOL: The manufacturer claims its targeting system reduces mist from coolant striking tool holders. This review does not independently measure that reduction. Mist-source reduction complements appropriate enclosure, ventilation, exposure assessment and fluid management; the product claim does not establish safe exposure.')]),
        '419bc9320487517c': ('419bc9320487517c71008b5bcfeebba0ce6406639fd9bd30263b97cae0a32f0d', [('School Activity: A large-scale data analysis activity was conducted during a faculty meeting at Spring Lake Middle School.\nActivity Design: The activity involved dividing staff into groups that rotated through five stations, each showcasing different data points.\nData Points: The data included big picture school data, gender/ethnicity group data performance, and other specific data points.\nFacilitators: Teacher leaders, including those in the school administration program and the school improvement team chair, facilitated the activity.\nOutcomes: Teachers discussed and generated insights from the data, such as the impact of male teacher presence on black male students\' success and the importance of student organization.\nPlatform Used: TodaysMeet, a real-time discussion platform, was used to capture and share thoughts, questions, and content.\nReflection: The activity emphasized using data as a reflection tool to answer questions like "What can we do better?" and "What activities/practices do we need to protect or get rid of?"\nStaff Engagement: The activity was well-received, with staff engaging in honest and supportive dialogue.\nFuture Plans: The school plans to continue using such activities and tools to drive improvement and make a difference.', 'Historical school reflection: January19,2013. Reported discussion and enthusiasm are observations, not a causal evaluation of interventions.\n\nSchool Activity: A large-scale data analysis activity was conducted during a faculty meeting at Spring Lake Middle School.\nActivity Design: The activity involved dividing staff into groups that rotated through five stations, each showcasing different data points.\nData Points: The data included big picture school data, gender/ethnicity group data performance, and other specific data points.\nFacilitators: Teacher leaders, including those in the school administration program and the school improvement team chair, facilitated the activity.\nOutcomes: Teachers discussed and generated insights from the data, such as the impact of male teacher presence on black male students\' success and the importance of student organization.\nPlatform Used: TodaysMeet, a real-time discussion platform, was used to capture and share thoughts, questions, and content.\nReflection: The activity emphasized using data as a reflection tool to answer questions like "What can we do better?" and "What activities/practices do we need to protect or get rid of?"\nStaff Engagement: The activity was well-received, with staff engaging in honest and supportive dialogue.\nFuture Plans: The school plans to continue using such activities and tools to drive improvement and make a difference.'), ("such as the impact of male teacher presence on black male students' success", 'such as questions about whether teacher representation relates to student success')]),
        'bf21b1b59b718044': ('bf21b1b59b718044cc6d057990c75acb7b287301192d733ecb75fb0ad00a6840', [('Scan Results: The user has run both McAfee HouseCall and RAV (Rising Anti-Virus) scans, with both reporting no viruses found (0 viruses).', 'Scan Results: The supplied user report describes clean HouseCall and RAV scans. HouseCall is a TrendMicro product; the supplied expansion of RAV is unverified and omitted.'), ('Software Involved: McAfee is the antivirus software causing the issue, with the specific components being Auto VirusScan and HouseCall.', 'Software Context: The alert is attributed to McAfee AutoVirusScan; HouseCall is a separate scanner. An alert plus clean scans does not establish whether a file is malicious, a false positive or a persistent reinfection.')]),
        '1e038cf11aed45e1': ('1e038cf11aed45e165dfe7105ceda3ecd16f7e918afc7e87e63fc61bb6c0e1ce', [("Debate Essence: The 'point of clash' is the core disagreement between two sides in a debate.\nOrigin of Disagreements: All other disagreements in a debate stem from this fundamental clash.\nAnalogy: The 'point of clash' is like the hinge of a door, allowing other arguments to move around it.\nExample Motion: 'This house would make all schools co-educational'\n- Pro Argument: Co-educational schools teach children to relate to the opposite gender better, benefiting both boys and girls.\n- Con Argument: Girls perform better academically in single-sex schools, with more likely to choose science subjects and have women role models.\nPoint of Clash in Example: Social vs academic benefits of education.\nSuccessful Debating:\n- Focus on the point of clash.\n- Build arguments to support your side and expose flaws in the opposing side's arguments.\n- Use the summary to remind judges of the point of clash and how you've won it.\nDifferent Points of Clash: The same motion can generate different points of clash in different debates. In the co-educational schools example, the clash could be between collective benefit vs individual freedom.\nClarifying the Point of Clash: The first speaker's definition of the motion often reveals the point of clash.\nCommon Mistake: Avoid getting distracted by marginal or practical issues unrelated to the point of clash.", "Debate-teaching example: the pro/con positions below are illustrative arguments from the source, not independently established education-research conclusions.\n\nDebate Essence: The 'point of clash' is the core disagreement between two sides in a debate.\nOrigin of Disagreements: All other disagreements in a debate stem from this fundamental clash.\nAnalogy: The 'point of clash' is like the hinge of a door, allowing other arguments to move around it.\nExample Motion: 'This house would make all schools co-educational'\n- Pro Argument: Co-educational schools teach children to relate to the opposite gender better, benefiting both boys and girls.\n- Con Argument: Girls perform better academically in single-sex schools, with more likely to choose science subjects and have women role models.\nPoint of Clash in Example: Social vs academic benefits of education.\nSuccessful Debating:\n- Focus on the point of clash.\n- Build arguments to support your side and expose flaws in the opposing side's arguments.\n- Use the summary to remind judges of the point of clash and how you've won it.\nDifferent Points of Clash: The same motion can generate different points of clash in different debates. In the co-educational schools example, the clash could be between collective benefit vs individual freedom.\nClarifying the Point of Clash: The first speaker's definition of the motion often reveals the point of clash.\nCommon Mistake: Avoid getting distracted by marginal or practical issues unrelated to the point of clash.")]),
        'e19e5e9d5d189164': ('e19e5e9d5d189164c4bdd1de4875c6fa875eec19635fb41f4ef05187f779437e', [('Saltwater pools convert salt into chlorine through electrolysis to sanitize the water and balance pH levels, providing a gentler swimming environment compared to traditional chlorine pools.\nSaltwater pools require between 0.5 to 2 pounds of salt per gallon of water, with the exact amount depending on factors such as pool size and weather conditions.\nTo add salt to a pool, first add fresh water, then test the salinity level, and use a salt table to determine the required amount of salt to reach the desired ppm (parts per million).\nOnly use sodium chloride (NaCl) that is 99% pure for your pool, as other types of salt can cause damage or health issues.\nAdding too much salt to a pool can make the water more acidic, corrode metal fixtures, and cause skin irritations. Signs of excess salt include cloudy water or unpleasant smells.', 'Saltwater Pools: Salinity and Chlorination\n\nA salt chlorinator uses electrolysis to generate disinfectant from dissolved salt. A saltwater pool still requires measured free chlorine and pH control; the chlorinator does not automatically balance pH.\n\nDetermine salt additions from measured existing salinity, pool volume and the specific equipment manual. The supplied0.5–2 pounds per gallon is not a valid general dosing instruction and is removed. As an arithmetic illustration only, raising initially unsalted water to3200mg/L corresponds to about0.0267lb per US gallon, not0.5–2lb. This is not an instruction to add that amount to an existing pool.\n\nUse the salt grade and operating range specified by the equipment manufacturer. Do not infer salinity, acidity or safety from cloudiness, odour or skin irritation alone; test the relevant water parameters.')]),
        '3f3bcdf71612c00d': ('3f3bcdf71612c00d94e6b2b57352389330090ff9cbbd4b3528829b178ca331ef', [('Chlorine Production in Saltwater Pool:\n- Uses a chlorinator that produces chlorine gas from saltwater.\n- Chlorine gas breaks down into salt and dissolves into the water for sanitization.\n\nChlorine Production in Chlorinated Pool:\n- Uses chlorinated tablets or sticks that need to be regularly added to the water.\n- Chlorine gas breaks down into chloride and dissolves into the water.\n\nChlorine Testing:\n- Use a pool test kit at least once a week to maintain proper chlorine levels.\n\nCost Comparison:\n- Saltwater pool installation: $1,500 - $2,000 (plus installation fees) vs. Chlorinated pool: varies.\n- Annual maintenance cost: Saltwater pool - $100 vs. Chlorinated pool - $300 - $800.\n\nMaintenance Comparison:\n- Saltwater pool: Easier and less expensive to maintain due to no need for constant chlorine addition.\n- Chlorinated pool: Requires regular monitoring of chlorine levels and addition of chlorine tablets/sticks.\n\nOdor and Irritation:\n- Saltwater pool: No distinct chlorine smell, less skin and eye irritation.\n- Chlorinated pool: Has a strong chlorine smell, can cause skin and eye irritation.\n\nImpact on Hair and Swimsuits:\n- Saltwater pool: Does not turn hair green or fade swimsuits.\n- Chlorinated pool: Can turn hair green and fade swimsuits due to copper and chlorine interaction.\n\nSaltwater Pool Maintenance:\n- Daily: Skim surface for debris, brush walls and floor to prevent algae growth.\n- Weekly: Backwash filter, check and clean skimmer baskets, test water quality.\n- Monthly: Check and adjust alkalinity (80-120 ppm), check and adjust calcium hardness (180-220 ppm), test salinity (3000-4000 ppm), test stabilizer.', 'Saltwater and Manually Chlorinated Pool Maintenance\n\nSalt chlorinators produce disinfecting chlorine species from dissolved salt. Other pools may receive liquid chlorine or suitable solid products. The useful disinfectant is not described as chlorine gas simply breaking down into salt and then sanitizing the water.\n\nBoth systems require water testing and maintenance. CDC advises testing chlorine and pH at least twice daily, and more often with heavy use; weekly testing alone is insufficient under that guidance. Follow product labels and local requirements.\n\nInstallation and running costs depend on equipment, pool size, electricity, chemistry and maintenance. The supplied universal dollar estimates and guarantees about odour, irritation, hair and swimsuit effects are omitted. Saltwater systems do not eliminate the need to monitor disinfectant and pH.\n\nFilter cleaning, salinity, stabilizer, hardness and alkalinity management should follow the equipment and water-treatment guidance for the actual pool; a universal daily/weekly/monthly schedule or set of target ranges is not established here.')]),
    }
    rule = rules.get(doc_id)
    if rule is None or hashlib.sha256(text.encode("utf-8")).hexdigest() != rule[0]:
        return text
    for old, new in rule[1]:
        if text.count(old) != 1:
            raise ValueError("Unexpected reviewed source text")
        text = text.replace(old, new, 1)
    return text
```

Local validation:27 reviewed originals changed; three batch texts and other470 documents unchanged. Exact IDs/hashes, changed-input and unrelated-ID negative controls and idempotence pass. Sensitive values absent from rules/applied texts; only masked original displays added to audit.353 unique CSV IDs, exact header, single-line notes, verdict/code agreement and ordered evidence segments pass.45x20min=15h and36x20min=12h; illustrative3200mg/L salt conversion is0.0267lb per US gallon, not a prescription for a pool. No original pack mutation or full-pack cleaning.

Medical and pool corrections are bounded independent editorial changes, not restoration of unavailable originals, individualized dosing, equipment certification or measurements. Current operations/policies and unverified source-derived history are not certified. The adult-CAP guideline explicitly abandoning HCAP categorization is [the2019ATS/IDSA executive summary](https://www.idsociety.org/globalassets/idsa/practice-guidelines/community-acquired-pneumonia-in-adults/executive_summary.pdf).

Harm/gold tests: pending Data A / Data B; no gold set supplied. Exact source statuses, references and before/after hashes recorded per row locally.


## Pre-push format/evidence corrections —31October2026

Invalid broad B on95 is now B5 for malformed LaTeX. Invalid broad A on148 is withdrawn: isolated OCR junk is an editorial repair, not a demonstrated A-series encoding disease.124 now uses C5;C1 with separate ordered evidence. Missing evidence is supplied from original text for22,95,111–113,121–124,142–143.121–123 continuation classifications remain explicitly provisional; no strict truncated-sentence proof is claimed. Gold tests remain pending and no test/data files are added.
