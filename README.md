# Stylometric De-AI & Anti-Provenance Rewriter

![HumanizerPro Logo](docs/logo.png)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-emerald.svg)](SKILL.md)
[![Languages: 13+ Universal](https://img.shields.io/badge/Examples-13%20Languages-purple.svg)](docs/examples.html)

**`stylometric-de-ai-rewriter`** is a universal, zero-dependency LLM skill designed to sanitize multi-vendor AI provenance artifacts (**Layer A**) and transform robotic AI-generated drafts into natural human writing across all languages and genres using a deterministic **6-Pass Stylometric Editing Protocol** (**Layer B**).

### Creator: [Dr. Fərid Zeynalov](https://stomatoloq.az) (Baku, Azerbaijan).
---

## 📖 Web Documentation & Interactive Guides

Full standalone HTML documentation, visual rule cards, and multilingual before/after comparisons are located in the `docs/` directory:

- 🌐 **[Extended Web Documentation (`docs/readme.html`)](docs/readme.html)** — Full system architecture, Core Safeguards, Layer A provenance sanitization reference, local file metadata commands, and output modes.
- ⚡ **[The 6 Core Rules (`docs/rules.html`)](docs/rules.html)** — Deep-dive visual cards for each of the 6 stylometric passes neutralizing 38 universal LLM signatures.
- 🌍 **[13-Language Before & After Showcase (`docs/examples.html`)](docs/examples.html)** — Flag-indexed interactive transformations across **English, Azerbaijani, Turkish, Mandarin Chinese, Hindi, Spanish, German, Modern Standard Arabic, Portuguese, Russian, Japanese, French, and Indonesian**.

---

## 🎯 Core Safeguards

1. **Source Fidelity First:** Never invents facts, numbers, dates, named sources, citations, or physical details absent from the source text (fiction is exempt). Preserves genuine empirical, clinical, or legal epistemic hedges (*may, might, suggests, is associated with*) while stripping performative throat-clearing (*"it is worth noting that"*).
2. **Genre and Register Gate:** Classifies the input before editing so essay rules are never forced onto non-essay formats:
   - **Emails, messages, and thread replies:** Preserves greetings, sign-offs, and actionable task/schedule bullets; leads directly with the decision, question, or request (`#26`).
   - **Academic abstracts and executive summaries:** Preserves required structural moves (objective, method, results, conclusion) and empirical confidence bounds.
   - **Technical documentation, READMEs, legal clauses, and checklists:** Keeps numbered steps, bullet lists, tables, code blocks, inline identifiers, paths, and exact defined terms.
   - **Short texts (< 120 words or < 5 sentences):** Skips long-range horizon anchors and 300-word window targets; focuses on stripping chatbot wrappers, negative parallelisms, and inflated verbs.
3. **Universal Morphosyntactic Scaling:** Adapts sentence-length dispersion targets to the morphological typology of the target language:
   - **Analytic & moderately inflected languages (English, Spanish, French, Italian, Portuguese):** Alternates between short (3–7 words) and complex (35–55 words) sentences (`CV >= 0.50` on texts of 5+ sentences).
   - **Agglutinative, polysynthetic & heavily inflected languages (Azerbaijani, Turkish, Kazakh, Uzbek, Finnish, Hungarian, Korean, Japanese, Russian, German, Arabic):** Alternates compact 3–5 word finite clauses with 22–35 word multi-clause sentences (`CV >= 0.45`) while respecting native constituent order (such as SOV topic-focus scrambling in Turkic and free constituent fronting in Slavic).
   - **Isolating & logographic scripts (Mandarin Chinese, Cantonese, Japanese Kanji/Kana, Thai, Khmer):** Varies clause character counts sharply across punctuation boundaries.

---

## 🛡️ Two-Tier Sanitization Architecture

### Tier 1: Layer A — Multi-Vendor Technical Provenance Hygiene
Strips mechanical markers emitted across OpenAI/ChatGPT, Anthropic/Claude, Google/Gemini, DeepSeek, Qwen, Perplexity, Meta/Llama, Mistral, Cohere, Grok, and Copilot:
- **Unconditional Invisible Codepoint Strip:** Removes zero-width spaces (`\u200B`), word joiners (`\u2060`), soft hyphens (`\u00AD`), BOM (`\uFEFF`), combining grapheme joiners (`\u034F`), Arabic letter marks (`\u061C`), Mongolian free variation selectors (`\u180B`–`\u180E`), directional Bidi overrides/isolates (`\u200E`–`\u200F`, `\u202A`–`\u202E`, `\u2066`–`\u2069`), invisible math operators (`\u2061`–`\u2064`), interlinear annotation anchors (`\uFFF9`–`\uFFFB`), Plane 14 Unicode Tag characters (`\uE0000`–`\uE007F`), and steganographic Variation Selectors Supplement (`\uE0100`–`\uE01EF`).
- **Conditional Joiner Preservation:** Preserves `\u200D` (ZWJ) inside multi-codepoint emoji sequences and preserves `\u200C` (ZWNJ) / `\u200D` (ZWJ) where orthographically required (Arabic, Persian, Kurdish, Urdu, Devanagari, Bengali, Gurmukhi, Gujarati, Oriya, Tamil, Telugu, Kannada, Malayalam, Sinhala, Thai, Lao, Tibetan, Myanmar, Khmer). Strips them only when wedged between Latin, Cyrillic, or Greek letters.
- **Exotic Whitespace & Punctuation Normalization:** Replaces narrow no-break spaces (`\u202F`), non-breaking spaces (`\u00A0`), ideographic spaces (`\u3000` outside CJK), and quad/thin/hair spaces (`\u2000`–`\u200A`, `\u205F`) with standard ASCII spaces (`0x20`). Normalizes leaked fullwidth CJK punctuation (`，`, `：`, `；`, `（`, `）`) inside Latin, Cyrillic, or Turkic sentences.
- **UI & Citation Artifact Removal:** Strips OpenAI citation brackets (`【4:0†source】`, `:contentReference[oaicite:0]{index=0}`), unanchored footnote markers, and LLM tracking parameters (`?utm_source=chatgpt.com|claude|gemini|perplexity|copilot`).
- **Canonical NFC & Non-Destructive Mixed-Script Flagging:** Emits standard canonical `NFC` characters and flags any single word token mixing Latin, Cyrillic, and Greek scripts (e.g., a Cyrillic `о` inside a Latin word) while outputting the corrected pure-script word.

---

### Tier 2: Layer B — The 6 Core Rules (6-Pass Stylometric Editing Pipeline)

Explore the full interactive breakdown in **[`docs/rules.html`](docs/rules.html)** and multilingual examples in **[`docs/examples.html`](docs/examples.html)**:

| Pass / Rule | Target Signatures | Core Focus |
| :--- | :--- | :--- |
| **Pass 1: Macro-Discourse & Epistemic Alignment** | `#4, #21, #22, #25, #27–#32` | Cut panoramic openers (*"In today's rapidly evolving landscape..."*), moralizing codas, and paragraph sandwich symmetry; ground claims in source specifics. |
| **Pass 2: Typographic & Visual Scaffolding** | `#33, #34, #35, #36, #37, #38` | Dissolve decorative bulletization and `**[Bold Label]:**` lead-ins into prose; vary paragraph block weight; replace habitual em-dashes and diversify punctuation. |
| **Pass 3: Rhetorical Templates & Transitions** | `#11, #20, #23, #24, #26` | Collapse *"Not merely X, but Y"* negative parallelisms; unbalance `"While X, Y"` seesaws; prune forced triads (`A, B, and C`); cut polar merisms and heavy discourse glue. |
| **Pass 4: Syntactic Tree Variance & Burstiness** | `#2, #3, #7, #8, #9, #10, #12` | Cut shallow trailing participles (*", highlighting..."*) and stacked converbs (*"-araq/-ərək"*); alternate compact and complex parse trees; apply natural constituent fronting. |
| **Pass 5: Lexical Precision & Collocation De-Smoothing** | `#1, #5, #6, #13–#19` | Restore direct copulas (*is, are, has*) and active verbs over inflated substitutes (*serves as, boasts*); purge AI puffery, dead metaphors, weasel citations, and high-PMI clichés. |
| **Pass 6: Self-Audit Verification Gate** | Final Quality Gate | Verify sentence-length range, dash density (`<= 1 per 400 words`), non-terminal punctuation variety (`>= 3` types), and zero-tolerance structural tells. |

---

## 🚀 Installation & Usage

### 1. Install in Your AI Agent or Skill Collection
1. Copy [`SKILL.md`](SKILL.md) from this repository.
2. Add it to your LLM workspace, custom agent instructions, or skill directory.
3. Trigger the workflow in any language using `/stylometric-de-ai-rewriter` or `/remove-ai-marks`.

### 2. Output Modes
- **Default Mode:** Outputs the sanitized and rewritten text first, followed by a compact **Anti-Provenance & Stylometric Audit** table summarizing Layer A artifacts stripped and Pass 6 structural verification metrics.
- **Clean / Text-Only Mode:** When you ask for *"only the text"* or run the skill inside an automated pipeline, it outputs strictly the final rewritten text with no commentary.

---

## 🤝 Contributing

Contributions expanding cross-linguistic patterns, vendor provenance markers, or documentation are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting a Pull Request.

## 📄 License

Released under the **MIT License**. Free for personal, academic, and commercial use. See [LICENSE](LICENSE) for full text.