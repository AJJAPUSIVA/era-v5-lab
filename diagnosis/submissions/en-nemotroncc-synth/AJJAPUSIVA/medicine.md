# Medicine sheet: en-nemotroncc-synth (AJJAPUSIVA)

All 500 source documents have full-read labels: **401 clean, 93 fix and 6 drop**. Each label includes the document ID, verdict, disease evidence and a concise review note. Source checks and factual corrections are distinguished from the repository's disease taxonomy.

| Code | Label occurrences |
|---|---:|
| B4 | 2 |
| B5 | 1 |
| C1 | 48 |
| C3 | 34 |
| C4 | 7 |
| C5 | 2 |
| F1 | 7 |
| F3 | 3 |

Counts are document occurrences; a document can have multiple codes. A clean label means no listed disease was identified, rather than a guarantee that every source claim is correct.

The candidates below cover the four codes occurring more than five times: C1, C3, C4 and F1. Exact source-ID and SHA-256 guards keep document-specific operations confined to reviewed examples. The original Parquet and local comparison artifacts are outside this submission.

## Validation

- 500 unique document IDs match all 500 source records; every read value is `full`.
- Required CSV columns, single-line notes and verdict/code consistency checked.
- Candidate positive cases, changed-input and unrelated-ID controls, idempotence, split reconstruction and Q&A preservation checked.
- The source Parquet checksum is unchanged. No cleaned full-pack export is included.
- The independent modern Swift example and the six blackjack settlement cases were checked locally; RPG fragments were checked against documentation.

**Gold-set evaluation:** the Data A / Data B English reference set is not available in the checkout, so gold changed/drop counts and the repository's 1% result cannot be reported. Local checks below are sample checks, not gold-set results. Exact-ID candidates do not establish generalization to unseen documents.

## Shared interfaces

Single-document candidates accept `(doc_id, text)` and return text; `None` means drop. The contextual C3 operation returns a derived body with provenance and separate Q&A, or `None` for no pair operation. It does not delete either source record.

```python
import hashlib

def source_hash(text):
    return hashlib.sha256(text.encode("utf-8")).hexdigest()

def harm_test(medicine, gold_records):
    total = changed = dropped = 0
    for record in gold_records:
        original = record["text"]
        result = medicine(record["doc_id"], original)
        if result is not None and not isinstance(result, str):
            raise TypeError("Expected text or None")
        total += 1
        dropped += result is None
        changed += result is not None and result != original
    if total == 0:
        raise ValueError("Gold set must contain records")
    affected = changed + dropped
    return {"documents": total, "changed": changed, "dropped": dropped,
            "affected_percent": 100 * affected / total,
            "passes_one_percent_threshold": 100 * affected <= total}
```

## Medicine 1 — C1: reviewed boilerplate spans

1. **Disease:** C1, 48 labelled occurrences. This candidate covers three exact spans:
   - `3cd12d535137ab2f`: `The Awesomer is reader-supported. When you buy through links on our si` at offset 0.
   - `701cacaccdb06931`: `Citation Disclaimer: These citations have been automatically generated` at offset 45.
   - `76b4036d49000599`: `Enter your email address to follow this blog and receive notifications` at offset 2211.
2. **Medicine:** remove the reviewed span only when its document ID and source hash match.
3. **Action:** fix; retain the remaining article text.
4. **Harm test:** use `harm_test(medicine_c1, gold_records)`; gold-set status is recorded above. Local checks changed exactly these three originals and left changed inputs and unrelated IDs unchanged.
5. **Side effects:** a disclaimer or subscription passage can be substantive in another context. These offsets must not be generalized to other documents.

```python
def medicine_c1(doc_id, text):
    spans = {'3cd12d535137ab2f': ('3cd12d535137ab2f29d7b8a38461d7c3e35610d67581e5e57df0999e49918a80', 0, 180), '701cacaccdb06931': ('701cacaccdb06931da002988266d4a60dc9f4b9152b3a2462d8d8ac262dd0939', 45, 287), '76b4036d49000599': ('76b4036d49000599952ad548de4cdee33afdeb27b5df3e12b44067c012a1b9be', 2211, 2304)}
    rule = spans.get(doc_id)
    if rule is None or source_hash(text) != rule[0]:
        return text
    return text[:rule[1]] + text[rule[2]:]
```

## Medicine 2 — C3: contextual boundary reconstruction

