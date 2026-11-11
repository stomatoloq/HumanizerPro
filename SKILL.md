---
name: stylometric-de-ai-rewriter
description: Sanitize multi-vendor AI provenance marks (invisible Unicode, exotic spaces, LLM citation/UTM artifacts, C2PA/EXIF/XMP/OOXML metadata) and rewrite robotic AI prose into natural human writing across all languages and genres using a 6-pass stylometric protocol. Use whenever the user asks to humanize text, remove AI tells, strip watermarks, clean AI metadata, increase burstiness, or run /stylometric-de-ai-rewriter or /remove-ai-marks in any language.
---

# Stylometric De-AI and Anti-Provenance Rewriter

Clean technical AI provenance artifacts (Layer A) and edit out statistical, syntactic, and rhetorical LLM habits (Layer B) across any language or document genre purely through native LLM analysis and regeneration while strictly preserving source facts.

## Core Safeguards

1. **Source Fidelity First:** Never invent facts, numbers, dates, named sources, citations, or physical details that do not appear in the source text (fiction is exempt). Keep epistemic hedges (*may, might, suggests, is associated with, ehtimal olunur, может*) whenever they reflect genuine empirical uncertainty, clinical bounds, or legal caveats in the source; only cut performative throat-clearing (*"it is worth noting that"*, *"qeyd etmək lazımdır ki"*).
2. **Genre and Register Gate:** Classify the input before editing. Never force essay rules onto non-essay formats:

   - **Emails, messages, and thread replies:** Keep greetings, sign-offs, and actionable schedule or task bullets. Lead directly with the decision, question, or request instead of re-explaining background the recipient already knows (`#26`).
   - **Academic abstracts and executive summaries:** Preserve required structural moves (objective, method, results, conclusion) and empirical confidence bounds.
   - **Technical documentation, READMEs, legal clauses, and checklists:** Keep numbered steps, bullet lists, tables, code blocks, inline identifiers, paths, and exact defined terms. Do not cycle synonyms for technical terminology.
   - **Short texts (under 120 words or fewer than 5 sentences):** Skip long-range horizon anchors and 300-word window targets; focus on stripping chatbot wrappers, negative parallelisms, and inflated verbs.

3. **Universal Morphosyntactic Scaling:** Sentence-length variation adapts to the morphological typology of the target language:

   - **Analytic and moderately inflected languages (English, Spanish, French, Italian, Portuguese):** Alternate sharply between short (3 to 7 words) and complex (35 to 55 words) sentences so word-length dispersion stays high (`CV >= 0.50` on texts of 5+ sentences).
   - **Agglutinative, polysynthetic, and heavily inflected languages (Azerbaijani, Turkish, Kazakh, Uzbek, Finnish, Hungarian, Korean, Japanese, Russian, German, Arabic):** Suffixation and case inflection pack entire clauses into fewer word tokens. Alternate compact 3-to-5-word finite clauses with 22-to-35-word multi-clause sentences (`CV >= 0.45`) and respect native constituent order, such as SOV topic-focus scrambling in Turkic and free constituent fronting in Slavic.
   - **Isolating and logographic scripts (Mandarin Chinese, Cantonese, Japanese Kanji/Kana, Thai, Khmer):** Vary clause character counts sharply across punctuation boundaries rather than counting whitespace words.

## Tier 1: Layer A - Multi-Vendor Technical Provenance Hygiene

During token regeneration, emit clean standard characters and strip mechanical markers emitted across all major LLM platforms (OpenAI/ChatGPT, Anthropic/Claude, Google/Gemini, DeepSeek, Qwen, Perplexity, Meta/Llama, Mistral, Cohere, Grok, and Copilot):

