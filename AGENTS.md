# NAMEGEN Demo Agent Notes

Local instructions for the NAMEGEN demo page. Goal: fast, high-quality, pronounceable LLC names tailored to home inspection / property management consulting.

## What this agent does
- Takes 2–5 seed words and outputs candidate company names.
- Blends three sources: (1) local portmanteau, (2) local phono-aesthetic, (3) LLM suggestions via the configured Worker.

## How to run it
- Open `index.html` locally; no build step required.
- Set **Worker URL** to your Cloudflare Worker `/complete`.
- Paste the API key if your worker enforces `X-Auth-Key`/Bearer.
- Enter at least 2 words, click **Generate** (or press Enter).

## Worker contract (expected)
- `POST /complete` JSON: `{ prompt, temperature?, auth_key? }`
- If `X-Auth-Key` is required, the page sends it when the API key field is filled. Bearer works if the worker supports it.
- Response must include `content` (string). If `content` embeds JSON like `{"names":[{"name":"VoltForge","reason":"why"}]}`, it will be parsed; otherwise lines are parsed as names.

## What “good” names mean (guide for LLM)
- 3–12 chars preferred (hard cap 20), easy to pronounce, passes “radio test”.
- Tone: trustworthy, competent, steady (home inspection/property management consulting). Avoid “crypto/AI/dev” vibes.
- Tie to seeds via phonetics or concept; avoid generic dictionary-only words.
- Avoid double words, avoid long hyphens, avoid trailing numbers.
- Soft vetoes: hard consonant clusters (3+), obvious trademarks, geography unless asked.

## Output format for LLM (strict)
- JSON only: `{"names":[{"name":"NameHere","reason":"why it works"}]}`
- 8–12 entries max.
- Reasons: concise (4–12 words), highlight trust/safety/steadiness and seed tie-in.

## LLM guardrails (phonetic + aesthetic)
- Phonetic anchors: trusted openings (Safe, Sure, Haven, Harbor, Anchor, Shelter, Guardian, Oak, Cedar, True, Clear, Bright, Prime); flowing starts (L, R) and nasals (M, N); avoid 3+ consonant clusters. If consonants clash, bridge with a warm vowel (a/e/o).
- Aesthetic guards: length sweet spot 5–10 chars (hard stop 20); 2–3 syllables ideal, reject 5+; balance ascenders/descenders (avoid piling g/p/y/j or only tall letters).
- Style bans: no tech/crypto/AI jargon, no numbers, no hyphens, no “LLC/Inc” in name, no obvious trademarks, avoid geography unless provided, avoid seed duplication.
- Suffix palette (use sparingly): -stead, -point, -line, -lane, -haven, -house, -guard, -mark, -stone, -ridge, -craft, -scope, -path, -crest.
- Good vs bad examples: Good: Harborly, Surestead, Oakline, Nexthome, Clearpoint. Bad: AIInspect, CryptoCheck, Home-Inspect-123, NYCHomeLLC, TrustMgmtCorp, Inspectorator.
- Scoring hints: +2 if starts with trusted sound (H/S/A/L/M/N); +1 if contains liquid (l/r) without clusters; +1 if 2–3 syllables, -2 if 5+; -2 if 3+ consonants in a row; +1 if ends with approved suffix; reject if JSON format is violated.

## Local blend logic (summary)
- **Portmanteau**: syllable-ish splits; pairwise + chained; filters 3–20 chars.
- **Phono-Aesthetic**: overlap/liquid/bridge/snap; validates length, clusters, syllables, asc/desc balance; scores mechanical/flow/trust anchors and vowel balance; keeps top 6.

## Demo tips
- Use short, pronounceable seeds relevant to inspection/management (“Inspect”, “Haven”, “Anchor”, “Oak”, “Trust”, “Dwell”, “Shield”) for better blends.
- If the worker is offline, local sections still populate; LLM panel shows an error bubble.
- Swap workers by changing the URL/key—no code changes needed.
---------------