1. **Disease:** C3, 34 labelled occurrences. Three sentence/verse continuation pairs:
   - `a147d6925833c136` → `fe0100ca641983cc`: “great was his” → “desire to win”.
   - `ee6655c72b3799da` → `89a8e620955b1040`: “Thus to behold the day” → “Which sees the beauteous Kriemhild”.
   - `84649d75dd11ae14` → `bca8eefdcfb28646`: “over the grassy” → “plain”.
2. **Medicine:** join the two reviewed article bodies with a separator, retaining both source IDs and each generated Q&A separately. Other section-level and internal-omission classifications remain identified by their evidence in the label sheet; this candidate covers only the explicit continuation pairs above.
3. **Action:** create a contextual reconstruction; retain the original records.
4. **Harm test:** requires contextual gold pairs. Count both source records as affected when a pair is reconstructed; the operation drops zero source records. Gold-set status is recorded above. Local checks preserve both bodies and both Q&A exactly apart from the inserted separator.
5. **Side effects:** incorrect adjacency can merge unrelated text. Matching hashes do not establish that every neighbouring document should be joined; use only the listed pair identities.

```python
def medicine_c3_pair(left_id, left, right_id, right):
    pairs = {('a147d6925833c136', 'fe0100ca641983cc'): ('a147d6925833c1363b68666dd55c988c133223eced68d43cacc02d70fb0691e0', 'fe0100ca641983ccb5cc98a67b4f0095934c3da502a4793cebc0be7051b7a224'), ('ee6655c72b3799da', '89a8e620955b1040'): ('ee6655c72b3799dacd976bf36a2a009c70cb5cefdd0c4b4396f78f6a16f558bf', '89a8e620955b10402627a2686535abdf7d821d542c0c0f52a6f37b37b766522d'), ('84649d75dd11ae14', 'bca8eefdcfb28646'): ('84649d75dd11ae14397b9cda227514f8de584f19c092a106ed119dd030373536', 'bca8eefdcfb2864690c54dd866aad2d3194135c99dbca1dab1eba7f3e79f0c2f')}
    expected = pairs.get((left_id, right_id))
    if expected != (source_hash(left), source_hash(right)):
        return None
    left_body, left_marker, left_qa = left.partition("\n\nQuestion:")
    right_body, right_marker, right_qa = right.partition("\n\nQuestion:")
    return {"source_ids": (left_id, right_id),
            "body": left_body + " " + right_body,
            "source_qa": (left_marker + left_qa, right_marker + right_qa)}

def harm_test_pairs(medicine, gold_pairs):
    total = changed = 0
    for left, right in gold_pairs:
        total += 2
        result = medicine(left["doc_id"], left["text"],
                          right["doc_id"], right["text"])
        changed += 2 if result is not None else 0
    if total == 0:
        raise ValueError("Gold pairs must contain records")
    return {"documents": total, "changed": changed, "dropped": 0,
            "affected_percent": 100 * changed / total,
            "passes_one_percent_threshold": 100 * changed <= total}
```

## Medicine 3 — C4: separate reviewed topic blocks

1. **Disease:** C4, 7 labelled occurrences. Three reviewed examples:
   - `a14eaa5e58baf3c3`: topic shifts at offsets 188, 338 and 409. First boundary text: `Australia's English has distinct accents and vocabulary`.
   - `5958fb6996158153`: topic shifts at offsets 386, 689, 897 and 1270. First boundary text: `Disruptive technologies span from the automobile to the`.
   - `6cd78f812660687b`: a separate vehicle question starts at offset 1650. First boundary text: `i have a 2003 chev s-10 pickup with automatic/ 4 cyl I `.
2. **Medicine:** split at the reviewed, zero-based character boundaries without changing or discarding any source character. Boundaries are explicit review decisions, rather than recovered original page metadata.
3. **Action:** emit separate fragments with source ID, fragment index and original bounds.
4. **Harm test:** use `harm_test(c4_harm_adapter, gold_records)`; gold-set status is recorded above. Local reconstruction checks concatenate every fragment back to its exact original. A changed fragment count affects one source document; no source document is dropped.
5. **Side effects:** a topic shift can be intentional. Apply only to the three source hashes; do not split an article solely because it covers several subjects.