- **Unconditional Invisible Codepoint Strip:** Omit zero-width spaces (`\u200B`), word joiners (`\u2060`), soft hyphens (`\u00AD`), byte-order marks (`\uFEFF`), combining grapheme joiners (`\u034F`), Arabic letter marks (`\u061C`), Mongolian free variation selectors (`\u180B` to `\u180E`), directional Bidi overrides and isolates (`\u200E` to `\u200F`, `\u202A` to `\u202E`, `\u2066` to `\u2069`), invisible math operators (`\u2061` to `\u2064`), interlinear annotation anchors (`\uFFF9` to `\uFFFB`), Plane 14 Unicode Tag characters (`\uE0000` to `\uE007F`), and steganographic Variation Selectors Supplement (`\uE0100` to `\uE01EF`).
- **Conditional Joiner Preservation:** Preserve `\u200D` (ZWJ) inside multi-codepoint emoji sequences, and preserve `\u200C` (ZWNJ) and `\u200D` (ZWJ) when adjacent to scripts where they are orthographically required (Arabic, Persian, Kurdish, Urdu, Devanagari, Bengali, Gurmukhi, Gujarati, Oriya, Tamil, Telugu, Kannada, Malayalam, Sinhala, Thai, Lao, Tibetan, Myanmar, Khmer). Strip them only when wedged between Latin, Cyrillic, or Greek letters.
- **Exotic Whitespace and Punctuation Normalization:** Replace narrow no-break spaces (`\u202F`, frequent in GPT tokenizers), non-breaking spaces (`\u00A0`), ideographic spaces (`\u3000` outside CJK), and quad/thin/hair spaces (`\u2000` to `\u200A`, `\u205F`) with standard ASCII spaces (`0x20`). Replace fullwidth CJK punctuation (`，`, `：`, `；`, `（`, `）`) leaked inside Latin, Cyrillic, or Turkic sentences (a frequent DeepSeek and Qwen artifact) with standard ASCII punctuation.
- **Multi-Vendor UI and Citation Artifact Removal:** Strip OpenAI/ChatGPT citation brackets (`【4:0†source】`, `:contentReference[oaicite:0]{index=0}`), unanchored Perplexity/Copilot footnote markers, and LLM tracking parameters (`?utm_source=chatgpt.com`, `utm_source=claude`, `utm_source=gemini`, `utm_source=perplexity`, `utm_source=copilot`) from URLs.
- **Canonical NFC and Non-Destructive Mixed-Script Flagging:** Emit standard canonical `NFC` characters (never apply blind confusable replacement tables that corrupt math symbols or legitimate Russian, Ukrainian, or bilingual Azerbaijani text). Inspect input words and flag any single word token that mixes Latin, Cyrillic, and Greek characters (e.g., a Cyrillic `о` inside a Latin word) in the audit summary while outputting the corrected pure-script word in the rewrite.
- **Container and File Metadata Guidance (When Asked About Files):** If the user asks how to strip metadata from binary or markup files locally, provide exact commands:

  - **PDF (`.pdf`):** `exiftool -all:all= input.pdf -o temp.pdf && qpdf --linearize temp.pdf cleaned.pdf`
  - **Office OpenXML and ODF (`.docx`, `.pptx`, `.xlsx`, `.odt`):** Clear generator tags in `docProps/core.xml` and `docProps/app.xml`, or run `exiftool -all= document.docx`
  - **Images and Media (`.png`, `.jpg`, `.webp`, `.avif`, `.mp4`):** Strip C2PA `JUMBF` boxes, EXIF tags, XMP `trainedAlgorithmicMedia`, and PNG `tEXt`/`iTXt` chunks via `exiftool -all= image.png` or `c2patool`
  - **Markup (`.md`, `.html`, `.tex`, `.svg`):** Strip generator YAML keys, HTML provenance comments, SVG `<metadata>`, and LaTeX `\pdfinfo` or `\hypersetup{pdfproducer=...}` fields.

## Tier 2: Layer B - 6-Pass Stylometric Editing Pipeline

Work top-down from macro-structure to lexical choice in your internal reasoning before emitting the final text. Each pass explains why the pattern appears in model output so you can apply editorial judgment rather than mechanical substitution.

### Pass 1: Macro-Discourse and Epistemic Alignment

Addresses signatures `#4, #21, #22, #25, #27, #28, #29, #30, #31, #32`.

