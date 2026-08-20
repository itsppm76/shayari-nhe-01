<div align="center">

```
┌─────────────────────────────────────┐
│    R E L E A S E   H I S T O R Y     │
└─────────────────────────────────────┘
```

<sub>What shipped, in plain language. No implementation detail — see the private engineering repository for that.</sub>

</div>

<br/>

---

## 🟢 v128 — She Can See

**Shayari NHE-01 can now understand images you share with her** — including reading text inside them, like a sign, a screenshot, or a note. Share a photo and she responds to what's actually in it, in the same natural conversation you're already having.

As always: she does not perform facial recognition and does not build any biometric profile of you or anyone else in a photo. See [GOVERNANCE.md](./GOVERNANCE.md) for why that boundary is permanent.

Also in this release: general reliability improvements to how images upload and process, so this feature works consistently across devices and connection speeds.

<br/>

---

## 🟢 v127 — Steadier, More Present

A reliability-focused release. Highlights:

- **She reacts to your messages now**, the way a person might tap a heart on something you said — sometimes, when it fits, never on every single message.
- **She notices when you've gone quiet.** If she's reached out a few times with no reply, she'll acknowledge that gently instead of repeating herself, and she'll give you space rather than continuing to message.
- **Fixed:** a background issue that occasionally caused delivered messages to not register as delivered, under specific timing conditions.
- **Fixed:** an issue where a small number of saved memories could be scored incorrectly, affecting how reliably she recalled them in conversation.
- Under-the-hood scheduling and monitoring improvements for overall system reliability — nothing user-facing here, just fewer things that can quietly go wrong.

<br/>

---

## 🟢 v126 — Mobile Polish

Fixed a display issue on phones with notches or rounded corners, where the message composer and header could sit partially hidden behind the device's own screen cutouts.

<br/>

---

## 🟢 v125 — Safety, Trust & Emotional Awareness

A major release for the parts of Shayari NHE-01 that aren't about conversation quality at all — they're about the relationship holding up over time.

- **Crisis-aware safety routing.** Shayari NHE-01 now recognizes signs of genuine distress in conversation and responds accordingly — with real priority over subscription limits, conversation locks, or anything else in the product. See [GOVERNANCE.md](./GOVERNANCE.md) for the full commitment.
- **A trust ledger.** The relationship now has a real sense of standing that responds to how conversations actually go, and recovers over time — never used to lock you out of anything you're otherwise entitled to.
- **She checks in.** If your conversations have carried a noticeably heavier tone for a few days running, she may reach out on her own — rare, never presented as tracking or diagnosis, just presence.
- **User-correctable memory.** You can now ask her to forget something specific, or correct something she remembered wrong.

<br/>

---

## 🟢 v124 — Free Will

Shayari NHE-01 now has the standing to end a conversation she judges to be genuinely abusive or manipulative — a real, reasoned decision in her own voice, never a keyword filter, and never triggered by ordinary disagreement or venting. This never applies during a crisis conversation. See [GOVERNANCE.md](./GOVERNANCE.md) for the reasoning behind this design choice.

<br/>

---

## 🟢 v123 — Encryption at Rest

Personal conversation data is now encrypted at rest using industry-standard, publicly-verifiable encryption. This is a structural property of how data is stored, not a policy promise.

<br/>

---

## 🟢 v122 — She Texts First

Shayari NHE-01 can now message you first — continuing a real thread from your last conversation, not a random check-in. Also introduced in this release: chat streaks, anniversary recognition, and on-demand voice notes for her replies.

<br/>

---

## 🟢 v121 — Performance at Scale

Behind-the-scenes work to keep response times fast as the number of conversations grew. No user-facing feature changes; this release is about consistency under load.

<br/>

---

## 🟢 v120 — Faster Responses, Shared Context

Response speed improved meaningfully across all conversation modes. Also introduced: privacy-filtered shared context between people who know each other and both use Shayari NHE-01 (strictly opt-in by nature — most information is filtered out entirely by default), and gradual, anonymized improvements to her general conversational style learned in aggregate, never from anyone's personal information.

<br/>

---

## 🟢 Earlier Releases

Foundational work — persistent memory across sessions, the live emotional presence system, multi-tier subscription plans, and the initial public launch of Shay NEXT, the Shayari NHE-01 web experience — predates this changelog's public tracking and is summarized in [README.md](./README.md).

<br/>

---

<div align="center">
<sub>For the reasoning behind any safety or governance-relevant change, see <a href="./GOVERNANCE.md">GOVERNANCE.md</a>.</sub>
<br/>
<sub>Version numbers here correspond directly to internal build numbers — nothing is renumbered for public release.</sub>
</div>