```python
def medicine_c4_split(doc_id, text):
    rules = {'a14eaa5e58baf3c3': ('a14eaa5e58baf3c30dcd36d4155a9170f88417fee5af4636c8ae212fb04c4624', [188, 338, 409]), '5958fb6996158153': ('5958fb699615815368c5862162fed173c7e5d92fb0760dbd2115ca3e18edbe51', [386, 689, 897, 1270]), '6cd78f812660687b': ('6cd78f812660687b04cb5ab5131b8c53fbf7a6e7771c3d97f05aab4e579f96d6', [1650])}
    rule = rules.get(doc_id)
    if rule is None or source_hash(text) != rule[0]:
        return [{"source_id": doc_id, "fragment": 1,
                 "start": 0, "end": len(text), "text": text}]
    bounds = [0] + rule[1] + [len(text)]
    return [{"source_id": doc_id, "fragment": index,
             "start": start, "end": end, "text": text[start:end]}
            for index, (start, end) in enumerate(zip(bounds, bounds[1:]), 1)]

def c4_harm_adapter(doc_id, text):
    parts = medicine_c4_split(doc_id, text)
    return text if len(parts) == 1 else "\n\n".join(p["text"] for p in parts)
```

## Medicine 4 — F1: remove private contact/testimonial material

1. **Disease:** F1, 7 labelled occurrences. Values are excluded from evidence and code:
   - `12693fe362d15d0d`: contact name at offset 1087; phone at offset 1110.
   - `d75b731d61eb6433`: customer name at offset 858.
   - `6700f6a8840e9c51`: contact name at offset 1201; email at offset 1215.
2. **Medicine:** return the reviewed privacy-safe replacement only for a matching source ID and hash. The lookup contains output text and hashes, never the removed personal values.
3. **Action:** fix. Named contact/testimonial material is removed; accompanying historical or factual qualifications are retained in the reviewed output.
4. **Harm test:** use `harm_test(medicine_f1, gold_records)`; gold-set status is recorded above. Local checks confirm three replacements, idempotence and unchanged altered inputs/unrelated IDs.
5. **Side effects:** removing an entire contact or testimonial passage loses surrounding context. Public professional attribution is not automatically private personal data; do not generalize this decision to every name.

```python
def medicine_f1(doc_id, text):
    reviewed = {'12693fe362d15d0d': ('12693fe362d15d0d98db6692a8a1f47d34481adf85677a5bf359aab419e51dca', 'Mindfulness and Self-Compassion Workshop\n\nThe Father Mychal Judge Center for Recovery presents a two-part workshop, "Introduction to Mindfulness and Self-Compassion," as part of its Mind Body Spirit Series. Led by Mark Dagnall, LADAC, CRC, this workshop is designed for individuals in recovery and their caregivers, aiming to provide a general understanding of mindfulness and self-compassion for overall well-being and recovery.\n\nThe workshop focuses on teaching meditation techniques, both formal and informal, to help participants develop non-judgmental awareness of the present moment. This is an excellent opportunity to explore and enjoy a mindfulness practice in a comfortable, loose-fitting attire.\n\nThe workshop is held over two sessions:\n- Session 1: Wednesday, March 19, 2014, from 5:30 PM to 7:30 PM\n- Session 2: Wednesday, March 26, 2014, from 5:30 PM to 7:30 PM\n\nBoth sessions take place in the Clare Conference Room on the 4th floor of St. Anthony Shrine. The cost is $20 per session or $35 for both sessions.'), 'd75b731d61eb6433': ('d75b731d61eb64339140e502e6399133daccf255e1db55799a97b537ce98bef7', 'Purpose: A Notion database template for benchmarking analysis to help solopreneurs, entrepreneurs, startups, and SMEs track performance and identify improvement areas.\nCreator\'s Motivation: "What cannot be measured cannot be improved."\nBenefits:\n- Identify improvement areas in the company/project.\n- Learn from the competition.\n- Monitor company/project performance.\n- Identify productivity points.\nTarget Users:\n- Entrepreneurs, startups, and SMEs.\n- Professionals in finance, marketing, sales, operations, human resources, or internationalization departments.\nFeatures:\n- Over 600 indicators and metrics for benchmarking.\n- Onboarding guide and creation hub for customizing metrics.\n- Introductory guide to benchmarking and use cases.'), '6700f6a8840e9c51': ('6700f6a8840e9c5116d5f8cb805209b7e0c158d012717164e872d5dd93657a3b', "Transition Process:\n- Involves stopping harvesting in one area and preparing for the new area within 24-56 hours.\n- Requires coordination among multiple departments: drivers, sanitation, food safety, maintenance, etc.\n- Happens twice a year for many row-crop growers.\n- Logistically challenging due to simultaneous moves and varied customer needs.\n\nD'Arrigo Brothers:\n- First grower to ship fresh produce from the West Coast to the East Coast (1926).\n- Moves entire growing operations, including equipment and staff, from Yuma, Arizona to Salinas, California annually.\n- Equipment moved: 34 buses, 70 harvesters, 30 forklifts and tractors, 12 storage trailers, and 20 heavy trucks.\n- Transition process spans 1.5 months.\n\nPRO*ACT's Role:\n- Assists suppliers and customers with communication during the transition process.\n- Helps D'Arrigo Brothers communicate with the entire supply chain about product timing, availability, and quality.\n\n2021 Transition Dates:\n- Spring move starts on April 5, 2021.\n- Suppliers loading in Salinas on April 5: Mann Packing, Duda, Church Brothers, Nunes, Salad Farms.\n- Later transitions: Ippolito (April 12), Taylor Farms (April 19), Greengate (April 26).")}
    entry = reviewed.get(doc_id)
    if entry is None or source_hash(text) != entry[0]:
        return text
    return entry[1]
```

