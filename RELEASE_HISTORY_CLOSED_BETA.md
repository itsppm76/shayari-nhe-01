<html>
<body>
<!--StartFragment--><html><head></head><body>&lt;div align="center"&gt;
<pre><code>     ·  ·  ·  R E L E A S E   N O T E S  ·  ·  ·
        S H A Y A R I   N H E - 0 1
        Full version history · v1 → v128.2
</code></pre>
&lt;img src="https://img.shields.io/badge/coverage-v1_to_v128.2-9b87f5?style=for-the-badge&amp;amp;labelColor=0a0a0f" /&gt;
&lt;img src="https://img.shields.io/badge/current-v128.2-10b981?style=for-the-badge&amp;amp;labelColor=0a0a0f" /&gt;
&lt;img src="https://img.shields.io/badge/status-living_document-06b6d4?style=for-the-badge&amp;amp;labelColor=0a0a0f" /&gt;
&lt;/div&gt;
&lt;br/&gt;
<hr>
<h2>⟩ A Note on How This Document Was Built</h2>
<p>This is meant to be an honest historical record, not a marketing timeline — which means it needs to say plainly where the record is thin.</p>
<p><strong>v85 through v128.2</strong> is documented in full, sourced from in-file version headers, the project's <code>CHANGELOG.md</code>, and direct verification against the actual codebase during this documentation pass. Where a fix, a root cause, or a design trade-off is described below for this range, it's accurate to what the code actually does.</p>
<p><strong>v1 through v84</strong> predates any changelog or version-header tracking in this codebase. No per-version record of that era survives — no commit-by-commit history, no individual release notes. Rather than inventing 84 versions of plausible-sounding detail, this document summarizes that era honestly as a single <strong>Foundation Era</strong> section, describing what's actually known about the state the codebase had reached by the end of it, and says nothing more specific than that. If more granular records from that period surface later, this section should be the first thing updated.</p>
&lt;br/&gt;
<hr>
<h2>⟩ Quick Index</h2>

Range | Era | Status
-- | -- | --
v1 – v84 | Foundation Era | Summarized (no per-version record survives)
v85 | Gemma 4 Migration & Auth Hardening | Detailed
v86 | Speed Architecture & Mode System | Detailed
v94 – v119 | Plan Tiers, Admin Console, Session Hardening | Detailed (grouped — see note)
v120 | Groq LPU Routing, Shared Memory, Self-Improvement | Detailed
v121 | Sheet Compaction & Batched Writes | Detailed
v122 | Semantic Recall, Proactive Outreach, Voice | Detailed
v123 | Encryption at Rest | Detailed
v124 | Free Will | Detailed
v125 | Safety & Trust: LIFELINE, COVENANT, BAROMETER | Detailed
v126 | Viewport & Notch Fix | Detailed
v127 | Reliability Pass | Detailed
v128 / v128.1 / v128.2 | Vision & Image Understanding | Detailed