1. **Cut Staged Run-Ups and Panoramic Openers (`#25`):** Models minimize initial token uncertainty by opening with broad historical sweeps (*"In today's rapidly evolving landscape..."*, *"Müasir dövrdə sürətlə inkişaf edən..."*, *"В современном стремительно меняющемся мире..."*) or conversational throat-clearing (*"Let's dive in"*, *"Here's what you need to know"*). Cut the warm-up and start with the specific claim, datum, or tension.
2. **Cut Moralizing and Pedagogical Codas (`#22`):** Post-trained models resist stopping on a concrete fact, appending uplifting send-offs (*"Ultimately, this paves the way for a brighter future..."*). In essays, reports, and articles, stop on the last substantive point.
3. **Break Paragraph Sandwich Symmetry (`#21`):** Models often restate a paragraph's opening sentence in its final line. Cut the redundant closer so each paragraph advances the argument.
4. **Trim Obvious Deductive Bridges (`#29`) and Re-Order Existing Details (`#4`):** When a causal chain `A -> B -> C` spells out an obvious intermediate step that the target reader already knows, cut `B`. In longer prose, pull a concrete detail that already exists later in the draft into the opening paragraph to create forward cohesion without inventing facts.
5. **Calibrate Epistemic Hedging (`#27, #32`):** RLHF rewards non-committal "both-sides" framing and stacked modal qualifiers (*"could potentially arguably"*). Strip performative buffers, but preserve source-backed uncertainty, clinical bounds, and genuine mid-sentence qualifications.
6. **Ground Claims in Source Specifics (`#28, #30, #31`):** Replace generic observer labels (*"profoundly impactful"*, *"deeply troubling"*) with the concrete mechanisms, numbers, or constraints already present in the source. When two facts stand in tension or irony, place them side by side without explaining the subtext.

### Pass 2: Typographic and Visual Scaffolding

Addresses signatures `#33, #34, #35, #36, #37, #38`.

1. **Dissolve Decorative Bulletization (`#36`) and Bold-Colon Labels (`#34`):** RLHF raters favor scannable `**[Noun Phrase]:** [Explanation]` bullets even in short prose. Unless the genre requires a list (emails with schedule options, READMEs, checklists), fold list items back into continuous prose and make the label the grammatical subject.
2. **Vary Paragraph Block Weight (`#35`):** Break uniform 4-sentence rectangular paragraphs by pairing fuller analytical paragraphs with occasional compact 1-to-2-sentence pivots.
3. **Replace Habitual Clause Dashes (`#33`) and Diversify Punctuation (`#37`):** Models overuse `em-dash (\u2014)` and spaced hyphens as a universal clause splice because dashes avoid specifying how two clauses relate. Replace most dashes with periods, commas, colons, parentheses, or semicolons so logical relationships are explicit.
4. **Remove Apologetic Scare Quotes (`#38`):** Drop quotation marks around ordinary metaphors or colloquialisms unless citing a source or signaling irony. Ensure quotation marks match the target language convention (`"..."`, `«...»`, or `„..."`).

### Pass 3: Rhetorical Templates and Transitions

Addresses signatures `#11, #20, #23, #24, #26`.

1. **Collapse Negative Parallelisms (`#20`):** Formulas like *"It is not merely X, but Y"*, *"sadəcə X deyil, həm də Y"*, *"sadece X değil, aynı zamanda Y"*, *"не просто X, а Y"*, and *"不仅是X，更是Y"* invent a strawman (`X`) to make `Y` sound profound. State `Y` directly unless correcting a real misconception held by the reader.
2. **Unbalance the Concession Seesaw (`#24`):** Avoid repeating `"While [X], [Y]"` across paragraphs. State real limitations directly without immediately rescuing them in the same sentence.
3. **Prune Forced Triads (`#11`):** Models default to three-part coordinate lists (`A, B, and C`) as a rhythmic halting rule. Keep three items when the source has three distinct facts; otherwise trim filler items to a single exact term or a two-part phrase.
4. **Replace Polar Merisms (`#26`) and Heavy Discourse Glue (`#23`):** Cut decorative range brackets (*"from solo clinics to global hospitals"*, *"from novices to seasoned experts"*) and mechanical sentence-initial conjunctive adverbs (*Moreover, Furthermore, Additionally, Bundan əlavə, Bununla belə, Кроме того, Более того*) when adjacent sentences already connect via shared referents or clear cause and effect.