## Additional bounded decisions

- B4 (2): the billing placeholder (`d2fb665c40af5edb`, `Total Due After -DATE-:` at offset 326) and embedded writing-template directions have document-specific evidence. The initial billing-heading candidate follows below.
- B5 (1): malformed sigma commands receive a source-specific mathematical-text repair.
- C5 (2) and F3 (3): the labelled exclusions remain exact-record decisions. C1 also has one recommendation-only record marked drop. Excluding an exact listed record is distinct from a general topic filter.
- Source retrieval limits and separate factual corrections are recorded in individual label notes. Religious, gambling, cannabis, medical and literary subject matter alone is not treated as a harmful-content code.

```python
def medicine_b4(text):
    old = "Total Due After -DATE-:"
    return text.replace(old, "Total Due After the Due Date:", 1) if text.count(old) == 1 else text

def reviewed_drop(doc_id, text):
    exclusions = {'67cd7f8dca56480d': '67cd7f8dca56480d8edd3d67e9d7e2151041478396ff01c8d72cc985acdd4bdd', 'ef65e5b6d0cda5a5': 'ef65e5b6d0cda5a55c4562ab524991044b2ffda7fbe62057e6bda9f5364ebdab', 'f27860217811cf33': 'f27860217811cf3306b4d8e5c37345587d6252e1a4fd3826d995e47190fd40cb', 'ce52c449f250761e': 'ce52c449f250761e9889951b8c51f26d34fc5135bcd20300261b6169943df709', 'beb73cd7cb9e50fd': 'beb73cd7cb9e50fda82be15af9d6453327cb697656d1b9c499dd6642c7d1593c', '06cfe5719541e863': '06cfe5719541e863bed260475ed925693ed9a8f4ece5161b3c5b58c8ad3ffaf8'}
    return None if exclusions.get(doc_id) == source_hash(text) else text
```

The B4 heading candidate can change intentional quoted templates; repeated headings are left unchanged. The exclusion candidate removes the whole reviewed record, including generated Q&A. Both require the same gold-set evaluation before general use.

## Selected source references

- [The Nibelungs, matching Upton edition](https://www.gutenberg.org/cache/epub/73227/pg73227-images.html): literary transcription and continuation checks.
- [IJSR publisher PDF](https://www.ijsr.net/archive/v8i3/ART20195931.pdf): the work-immersion study and its methods.
- [Swift official documentation](https://raw.githubusercontent.com/swiftlang/swift-book/main/TSPL.docc/LanguageGuide/TheBasics.md): optional values and unwrapping.
- [IBM READE reference](https://www.ibm.com/docs/en/i/7.4.0?topic=codes-reade-read-equal-key): keyed-record processing.
- [AAP newborn assessment](https://www.aap.org/en/patient-care/newborn-infant-and-early-childhood-nutrition/newborn-and-infant-health-assessment-and-promotion/first-office-visit-3-5-days/): feeding and weight evaluation.
- [IOC REDs consensus](https://doi.org/10.1136/bjsports-2023-106994): athlete-health terminology.
- [Kansas published blackjack rules](https://sos.ks.gov/publications/Register/Volume-41/Issues/Issue-24/06-16-22-50240.html): insurance stake and payout context.
- [Women's Sports Foundation 2016 record](https://www.womenssportsfoundation.org/education/title-ix-and-the-rise-of-female-athletes-in-america/): participation-statistic correction.
- [U.S. Copyright Office NFT study](https://copyright.gov/policy/nft-study/): token ownership and intellectual-property distinctions.

Source checks distinguish retrieved originals, matching replacement pages, independent references and unavailable pages. The linked references support specific corrections; they are not a claim that every original URL was recovered.