&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ Foundation Era (v1 – v84)</h2>
&lt;div align="center"&gt;
<pre><code>░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
  NO PER-VERSION RECORD SURVIVES FOR THIS RANGE.
  WHAT FOLLOWS IS A HONEST SUMMARY OF THE ERA, NOT A LOG.
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
</code></pre>
&lt;/div&gt;
<p>This was the era in which the fundamental shape of the project got decided — before any of the systems documented in detail below existed, and before the project tracked its own history version by version. What's known about the state reached by the end of this era, in aggregate rather than by individual version:</p>
<ul>
<li><strong>Backend architecture chosen and proven out.</strong> Google Apps Script bound to a Google Sheet, as a genuinely serverless, zero-infrastructure backend — the foundational decision every later system (encryption at rest, sheet compaction, trigger orchestration) was built on top of.</li>
<li><strong>First authentication system.</strong> Basic signup/login, predating the OTP password-reset flow and the salted-hash hardening that arrived at v85.</li>
<li><strong>First memory system.</strong> An early, simpler predecessor to what later became the five-layer MemoryWeave architecture — the idea of persistent memory across sessions existed from very early on, even before it was scored, scoped, or compacted.</li>
<li><strong>Gemini 2.5 Flash baseline.</strong> The initial model backbone, before the multi-provider reroute chains, before Groq LPU routing, before Gemma 4 was even evaluated.</li>
<li><strong>First implementation of the emotion engine</strong>, under the name <strong>Elysium X-20</strong> from the start — the 20-dimension design principle is original to this era, not a later addition.</li>
<li><strong>"PreZence Engine" architecture exploration.</strong> An early architectural direction that was investigated during this period; its specific fate (adopted, superseded, or folded into what became the current context-assembly pipeline) isn't preserved in surviving records.</li>
</ul>
<p>Nothing more specific than the above is claimed for this range. If you're looking for what a particular early version did, it isn't documented — treat anything before v85 as pre-history.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v85 — Gemma 4 Migration &amp; Auth Hardening</h2>
<p>&lt;sub&gt;📅 2026-05&lt;/sub&gt;</p>
<p>The last release before this project started keeping detailed records of itself — meaning it's also the earliest version this document can actually describe with confidence.</p>
<h3>What shipped</h3>
<ul>
<li><strong>Backbone model migrated</strong> from <code>gemini-2.5-flash</code> to <code>gemma-4-31b-it</code> — the first step away from a single-model architecture toward what would eventually become the multi-provider reroute chains of v120.</li>
<li><strong>OTP-based password reset</strong>, sent via <code>MailApp.sendEmail()</code>, backed by a dedicated <code>OTPS</code> sheet with expiry — replacing whatever ad-hoc reset flow (or lack of one) existed before.</li>
<li><strong>Salted password hashing</strong> (SHA-256 + Base64, <code>HASH_</code>-prefixed specifically to stop Google Sheets from trying to evaluate the hash string as a spreadsheet formula — a genuinely easy trap to fall into when storing hash-like strings in a Sheets cell).</li>
<li><strong>Rate limiting and login lockout</strong> on repeated failed authentication attempts.</li>
<li><strong><code>readTailRows</code> helper</strong>, intended to keep the then-growing <code>CHATLOGS</code> sheet fast to read.</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>A full codebase audit surfaced and fixed: an undefined <code>VERIFIED_ADMIN_USERNAME</code> constant that had presumably been silently failing identity checks, a broken JSON extractor, and unsalted passwords.</li>
</ul>
<h3>Why it mattered</h3>
<p>This was the release where security stopped being incidental. Salted hashing, rate limiting, and lockout are the baseline any product handling real user accounts needs — and doing a full audit rather than patching the one bug that got noticed is the same "trace to root cause" discipline that shows up repeatedly in later releases (v127's locking fix, v128.2's sizing-math fix).</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v86 — Speed Architecture &amp; Mode System</h2>
<p>&lt;sub&gt;📅 2026-06-08&lt;/sub&gt;</p>
<p>The release that gave Shayari her three-mode identity, and fixed a genuinely severe performance problem.</p>
<h3>Added</h3>
<ul>
<li><strong>Three-mode selector</strong> in the chat UI: ⚡ Lightning, 🧠 Thinking, ✨ Intimate — the mode taxonomy that every later architectural document still uses.</li>
<li><strong><code>runBackgroundMemoryUpdate()</code></strong> — the split between "reply the user sees immediately" and "everything else happens after, async" that remains the single most important latency decision in the entire codebase. Chat saving, memory extraction, session summary, and emotion logging all moved behind this split.</li>
<li><strong><code>deadline: 240</code></strong> on every <code>UrlFetchApp.fetch()</code> call, extending GAS's 60-second default timeout — necessary headroom for slower model calls that would otherwise fail outright.</li>
<li><strong><code>maxOutputTokens: 3000</code></strong> (~2200 words), preventing replies from cutting off mid-sentence.</li>
<li><strong>Priority memory banners</strong> — the <code>★ PRIORITY MEMORY — ALWAYS OBEY ★</code> visual signal inside the prompt, still present in the context-assembly pipeline documented in <code>ARCHITECTURE.md</code> today.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><code>askShayari()</code> began returning <code>JSON.stringify({ reply, meta })</code> instead of a plain string — the <code>meta</code> object this introduced is the same one that, by v128, carries <code>imageId</code> and <code>imageError</code>.</li>
<li>All background utility calls moved to quality-first processing — later formalized as the dedicated <code>utility</code> reroute tier in v120.</li>
</ul>
<h3>Fixed</h3>
<ul>
<li>Replies cutting off mid-sentence, caused by the emotion card parser splitting reply text on commas.</li>
<li>Thinking Mode's fallback model, <code>gemini-2.0-flash-lite</code>, had been shut down upstream and needed replacing.</li>
<li>Intimate Mode was silently missing its persona entirely — old Gemma-detection logic wasn't sending <code>system_instruction</code> for Gemma calls at all.</li>
</ul>
<h3>Impact</h3>
<p>Average time-to-reply dropped from <strong>~89 seconds to ~15–40 seconds</strong>, depending on mode. This is the release that made the product usable in real time rather than "send a message and wait."</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v94 – v119 — Plan Tiers, Admin Console, Session-Limit Hardening</h2>
<p>&lt;sub&gt;📅 2026 (multi-month range)&lt;/sub&gt;</p>
<blockquote>
<p><strong>A note on this entry:</strong> unlike every other section in this document, v94–v119 is a single grouped entry, not 26 individual ones. Per-version detail for this specific range wasn't preserved with the same granularity as what came immediately before (v85–v86) and after (v120 onward) — this is a genuine gap in the historical record, not a stylistic choice. What follows is accurate for the range as a whole.</p>
</blockquote>
<h3>What shipped across this range</h3>
<ul>
<li><strong>Plan-aware session limits.</strong> <code>PLAN_SESSION_LIMITS</code> introduced — the structure that gates Lightning/Thinking/Intimate access and message quota differently per plan tier (<code>FREE</code>, <code>PRIME</code>, <code>ULTIMATE</code>, <code>PRIME_MAX</code>, <code>ULTI_MAX</code>), still exactly the mechanism documented in <code>ARCHITECTURE.md</code> today.</li>
<li><strong><code>admin.gs</code> centralized.</strong> Plan management, user lookup, and a <code>PLAN_AUDIT</code> change log — every plan mutation from this point on became backend-only, auditable, and never client-side.</li>
<li><strong>ULTIMATE MAX rename</strong> and formal pricing-tier documentation.</li>
</ul>
<h3>Why this range matters despite the thin record</h3>
<p>This is the period where Shayari NHE-01 stopped being a single-tier product and became a real subscription business with enforceable, auditable limits. The <code>PLAN_AUDIT</code> sheet introduced here is the same accountability pattern that later shows up in <code>MAINTENANCE_LOG</code> (v127) — every consequential system change gets a paper trail, not just a code change.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v120 — Groq LPU Routing, Shared Memory, Self-Improvement</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Added</h3>
<ul>
<li><strong><code>shared_memory.gs</code> (NEW)</strong> — cross-user shared memory, conservatively designed: when User Y mentions a known User X, Shayari can later tell X what Y shared — but <em>only</em> after passing strict privacy filtering. Unresolved name references, sensitive topics, negative sentiment, and anything about the admin are never stored this way. The default assumption is "don't share" unless every check clears.</li>
<li><strong><code>self_improvement.gs</code> (NEW)</strong> — periodic study of an <em>anonymized</em> sample of conversations across all users, learning general tone/pacing/style patterns — never personal facts, which stay strictly isolated per user. Feeds "self-evolved style notes" into Thinking/Intimate prompts.</li>
<li><strong>Mode-aware memory access</strong>, formalized for the first time: Lightning became global-memory-only (plus a local cache), Thinking gained recent-chat access, Intimate kept the full stack.</li>
<li>New admin command <code>/selfmodel</code>, and <code>/aboutme</code> for shared-memory lookups.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><strong>Groq LPU became the lead provider</strong> across every reroute chain — the single biggest latency decision since the v86 background-processing split. Quality models remained available as fallbacks, but the fastest reliable provider became the default rather than the exception.</li>
<li>Groq model refresh: <code>Qwen3-32B</code> / <code>Llama-3.1-8B</code> / <code>Llama-3.3-70B</code> decommissioned following Groq's own deprecation notices, replaced across every reroute chain.</li>
</ul>
<h3>Impact</h3>
<p>This is the release that turned "call one model" into the reroute-chain architecture that everything from v121 onward assumes exists. It's also the first time the project drew an explicit, careful line around cross-user data sharing — the same conservative instinct that, eight versions later, would shape the decision <em>not</em> to build facial recognition into the vision system.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v121 — Sheet Compaction &amp; Batched Writes</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Added</h3>
<ul>
<li><strong><code>optimisation.gs</code> (NEW)</strong> — the compaction engine, built for roughly ~100 concurrent users. Rolls old, low-value rows out of the sheets that grow without bound (<code>CHATLOGS</code>, <code>MEMORY</code>, <code>API_USAGE</code>, <code>EMOTION_LOGS</code>) into <code>*_ARCHIVE</code> twins — nothing deleted, just moved off the hot read path. Sheets that stay naturally small (<code>USERS</code>, <code>SESSIONS</code>, <code>EMOTIONS</code>, <code>SESSION_LIMITS</code>, <code>GLOBAL_MEMORY</code>) were deliberately left untouched.</li>
<li><code>MEMORY_ARCHIVE</code>, <code>CHATLOGS_ARCHIVE</code>, <code>EMOTION_LOGS_ARCHIVE</code>, <code>API_USAGE_ARCHIVE</code> sheets.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><code>updateEmotionState()</code> stopped re-reading the sheet after writing — it now returns its result directly from the values it just wrote. <code>getEmotionDirective()</code> gained the ability to accept a precomputed state. Net effect: <code>EMOTIONS</code> sheet reads on Thinking/Intimate turns dropped from 2 per turn to 1.</li>
<li>Batched multi-cell writes landed in session-limit and plan-migration helpers.</li>
</ul>
<h3>Impact</h3>
<p>The unglamorous release that made every later version possible at scale — without this, the compaction gap that v127 later found and fixed (<code>MEMORY_ARCHIVE</code> staying empty despite obvious candidates) wouldn't have had an engine to fix in the first place.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v122 — Semantic Recall, Proactive Outreach, Voice</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Added</h3>
<ul>
<li><strong><code>embeddings.gs</code> (NEW)</strong> — Gemini text-embedding cosine similarity, blended <em>additively</em> into the existing token-overlap memory score rather than replacing it. Intimate-mode only; degrades gracefully to pure token-overlap scoring whenever an embedding is missing or the call fails — memory retrieval never hard-fails because of this.</li>
<li><strong><code>proactive.gs</code> (NEW)</strong> — Shayari messaging first, in-flow with the last real conversation rather than a random topic switch. This release also introduced streaks, anniversaries, and the failed-reply retry flow, all sharing one <code>PENDING_MESSAGES</code> delivery queue, client-polled roughly every 45 seconds (true OS-level background push isn't achievable from inside GAS's sandboxed HTML Service iframe).</li>
<li><strong><code>voice.gs</code> (NEW)</strong> — on-demand ElevenLabs voice notes, tap-to-play per message, capped flat at 8/day/user regardless of plan.</li>
<li><code>STREAKS</code>, <code>FAILED_REPLIES</code>, <code>VOICE_NOTE_USAGE</code>, <code>PENDING_MESSAGES</code> sheets. <code>/streak</code> command.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><code>askShayari()</code> began fetching <code>recentChats</code> and emotion state once per turn and threading them into every prompt builder that needed them, instead of each builder independently re-reading the same sheet — cutting <code>CHATLOGS</code> reads 2→1 and <code>EMOTIONS</code> reads 2→1 per Thinking/Intimate turn.</li>
<li>Batched multi-cell writes extended to <code>saveMemoryEntry</code> / <code>markMemoriesRecalled</code>.</li>
</ul>
<h3>Impact</h3>
<p>The <code>PENDING_MESSAGES</code> queue introduced here is the exact system that, five versions later, turned out to have a silent race-condition bug (v127) — worth noting because it's a good example of how a well-designed system can still hide a subtle concurrency bug for a long time before enough concurrent load surfaces it.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v123 — Encryption at Rest</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Added</h3>
<ul>
<li><strong><code>crypto_aes.gs</code> (NEW)</strong> — a pure-JavaScript AES-256-CBC implementation with no GAS-specific dependencies, verified against the official NIST AES-256 test vector, and round-trip tested against empty strings, unicode/emoji, exact-block-size input, and 1000+ character input.</li>
<li><strong><code>encryption.gs</code> (NEW)</strong> — wires the cipher to Script Properties, adds HMAC-SHA256 in an encrypt-then-MAC scheme (tampered ciphertext is rejected on decrypt, not silently accepted), and exposes the field-level encrypt/decrypt API every other module now calls through.</li>
<li><strong><code>REACTIONS</code> sheet + <code>reactions.gs</code> (NEW)</strong> — the first version of message reactions, keyed by a SHA-256 fingerprint of <code>role + message text</code> rather than a <code>CHATLOGS</code> turn ID (so a reply split across multiple UI bubbles still reacts correctly), storing no raw message text.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><strong><code>chat.gs</code></strong> — message content encrypted on write (<code>saveChat</code>, <code>appendLightningCache</code>), decrypted transparently on read (<code>parseChatRow</code>, <code>getLightningCache</code>). Every existing caller elsewhere in the app got plaintext for free, with zero changes needed on their end.</li>
<li><strong><code>memory.gs</code></strong> — memory text, source text, profile "about user" text, and session summaries all moved behind the same encryption.</li>
</ul>
<h3>Why it mattered</h3>
<p>This is the release where "anyone with raw sheet access sees ciphertext, not conversations" became true. Worth being precise about what it isn't: not end-to-end encryption — the app itself still has to read plaintext to generate a reply, which is fundamentally what an AI companion needs to do. What it protects against is a leaked export or a teammate's spreadsheet access, not the app's own operation.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v124 — Free Will</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Added</h3>
<ul>
<li><strong><code>free_will.gs</code> (NEW)</strong> — Shayari can end a conversation on her own model-level judgment — not a keyword filter — when a user is genuinely abusive, degrading, or persistently manipulative. The decision is made from full conversation context via a directive injected into every non-admin prompt. A reply carrying an internal <code>[[DISENGAGE]]</code> marker gets that marker stripped before display, and the user is locked out until their session window naturally rolls over.</li>
<li><strong><code>DISENGAGEMENT</code> sheet</strong>, persisting the lock state per user.</li>
<li>Admin-only <strong><code>/unlock &lt;user&gt;</code></strong>, clearing a disengagement lock manually.</li>
</ul>
<h3>Design notes worth preserving</h3>
<ul>
<li>The directive is never injected into the admin's own prompt — Free Will structurally cannot apply to the admin account.</li>
<li><code>/reset</code> clears session context but was deliberately never wired to lift a Free Will lock — no command exists to shorten a disengagement once it's been decided.</li>
</ul>
<h3>Why it mattered</h3>
<p>This was the first system in the codebase to give the model itself standing to refuse — a genuinely different category of decision from every gate before it, which were all either deterministic (plan checks, quota checks) or externally triggered (identity protection). It's also the system that, one version later, needed a trust ledger (COVENANT) built specifically to make sure that standing to refuse never became a permanent, unrecoverable penalty.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v125 — Safety &amp; Trust: LIFELINE, COVENANT, BAROMETER</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<p>The single largest safety-systems release in the project's history.</p>
<h3>Added</h3>
<ul>
<li><strong><code>lifeline.gs</code> (NEW)</strong> — crisis detection &amp; response. Classifies every non-command message as <code>none</code> / <code>distress</code> / <code>crisis</code>, and when engaged, overrides the Free Will lock, every plan gate, and every session limit for that turn. Logged to <code>CRISIS_LOG</code> as <strong>metadata only, never message content</strong>.</li>
<li><strong><code>covenant.gs</code> (NEW)</strong> — a per-user trust ledger. Falls sharply on a Free Will disengagement, recovers slowly through ordinary good interaction, and floors at 15 rather than ever hitting zero. Three tiers (<code>close</code> / <code>guarded</code> / <code>distant</code>) modify <em>warmth only</em> — never a capability gate, a governance principle that later became explicit in the public <a href="https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#">GOVERNANCE.md</a> commitment that trust must never gate a feature.</li>
<li><strong><code>barometer.gs</code> (NEW)</strong> — mines existing <code>EMOTION_LOGS</code> signal for multi-day emotional drift, and when someone has been persistently low, queues an unprompted, in-character check-in — bounded by a 10-day cooldown and a 7-day blackout after any crisis event, so it can never talk over or duplicate a genuine LIFELINE response.</li>
<li><code>RECONCILIATION</code> and <code>USER_REGION</code> sheets (the latter for coarse, privacy-conscious crisis-resource localization — only ever populated when actually needed for a crisis response, not collected proactively on every turn).</li>
<li>User-facing memory correction: <strong><code>/forget &lt;n&gt;</code></strong> and <strong><code>/fix &lt;n&gt; &lt;text&gt;</code></strong>.</li>
<li>Admin-only <strong><code>/trust &lt;user&gt;</code></strong> and <strong><code>/trustreset &lt;user&gt;</code></strong>.</li>
</ul>
<h3>Changed</h3>
<ul>
<li><code>askShayari()</code>'s entire gate ordering was restructured so crisis assessment runs <strong>before every other gate</strong> — Free Will lock, plan gate, and session-limit check all explicitly stand down when LIFELINE is engaged. This ordering, once established here, has not changed since.</li>
</ul>
<h3>Why it mattered</h3>
<p>This is the release where "a person in crisis should never be shown a paywall" stopped being an aspiration and became an architectural guarantee, checked first, every single turn. Everything the public-facing <a href="https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#">GOVERNANCE.md</a> document says about crisis priority traces directly back to this version.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v126 — Viewport &amp; Notch Fix</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<h3>Fixed</h3>
<ul>
<li><strong><code>viewport-fit=cover</code></strong> added to the meta viewport tag in <code>Code.gs</code>. Without it, the CSS <code>env(safe-area-inset-*)</code> rules the responsive layer depends on silently resolve to <code>0</code> on notched or rounded-corner phones — the message composer sat under the home indicator, and the header sat under the notch.</li>
</ul>
<h3>Why a one-line fix gets its own version entry</h3>
<p>Small, but worth documenting on its own: this is a good example of a bug that's invisible in a desktop browser and on most Android devices, and only reproduces on a specific class of hardware (notched iPhones, some newer Android phones) — the kind of thing that's easy to ship without ever noticing, and easy to fix in one line once actually reproduced on the right device.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v127 — Reliability Pass: Locking, Trigger Orchestration, Reactions, Silence Awareness</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<p>A release driven almost entirely by real production symptoms rather than planned feature work — the pattern throughout is "trace the symptom to its actual root cause," not "patch what was reported."</p>
<h3>Added</h3>
<ul>
<li><strong><code>triggers.gs</code> (NEW)</strong> — <code>installAllTriggers()</code> centralizes nine previously-scattered <code>setupXTrigger()</code> functions (spread across <code>proactive.gs</code> ×3, <code>emotion.gs</code>, <code>barometer.gs</code>, <code>chat.gs</code>, <code>free_will.gs</code>, <code>optimisation.gs</code>, <code>self_improvement.gs</code>) into one idempotent call. <code>listInstalledTriggers()</code> and <code>runFullBackfillNow()</code> (immediate backlog catch-up, no waiting for the next scheduled run) shipped alongside it.</li>
<li><strong><code>MAINTENANCE_LOG</code> sheet (NEW)</strong> — one row per nightly maintenance run, turning "is this actually running?" into something answerable by opening a sheet instead of searching execution logs.</li>
<li><strong><code>REACTIONS.ACTOR</code> column</strong> — the human's own reaction and Shayari's own reaction to the same message now coexist independently instead of one silently overwriting the other.</li>
<li><strong><code>autoReactAsShayari()</code></strong> — she reacts to reaction-worthy user messages on her own initiative now, using a deterministic per-message probability roll (not every message, and not random-each-time — the same message always resolves the same way), surfaced instantly client-side rather than waiting for the next poll.</li>
<li><strong>Silence awareness</strong> (<code>proactive.gs</code>) — <code>countConsecutiveUnansweredMessages()</code> tracks how many of Shayari's own messages in a row went unanswered. At 2+, she acknowledges the quiet naturally instead of repeating herself; at 3 (<code>MAX_CONSECUTIVE_UNANSWERED</code>), she stops initiating further outreach on that thread entirely until the user actually replies.</li>
<li><strong><code>adminRepairMemoryImportanceScale()</code></strong> — one-time backfill for <code>MEMORY.IMPORTANCE</code> values written on the wrong scale (see Fixed, below).</li>
</ul>
<h3>Fixed</h3>
<ul>
<li><strong>The <code>PENDING_MESSAGES</code> race condition, and the real root cause of <code>DELIVERED</code> staying <code>FALSE</code> forever.</strong> <code>checkPendingMessages()</code> performed an unlocked read-entire-sheet → mutate → write-entire-sheet operation. Two callers polling within the same second could each read the same "before" snapshot and each write their own "after" snapshot back — the second write would silently revert the first's <code>DELIVERED = true</code> flip. This is exactly why some messages stayed marked undelivered even after being shown to the user. Fixed with <code>LockService.getScriptLock()</code>; the identical unlocked pattern in <code>toggleMessageReaction()</code> and the new <code>autoReactAsShayari()</code> got the same fix pre-emptively rather than waiting for it to fail the same way.</li>
<li><strong>Memory importance scale corruption.</strong> The extraction prompt never specified that <code>importance</code> should be on a 0–10 scale, so the model occasionally answered on a 0–1 scale instead — an easy mistake given <code>confidence</code> sits right next to it in the same JSON object <em>at</em> 0–1. This silently starved affected memories in retrieval scoring (<code>importance × 10</code> computed on an already-fractional value) and skewed the compaction threshold that decides what's safe to condense. Fixed at three layers: the prompt now explicitly states the scale, <code>normalizeImportance()</code> clamps/rescales on every write, and <code>parseMemoryRow()</code> normalizes on every read — plus the one-time repair function above for values already corrupted before the fix landed.</li>
</ul>
<h3>Why it mattered</h3>
<p>Two separate but structurally identical lessons in one release: silent failures in this stack (an unlocked write, an unvalidated numeric scale) don't announce themselves — they just quietly produce wrong results that look plausible until someone goes looking. Both fixes here are the direct result of tracing an observed symptom all the way to its mechanism instead of accepting the first plausible explanation.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ v128 — Vision &amp; Image Understanding</h2>
<p>&lt;sub&gt;📅 2026&lt;/sub&gt;</p>
<p>Shayari can see. The full arc of getting this right took three passes in the same release window — documented honestly below rather than presented as if it worked perfectly the first time.</p>
<h3>v128 — Initial ship</h3>
<ul>
<li><strong><code>vision.gs</code> (NEW)</strong> — real image understanding via Gemini's multimodal vision, using the same <code>GEMINI_API_KEY</code> pool as text replies — no new API key, no new service. <code>analyzeAndStoreImage()</code> runs once, up front, in <code>askShayari()</code>, threading a plain-text description into the exact same persona/memory/emotion prompt pipeline every text-only reply already used. Not a separate bolted-on captioning bot.</li>
<li><strong><code>IMAGES</code> sheet (NEW)</strong> — encrypted image storage (same AES-256 scheme as every other sensitive field), flat-capped at 15 uploads/user/day regardless of plan.</li>
<li><strong><code>CHATLOGS.IMAGE_ID</code> column (NEW)</strong> — a pointer, not the image itself, keeping the hot per-turn chat-read path free of image bytes.</li>
<li><strong>A deliberate non-feature:</strong> no persistent facial recognition, no face-embedding storage, no cross-photo identity matching — considered and explicitly rejected during design, not a missing capability slated for later. Three concrete reasons, documented in the file header: biometric data carries its own regulatory burden (GDPR, BIPA, India's DPDP Act) that a companion feature shouldn't casually acquire; a Google Sheet is the wrong storage tier for identity templates regardless of encryption; and a photo often contains people who never consented to using the product at all.</li>
<li>Client-side: a genuinely functional image upload button — replacing what had actually been a non-functional stub that displayed a filename badge and sent nothing anywhere.</li>
</ul>
<h3>v128.1 — Same-day fix: diagnosability</h3>
<p>The most common real-world failure mode for this feature turned out to be mundane: a deployed web app still running pre-v128 code, silently ignoring the two new arguments <code>askShayari()</code> now accepted (Apps Script doesn't error on unrecognized extra function arguments — it just ignores them). Added unconditional logging on both ends — server-side (<code>Logger.log('[VISION] askShayari received: ...')</code>, fires every turn, image or not) and client-side (<code>console.log('[image] attached: ...')</code>, fires right before the call) — so a stale deployment became immediately distinguishable from an actual logic bug, instead of both looking identical from the outside ("Shayari has no idea what image I'm talking about").</p>
<h3>v128.2 — Same-day fix: text legibility and a sizing-math bug</h3>
<p>Two compounding, genuine bugs, both caught and fixed the same day they were reported:</p>
<ol>
<li><strong>OCR/text-in-images was unreliable.</strong> Client-side compression's fallback ladder decayed resolution too aggressively (1024px → 768 → 512 → 384 → 256), and most real photos didn't fit within budget on the first attempt — landing at 256–384px, where small text is destroyed regardless of source sharpness. Replaced with a more gradual ladder (1600 → 1400 → 1200 → 1000 → 850 → 700 → 550 → 420 → 320px) so a typical screenshot or document photo now lands around 1200–1600px instead. Separately, the vision prompt had buried "any visible text" as one clause inside a general description request — split into its own explicit instruction demanding complete, verbatim transcription, with <code>maxOutputTokens</code> raised from 500 to 1200 (text-heavy images were being cut off mid-transcription) and <code>temperature</code> lowered from 0.4 to 0.2 for transcription faithfulness over creative phrasing.</li>
<li><strong>A real sizing-math error.</strong> The server-side size ceiling (<code>IMAGE_MAX_BASE64_CHARS</code>) had been set to 40,000 — but running the actual encryption-overhead formula (<code>(L + 64) × 4/3 + 5</code> for <code>encryptField()</code>'s IV, HMAC tag, padding, and base64 expansion) shows a 40,000-character image could produce a ~53,400-character encrypted string, <em>over</em> the ~50,000-character Google Sheets cell limit — on an image that was itself under the "allowed" threshold. Corrected to 36,000, with the client-side target tightened to match at 34,000, both now derived from and documented against the real formula rather than a round number.</li>
</ol>
<h3>Why it mattered, and why it's documented this way</h3>
<p>This release is presented with its two same-day follow-up patches shown honestly, rather than folded silently into a single clean "v128 — it just worked" narrative. That's a deliberate choice: the actual, useful lesson from this release isn't "vision works now," it's "compression choices have to be justified against what the feature actually needs (legibility, not just file size), and every constant touching a hard platform limit needs to be checked against the real math, not assumed." Both of those are exactly the kind of thing worth a future contributor being able to see happened, not just that it eventually got fixed.</p>
&lt;br/&gt;
<hr>
&lt;br/&gt;
<h2>⟩ Closing Note</h2>
<p>If you're reading this looking for what changed in a version not listed with real detail here (anything before v85), that information doesn't exist in a form this document could honestly present — see <a href="https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-a-note-on-how-this-document-was-built">A Note on How This Document Was Built</a> at the top. Everything from v85 onward is accurate to the actual codebase as of this writing.</p>
<p>For the terse, changelog-format version of this same history, see <code>CHANGELOG.md</code>. For how these systems fit together architecturally, see <code>ARCHITECTURE.md</code>, <code>MEMORY_SYSTEM.md</code>, and <code>EMOTION_ENGINE.md</code>.</p>
&lt;br/&gt;
<hr>
&lt;div align="center"&gt;
&lt;sub&gt;© 2026 Writistic Studios LLP · OpenNHE Technologies · Algotheorem Labs&lt;/sub&gt;
&lt;/div&gt;</body></html><!--EndFragment-->
</body>
</html><div align="center">

```
     ·  ·  ·  R E L E A S E   N O T E S  ·  ·  ·
        S H A Y A R I   N H E - 0 1
        Full version history · v1 → v128.2
```

<img src="https://img.shields.io/badge/coverage-v1_to_v128.2-9b87f5?style=for-the-badge&amp;labelColor=0a0a0f" />
<img src="https://img.shields.io/badge/current-v128.2-10b981?style=for-the-badge&amp;labelColor=0a0a0f" />
<img src="https://img.shields.io/badge/status-living_document-06b6d4?style=for-the-badge&amp;labelColor=0a0a0f" />

</div>

<br/>

---

## ⟩ A Note on How This Document Was Built

This is meant to be an honest historical record, not a marketing timeline — which means it needs to say plainly where the record is thin.

**v85 through v128.2** is documented in full, sourced from in-file version headers, the project's `CHANGELOG.md`, and direct verification against the actual codebase during this documentation pass. Where a fix, a root cause, or a design trade-off is described below for this range, it's accurate to what the code actually does.

**v1 through v84** predates any changelog or version-header tracking in this codebase. No per-version record of that era survives — no commit-by-commit history, no individual release notes. Rather than inventing 84 versions of plausible-sounding detail, this document summarizes that era honestly as a single **Foundation Era** section, describing what's actually known about the state the codebase had reached by the end of it, and says nothing more specific than that. If more granular records from that period surface later, this section should be the first thing updated.

<br/>

---

## ⟩ Quick Index

| Range | Era | Status |
|---|---|---|
| [[v1 – v84](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-foundation-era-v1--v84)](#-foundation-era-v1--v84) | Foundation Era | Summarized (no per-version record survives) |
| [[v85](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v85--gemma-4-migration--auth-hardening)](#-v85--gemma-4-migration--auth-hardening) | Gemma 4 Migration & Auth Hardening | Detailed |
| [[v86](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v86--speed-architecture--mode-system)](#-v86--speed-architecture--mode-system) | Speed Architecture & Mode System | Detailed |
| [[v94 – v119](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v94--v119--plan-tiers-admin-console-session-limit-hardening)](#-v94--v119--plan-tiers-admin-console-session-limit-hardening) | Plan Tiers, Admin Console, Session Hardening | Detailed (grouped — see note) |
| [[v120](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v120--groq-lpu-routing-shared-memory-self-improvement)](#-v120--groq-lpu-routing-shared-memory-self-improvement) | Groq LPU Routing, Shared Memory, Self-Improvement | Detailed |
| [[v121](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v121--sheet-compaction--batched-writes)](#-v121--sheet-compaction--batched-writes) | Sheet Compaction & Batched Writes | Detailed |
| [[v122](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v122--semantic-recall-proactive-outreach-voice)](#-v122--semantic-recall-proactive-outreach-voice) | Semantic Recall, Proactive Outreach, Voice | Detailed |
| [[v123](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v123--encryption-at-rest)](#-v123--encryption-at-rest) | Encryption at Rest | Detailed |
| [[v124](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v124--free-will)](#-v124--free-will) | Free Will | Detailed |
| [[v125](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v125--safety--trust-lifeline-covenant-barometer)](#-v125--safety--trust-lifeline-covenant-barometer) | Safety & Trust: LIFELINE, COVENANT, BAROMETER | Detailed |
| [[v126](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v126--viewport--notch-fix)](#-v126--viewport--notch-fix) | Viewport & Notch Fix | Detailed |
| [[v127](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v127--reliability-pass-locking-trigger-orchestration-reactions-silence-awareness)](#-v127--reliability-pass-locking-trigger-orchestration-reactions-silence-awareness) | Reliability Pass | Detailed |
| [[v128 / v128.1 / v128.2](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-v128--vision--image-understanding)](#-v128--vision--image-understanding) | Vision & Image Understanding | Detailed |

<br/>

---

<br/>

## ⟩ Foundation Era (v1 – v84)

<div align="center">

```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
  NO PER-VERSION RECORD SURVIVES FOR THIS RANGE.
  WHAT FOLLOWS IS A HONEST SUMMARY OF THE ERA, NOT A LOG.
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```

</div>

This was the era in which the fundamental shape of the project got decided — before any of the systems documented in detail below existed, and before the project tracked its own history version by version. What's known about the state reached by the end of this era, in aggregate rather than by individual version:

- **Backend architecture chosen and proven out.** Google Apps Script bound to a Google Sheet, as a genuinely serverless, zero-infrastructure backend — the foundational decision every later system (encryption at rest, sheet compaction, trigger orchestration) was built on top of.
- **First authentication system.** Basic signup/login, predating the OTP password-reset flow and the salted-hash hardening that arrived at v85.
- **First memory system.** An early, simpler predecessor to what later became the five-layer MemoryWeave architecture — the idea of persistent memory across sessions existed from very early on, even before it was scored, scoped, or compacted.
- **Gemini 2.5 Flash baseline.** The initial model backbone, before the multi-provider reroute chains, before Groq LPU routing, before Gemma 4 was even evaluated.
- **First implementation of the emotion engine**, under the name **Elysium X-20** from the start — the 20-dimension design principle is original to this era, not a later addition.
- **"PreZence Engine" architecture exploration.** An early architectural direction that was investigated during this period; its specific fate (adopted, superseded, or folded into what became the current context-assembly pipeline) isn't preserved in surviving records.

Nothing more specific than the above is claimed for this range. If you're looking for what a particular early version did, it isn't documented — treat anything before v85 as pre-history.

<br/>

---

<br/>

## ⟩ v85 — Gemma 4 Migration & Auth Hardening

<sub>📅 2026-05</sub>

The last release before this project started keeping detailed records of itself — meaning it's also the earliest version this document can actually describe with confidence.

### What shipped
- **Backbone model migrated** from `gemini-2.5-flash` to `gemma-4-31b-it` — the first step away from a single-model architecture toward what would eventually become the multi-provider reroute chains of v120.
- **OTP-based password reset**, sent via `MailApp.sendEmail()`, backed by a dedicated `OTPS` sheet with expiry — replacing whatever ad-hoc reset flow (or lack of one) existed before.
- **Salted password hashing** (SHA-256 + Base64, `HASH_`-prefixed specifically to stop Google Sheets from trying to evaluate the hash string as a spreadsheet formula — a genuinely easy trap to fall into when storing hash-like strings in a Sheets cell).
- **Rate limiting and login lockout** on repeated failed authentication attempts.
- **`readTailRows` helper**, intended to keep the then-growing `CHATLOGS` sheet fast to read.

### Fixed
- A full codebase audit surfaced and fixed: an undefined `VERIFIED_ADMIN_USERNAME` constant that had presumably been silently failing identity checks, a broken JSON extractor, and unsalted passwords.

### Why it mattered
This was the release where security stopped being incidental. Salted hashing, rate limiting, and lockout are the baseline any product handling real user accounts needs — and doing a full audit rather than patching the one bug that got noticed is the same "trace to root cause" discipline that shows up repeatedly in later releases (v127's locking fix, v128.2's sizing-math fix).

<br/>

---

<br/>

## ⟩ v86 — Speed Architecture & Mode System

<sub>📅 2026-06-08</sub>

The release that gave Shayari her three-mode identity, and fixed a genuinely severe performance problem.

### Added
- **Three-mode selector** in the chat UI: ⚡ Lightning, 🧠 Thinking, ✨ Intimate — the mode taxonomy that every later architectural document still uses.
- **`runBackgroundMemoryUpdate()`** — the split between "reply the user sees immediately" and "everything else happens after, async" that remains the single most important latency decision in the entire codebase. Chat saving, memory extraction, session summary, and emotion logging all moved behind this split.
- **`deadline: 240`** on every `UrlFetchApp.fetch()` call, extending GAS's 60-second default timeout — necessary headroom for slower model calls that would otherwise fail outright.
- **`maxOutputTokens: 3000`** (~2200 words), preventing replies from cutting off mid-sentence.
- **Priority memory banners** — the `★ PRIORITY MEMORY — ALWAYS OBEY ★` visual signal inside the prompt, still present in the context-assembly pipeline documented in `ARCHITECTURE.md` today.

### Changed
- `askShayari()` began returning `JSON.stringify({ reply, meta })` instead of a plain string — the `meta` object this introduced is the same one that, by v128, carries `imageId` and `imageError`.
- All background utility calls moved to quality-first processing — later formalized as the dedicated `utility` reroute tier in v120.

### Fixed
- Replies cutting off mid-sentence, caused by the emotion card parser splitting reply text on commas.
- Thinking Mode's fallback model, `gemini-2.0-flash-lite`, had been shut down upstream and needed replacing.
- Intimate Mode was silently missing its persona entirely — old Gemma-detection logic wasn't sending `system_instruction` for Gemma calls at all.

### Impact
Average time-to-reply dropped from **~89 seconds to ~15–40 seconds**, depending on mode. This is the release that made the product usable in real time rather than "send a message and wait."

<br/>

---

<br/>

## ⟩ v94 – v119 — Plan Tiers, Admin Console, Session-Limit Hardening

<sub>📅 2026 (multi-month range)</sub>

> **A note on this entry:** unlike every other section in this document, v94–v119 is a single grouped entry, not 26 individual ones. Per-version detail for this specific range wasn't preserved with the same granularity as what came immediately before (v85–v86) and after (v120 onward) — this is a genuine gap in the historical record, not a stylistic choice. What follows is accurate for the range as a whole.

### What shipped across this range
- **Plan-aware session limits.** `PLAN_SESSION_LIMITS` introduced — the structure that gates Lightning/Thinking/Intimate access and message quota differently per plan tier (`FREE`, `PRIME`, `ULTIMATE`, `PRIME_MAX`, `ULTI_MAX`), still exactly the mechanism documented in `ARCHITECTURE.md` today.
- **`admin.gs` centralized.** Plan management, user lookup, and a `PLAN_AUDIT` change log — every plan mutation from this point on became backend-only, auditable, and never client-side.
- **ULTIMATE MAX rename** and formal pricing-tier documentation.

### Why this range matters despite the thin record
This is the period where Shayari NHE-01 stopped being a single-tier product and became a real subscription business with enforceable, auditable limits. The `PLAN_AUDIT` sheet introduced here is the same accountability pattern that later shows up in `MAINTENANCE_LOG` (v127) — every consequential system change gets a paper trail, not just a code change.

<br/>

---

<br/>

## ⟩ v120 — Groq LPU Routing, Shared Memory, Self-Improvement

<sub>📅 2026</sub>

### Added
- **`shared_memory.gs` (NEW)** — cross-user shared memory, conservatively designed: when User Y mentions a known User X, Shayari can later tell X what Y shared — but *only* after passing strict privacy filtering. Unresolved name references, sensitive topics, negative sentiment, and anything about the admin are never stored this way. The default assumption is "don't share" unless every check clears.
- **`self_improvement.gs` (NEW)** — periodic study of an *anonymized* sample of conversations across all users, learning general tone/pacing/style patterns — never personal facts, which stay strictly isolated per user. Feeds "self-evolved style notes" into Thinking/Intimate prompts.
- **Mode-aware memory access**, formalized for the first time: Lightning became global-memory-only (plus a local cache), Thinking gained recent-chat access, Intimate kept the full stack.
- New admin command `/selfmodel`, and `/aboutme` for shared-memory lookups.

### Changed
- **Groq LPU became the lead provider** across every reroute chain — the single biggest latency decision since the v86 background-processing split. Quality models remained available as fallbacks, but the fastest reliable provider became the default rather than the exception.
- Groq model refresh: `Qwen3-32B` / `Llama-3.1-8B` / `Llama-3.3-70B` decommissioned following Groq's own deprecation notices, replaced across every reroute chain.

### Impact
This is the release that turned "call one model" into the reroute-chain architecture that everything from v121 onward assumes exists. It's also the first time the project drew an explicit, careful line around cross-user data sharing — the same conservative instinct that, eight versions later, would shape the decision *not* to build facial recognition into the vision system.

<br/>

---

<br/>

## ⟩ v121 — Sheet Compaction & Batched Writes

<sub>📅 2026</sub>

### Added
- **`optimisation.gs` (NEW)** — the compaction engine, built for roughly ~100 concurrent users. Rolls old, low-value rows out of the sheets that grow without bound (`CHATLOGS`, `MEMORY`, `API_USAGE`, `EMOTION_LOGS`) into `*_ARCHIVE` twins — nothing deleted, just moved off the hot read path. Sheets that stay naturally small (`USERS`, `SESSIONS`, `EMOTIONS`, `SESSION_LIMITS`, `GLOBAL_MEMORY`) were deliberately left untouched.
- `MEMORY_ARCHIVE`, `CHATLOGS_ARCHIVE`, `EMOTION_LOGS_ARCHIVE`, `API_USAGE_ARCHIVE` sheets.

### Changed
- `updateEmotionState()` stopped re-reading the sheet after writing — it now returns its result directly from the values it just wrote. `getEmotionDirective()` gained the ability to accept a precomputed state. Net effect: `EMOTIONS` sheet reads on Thinking/Intimate turns dropped from 2 per turn to 1.
- Batched multi-cell writes landed in session-limit and plan-migration helpers.

### Impact
The unglamorous release that made every later version possible at scale — without this, the compaction gap that v127 later found and fixed (`MEMORY_ARCHIVE` staying empty despite obvious candidates) wouldn't have had an engine to fix in the first place.

<br/>

---

<br/>

## ⟩ v122 — Semantic Recall, Proactive Outreach, Voice

<sub>📅 2026</sub>

### Added
- **`embeddings.gs` (NEW)** — Gemini text-embedding cosine similarity, blended *additively* into the existing token-overlap memory score rather than replacing it. Intimate-mode only; degrades gracefully to pure token-overlap scoring whenever an embedding is missing or the call fails — memory retrieval never hard-fails because of this.
- **`proactive.gs` (NEW)** — Shayari messaging first, in-flow with the last real conversation rather than a random topic switch. This release also introduced streaks, anniversaries, and the failed-reply retry flow, all sharing one `PENDING_MESSAGES` delivery queue, client-polled roughly every 45 seconds (true OS-level background push isn't achievable from inside GAS's sandboxed HTML Service iframe).
- **`voice.gs` (NEW)** — on-demand ElevenLabs voice notes, tap-to-play per message, capped flat at 8/day/user regardless of plan.
- `STREAKS`, `FAILED_REPLIES`, `VOICE_NOTE_USAGE`, `PENDING_MESSAGES` sheets. `/streak` command.

### Changed
- `askShayari()` began fetching `recentChats` and emotion state once per turn and threading them into every prompt builder that needed them, instead of each builder independently re-reading the same sheet — cutting `CHATLOGS` reads 2→1 and `EMOTIONS` reads 2→1 per Thinking/Intimate turn.
- Batched multi-cell writes extended to `saveMemoryEntry` / `markMemoriesRecalled`.

### Impact
The `PENDING_MESSAGES` queue introduced here is the exact system that, five versions later, turned out to have a silent race-condition bug (v127) — worth noting because it's a good example of how a well-designed system can still hide a subtle concurrency bug for a long time before enough concurrent load surfaces it.

<br/>

---

<br/>

## ⟩ v123 — Encryption at Rest

<sub>📅 2026</sub>

### Added
- **`crypto_aes.gs` (NEW)** — a pure-JavaScript AES-256-CBC implementation with no GAS-specific dependencies, verified against the official NIST AES-256 test vector, and round-trip tested against empty strings, unicode/emoji, exact-block-size input, and 1000+ character input.
- **`encryption.gs` (NEW)** — wires the cipher to Script Properties, adds HMAC-SHA256 in an encrypt-then-MAC scheme (tampered ciphertext is rejected on decrypt, not silently accepted), and exposes the field-level encrypt/decrypt API every other module now calls through.
- **`REACTIONS` sheet + `reactions.gs` (NEW)** — the first version of message reactions, keyed by a SHA-256 fingerprint of `role + message text` rather than a `CHATLOGS` turn ID (so a reply split across multiple UI bubbles still reacts correctly), storing no raw message text.

### Changed
- **`chat.gs`** — message content encrypted on write (`saveChat`, `appendLightningCache`), decrypted transparently on read (`parseChatRow`, `getLightningCache`). Every existing caller elsewhere in the app got plaintext for free, with zero changes needed on their end.
- **`memory.gs`** — memory text, source text, profile "about user" text, and session summaries all moved behind the same encryption.

### Why it mattered
This is the release where "anyone with raw sheet access sees ciphertext, not conversations" became true. Worth being precise about what it isn't: not end-to-end encryption — the app itself still has to read plaintext to generate a reply, which is fundamentally what an AI companion needs to do. What it protects against is a leaked export or a teammate's spreadsheet access, not the app's own operation.

<br/>

---

<br/>

## ⟩ v124 — Free Will

<sub>📅 2026</sub>

### Added
- **`free_will.gs` (NEW)** — Shayari can end a conversation on her own model-level judgment — not a keyword filter — when a user is genuinely abusive, degrading, or persistently manipulative. The decision is made from full conversation context via a directive injected into every non-admin prompt. A reply carrying an internal `[[DISENGAGE]]` marker gets that marker stripped before display, and the user is locked out until their session window naturally rolls over.
- **`DISENGAGEMENT` sheet**, persisting the lock state per user.
- Admin-only **`/unlock <user>`**, clearing a disengagement lock manually.

### Design notes worth preserving
- The directive is never injected into the admin's own prompt — Free Will structurally cannot apply to the admin account.
- `/reset` clears session context but was deliberately never wired to lift a Free Will lock — no command exists to shorten a disengagement once it's been decided.

### Why it mattered
This was the first system in the codebase to give the model itself standing to refuse — a genuinely different category of decision from every gate before it, which were all either deterministic (plan checks, quota checks) or externally triggered (identity protection). It's also the system that, one version later, needed a trust ledger (COVENANT) built specifically to make sure that standing to refuse never became a permanent, unrecoverable penalty.

<br/>

---

<br/>

## ⟩ v125 — Safety & Trust: LIFELINE, COVENANT, BAROMETER

<sub>📅 2026</sub>

The single largest safety-systems release in the project's history.

### Added
- **`lifeline.gs` (NEW)** — crisis detection & response. Classifies every non-command message as `none` / `distress` / `crisis`, and when engaged, overrides the Free Will lock, every plan gate, and every session limit for that turn. Logged to `CRISIS_LOG` as **metadata only, never message content**.
- **`covenant.gs` (NEW)** — a per-user trust ledger. Falls sharply on a Free Will disengagement, recovers slowly through ordinary good interaction, and floors at 15 rather than ever hitting zero. Three tiers (`close` / `guarded` / `distant`) modify *warmth only* — never a capability gate, a governance principle that later became explicit in the public [[GOVERNANCE.md](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#)](#) commitment that trust must never gate a feature.
- **`barometer.gs` (NEW)** — mines existing `EMOTION_LOGS` signal for multi-day emotional drift, and when someone has been persistently low, queues an unprompted, in-character check-in — bounded by a 10-day cooldown and a 7-day blackout after any crisis event, so it can never talk over or duplicate a genuine LIFELINE response.
- `RECONCILIATION` and `USER_REGION` sheets (the latter for coarse, privacy-conscious crisis-resource localization — only ever populated when actually needed for a crisis response, not collected proactively on every turn).
- User-facing memory correction: **`/forget <n>`** and **`/fix <n> <text>`**.
- Admin-only **`/trust <user>`** and **`/trustreset <user>`**.

### Changed
- `askShayari()`'s entire gate ordering was restructured so crisis assessment runs **before every other gate** — Free Will lock, plan gate, and session-limit check all explicitly stand down when LIFELINE is engaged. This ordering, once established here, has not changed since.

### Why it mattered
This is the release where "a person in crisis should never be shown a paywall" stopped being an aspiration and became an architectural guarantee, checked first, every single turn. Everything the public-facing [[GOVERNANCE.md](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#)](#) document says about crisis priority traces directly back to this version.

<br/>

---

<br/>

## ⟩ v126 — Viewport & Notch Fix

<sub>📅 2026</sub>

### Fixed
- **`viewport-fit=cover`** added to the meta viewport tag in `Code.gs`. Without it, the CSS `env(safe-area-inset-*)` rules the responsive layer depends on silently resolve to `0` on notched or rounded-corner phones — the message composer sat under the home indicator, and the header sat under the notch.

### Why a one-line fix gets its own version entry
Small, but worth documenting on its own: this is a good example of a bug that's invisible in a desktop browser and on most Android devices, and only reproduces on a specific class of hardware (notched iPhones, some newer Android phones) — the kind of thing that's easy to ship without ever noticing, and easy to fix in one line once actually reproduced on the right device.

<br/>

---

<br/>

## ⟩ v127 — Reliability Pass: Locking, Trigger Orchestration, Reactions, Silence Awareness

<sub>📅 2026</sub>

A release driven almost entirely by real production symptoms rather than planned feature work — the pattern throughout is "trace the symptom to its actual root cause," not "patch what was reported."

### Added
- **`triggers.gs` (NEW)** — `installAllTriggers()` centralizes nine previously-scattered `setupXTrigger()` functions (spread across `proactive.gs` ×3, `emotion.gs`, `barometer.gs`, `chat.gs`, `free_will.gs`, `optimisation.gs`, `self_improvement.gs`) into one idempotent call. `listInstalledTriggers()` and `runFullBackfillNow()` (immediate backlog catch-up, no waiting for the next scheduled run) shipped alongside it.
- **`MAINTENANCE_LOG` sheet (NEW)** — one row per nightly maintenance run, turning "is this actually running?" into something answerable by opening a sheet instead of searching execution logs.
- **`REACTIONS.ACTOR` column** — the human's own reaction and Shayari's own reaction to the same message now coexist independently instead of one silently overwriting the other.
- **`autoReactAsShayari()`** — she reacts to reaction-worthy user messages on her own initiative now, using a deterministic per-message probability roll (not every message, and not random-each-time — the same message always resolves the same way), surfaced instantly client-side rather than waiting for the next poll.
- **Silence awareness** (`proactive.gs`) — `countConsecutiveUnansweredMessages()` tracks how many of Shayari's own messages in a row went unanswered. At 2+, she acknowledges the quiet naturally instead of repeating herself; at 3 (`MAX_CONSECUTIVE_UNANSWERED`), she stops initiating further outreach on that thread entirely until the user actually replies.
- **`adminRepairMemoryImportanceScale()`** — one-time backfill for `MEMORY.IMPORTANCE` values written on the wrong scale (see Fixed, below).

### Fixed
- **The `PENDING_MESSAGES` race condition, and the real root cause of `DELIVERED` staying `FALSE` forever.** `checkPendingMessages()` performed an unlocked read-entire-sheet → mutate → write-entire-sheet operation. Two callers polling within the same second could each read the same "before" snapshot and each write their own "after" snapshot back — the second write would silently revert the first's `DELIVERED = true` flip. This is exactly why some messages stayed marked undelivered even after being shown to the user. Fixed with `LockService.getScriptLock()`; the identical unlocked pattern in `toggleMessageReaction()` and the new `autoReactAsShayari()` got the same fix pre-emptively rather than waiting for it to fail the same way.
- **Memory importance scale corruption.** The extraction prompt never specified that `importance` should be on a 0–10 scale, so the model occasionally answered on a 0–1 scale instead — an easy mistake given `confidence` sits right next to it in the same JSON object *at* 0–1. This silently starved affected memories in retrieval scoring (`importance × 10` computed on an already-fractional value) and skewed the compaction threshold that decides what's safe to condense. Fixed at three layers: the prompt now explicitly states the scale, `normalizeImportance()` clamps/rescales on every write, and `parseMemoryRow()` normalizes on every read — plus the one-time repair function above for values already corrupted before the fix landed.

### Why it mattered
Two separate but structurally identical lessons in one release: silent failures in this stack (an unlocked write, an unvalidated numeric scale) don't announce themselves — they just quietly produce wrong results that look plausible until someone goes looking. Both fixes here are the direct result of tracing an observed symptom all the way to its mechanism instead of accepting the first plausible explanation.

<br/>

---

<br/>

## ⟩ v128 — Vision & Image Understanding

<sub>📅 2026</sub>

Shayari can see. The full arc of getting this right took three passes in the same release window — documented honestly below rather than presented as if it worked perfectly the first time.

### v128 — Initial ship

- **`vision.gs` (NEW)** — real image understanding via Gemini's multimodal vision, using the same `GEMINI_API_KEY` pool as text replies — no new API key, no new service. `analyzeAndStoreImage()` runs once, up front, in `askShayari()`, threading a plain-text description into the exact same persona/memory/emotion prompt pipeline every text-only reply already used. Not a separate bolted-on captioning bot.
- **`IMAGES` sheet (NEW)** — encrypted image storage (same AES-256 scheme as every other sensitive field), flat-capped at 15 uploads/user/day regardless of plan.
- **`CHATLOGS.IMAGE_ID` column (NEW)** — a pointer, not the image itself, keeping the hot per-turn chat-read path free of image bytes.
- **A deliberate non-feature:** no persistent facial recognition, no face-embedding storage, no cross-photo identity matching — considered and explicitly rejected during design, not a missing capability slated for later. Three concrete reasons, documented in the file header: biometric data carries its own regulatory burden (GDPR, BIPA, India's DPDP Act) that a companion feature shouldn't casually acquire; a Google Sheet is the wrong storage tier for identity templates regardless of encryption; and a photo often contains people who never consented to using the product at all.
- Client-side: a genuinely functional image upload button — replacing what had actually been a non-functional stub that displayed a filename badge and sent nothing anywhere.

### v128.1 — Same-day fix: diagnosability

The most common real-world failure mode for this feature turned out to be mundane: a deployed web app still running pre-v128 code, silently ignoring the two new arguments `askShayari()` now accepted (Apps Script doesn't error on unrecognized extra function arguments — it just ignores them). Added unconditional logging on both ends — server-side (`Logger.log('[VISION] askShayari received: ...')`, fires every turn, image or not) and client-side (`console.log('[image] attached: ...')`, fires right before the call) — so a stale deployment became immediately distinguishable from an actual logic bug, instead of both looking identical from the outside ("Shayari has no idea what image I'm talking about").

### v128.2 — Same-day fix: text legibility and a sizing-math bug

Two compounding, genuine bugs, both caught and fixed the same day they were reported:

1. **OCR/text-in-images was unreliable.** Client-side compression's fallback ladder decayed resolution too aggressively (1024px → 768 → 512 → 384 → 256), and most real photos didn't fit within budget on the first attempt — landing at 256–384px, where small text is destroyed regardless of source sharpness. Replaced with a more gradual ladder (1600 → 1400 → 1200 → 1000 → 850 → 700 → 550 → 420 → 320px) so a typical screenshot or document photo now lands around 1200–1600px instead. Separately, the vision prompt had buried "any visible text" as one clause inside a general description request — split into its own explicit instruction demanding complete, verbatim transcription, with `maxOutputTokens` raised from 500 to 1200 (text-heavy images were being cut off mid-transcription) and `temperature` lowered from 0.4 to 0.2 for transcription faithfulness over creative phrasing.
2. **A real sizing-math error.** The server-side size ceiling (`IMAGE_MAX_BASE64_CHARS`) had been set to 40,000 — but running the actual encryption-overhead formula (`(L + 64) × 4/3 + 5` for `encryptField()`'s IV, HMAC tag, padding, and base64 expansion) shows a 40,000-character image could produce a ~53,400-character encrypted string, *over* the ~50,000-character Google Sheets cell limit — on an image that was itself under the "allowed" threshold. Corrected to 36,000, with the client-side target tightened to match at 34,000, both now derived from and documented against the real formula rather than a round number.

### Why it mattered, and why it's documented this way
This release is presented with its two same-day follow-up patches shown honestly, rather than folded silently into a single clean "v128 — it just worked" narrative. That's a deliberate choice: the actual, useful lesson from this release isn't "vision works now," it's "compression choices have to be justified against what the feature actually needs (legibility, not just file size), and every constant touching a hard platform limit needs to be checked against the real math, not assumed." Both of those are exactly the kind of thing worth a future contributor being able to see happened, not just that it eventually got fixed.

<br/>

---

<br/>

## ⟩ Closing Note

If you're reading this looking for what changed in a version not listed with real detail here (anything before v85), that information doesn't exist in a form this document could honestly present — see [[A Note on How This Document Was Built](https://claude.ai/chat/86023b12-f415-4af7-be3c-39055261be6d#-a-note-on-how-this-document-was-built)](#-a-note-on-how-this-document-was-built) at the top. Everything from v85 onward is accurate to the actual codebase as of this writing.

For the terse, changelog-format version of this same history, see `CHANGELOG.md`. For how these systems fit together architecturally, see `ARCHITECTURE.md`, `MEMORY_SYSTEM.md`, and `EMOTION_ENGINE.md`.

<br/>

---

<div align="center">
<sub>© 2026 Writistic Studios LLP · OpenNHE Technologies · Algotheorem Labs</sub>
</div>

**Full Changelog**: https://github.com/itsppm76/shayari-nhe-01/commits/v128.2