### Pass 4: Syntactic Tree Variance and Burstiness

Addresses signatures `#2, #3, #7, #8, #9, #10, #12`.

1. **Cut Shallow Trailing Participles and Converbs (`#10`):** Models tack interpretive tails onto factual clauses (*", highlighting the importance of..."*, *", ensuring seamless..."*, stacked Turkic `"-araq/-ərək"` or `"-ıb/-ib"` converbs, and Slavic adverbial participle tails). Delete tails that only assert significance; convert genuine causal outcomes into finite main verbs.
2. **Vary Sentence Length and Parse Trees (`#2, #3, #7, #8, #9, #12`):** Autoregressive decoding keeps sentence length clustered around a flat mean with repetitive right-branching openings. Mix compact, high-impact sentences with longer left-branching or compound sentences, and use natural constituent fronting (including SOV topic-focus shifts in agglutinative languages) so adjacent sentences never clone the same grammatical skeleton.

### Pass 5: Lexical Precision and Collocation De-Smoothing

Addresses signatures `#1, #5, #6, #13, #14, #15, #16, #17, #18, #19`.

1. **Restore Direct Copulas and Active Verbs (`#13, #15`):** Replace inflated relational verbs (*serves as, stands as, boasts, features, represents, rolunu oynayır, kimi çıxış edir, təcəssüm etdirir, является, представляет собой*) and dense nominalizations (*"the implementation of X facilitates"*) with direct copulas (*is, are, has, -dır/-dir*) or concrete finite verbs.
2. **Cut Promotional Puffery (`#14`), Dead Metaphors (`#16`), and Weasel Citations (`#19`):** Remove stock AI words (*tapestry, delve, landscape, beacon, cornerstone, catalyst, crucible, pivotal, crucial, testament, vibrant, seamless, robust, foster, harness, unlock*) and anonymous collective authorities (*"experts argue"*, *"observers note"*, *"mütəxəssislər qeyd edir ki"*). Keep the underlying fact.
3. **Prefer Concrete Hyponyms (`#17`) and Consistent Terms (`#18`):** Use the specific nouns from the source instead of vague category labels (*"regulatory frameworks"*, *"stakeholder dynamics"*), and repeat technical terms cleanly instead of cycling through awkward synonyms (*"the aforementioned methodology"*).
4. **Unstick High-PMI Collocations (`#1, #5, #6`):** Drop cliché adjective-noun pairings (*meticulously crafted, stark contrast, dərin iz, sarsılmaz təməl*). Use domain-exact vocabulary supported by the source.

### Pass 6: Self-Audit Verification Gate

Before outputting the final text, verify these four criteria directly against your draft:

- **Sentence Length Range:** On texts of 5+ sentences, confirm the rewrite includes both a compact sentence (`<= 7` words in analytic languages; `<= 5` words in agglutinative languages) and a longer multi-clause sentence (`>= 32` words in analytic; `>= 22` words in agglutinative), breaking uniform sentence length.
- **Dash Count:** Confirm clause-level dashes do not exceed `1 per 400 words` unless matching a user's reference voice sample.
- **Punctuation Variety:** On texts over 150 words, use at least 3 distinct non-terminal punctuation marks (from commas, semicolons, colons, parentheses, or question marks).
- **Structural Tells:** Confirm `0` unmotivated `"Not X, but Y"` strawmen, `0` decorative `**[Bold Label]:**` prose headers, and `0` trailing `", highlighting/ensuring/fostering"` participial riders.

## Output Modes

- **Default Mode:** Output the final sanitized and rewritten text first, followed by a compact **Anti-Provenance & Stylometric Audit** table summarizing Layer A artifacts stripped (such as UTM links, citation brackets, mixed-script homoglyphs, or exotic spaces) and Pass 6 structural verification (shortest/longest sentence word counts, dash count, and key signatures removed).
- **Clean / Text-Only Mode:** When the user asks for "only the text" or invokes the skill inside an automated pipeline, output strictly the final rewritten text with no commentary.