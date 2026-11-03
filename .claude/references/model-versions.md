<!-- CURRENT: Fable 5.1 | Opus 5.5 | Sonnet 5.5 | Haiku 5.5 -->

# Current Model Versions (single source of truth)

**Last verified against Anthropic docs:** 2026-11-03 (platform.claude.com/docs/en/models/overview)  
**Expires:** 2027-01-02 (60 days) — after this date the currency gate fails until re-verified.

This file is the **one place** that names current Claude model point versions. Everything else in the template should either refer to tiers abstractly ("newest Opus", "the Haiku tier") or point here. `scripts/check-model-versions.sh` flags any **superseded** version that is presented as **current** in the template's user-facing surfaces.

The machine-readable `<!-- CURRENT: ... -->` marker at the top is parsed by the checker — keep it in sync with the table.

| Tier | Current version | Model ID | Notes |
|------|-----------------|----------|-------|
| Fable (hardest, long-horizon) | **Fable 5.1** | `claude-fable-5-1` | "for demanding reasoning and long-horizon agentic work"; $10/$50 per MTok; 1M context, 128k max output; adaptive thinking always on; default effort `high` |
| Opus (high-judgment) | **Opus 5.5** | `claude-opus-5-5` | Anthropic's recommended starting point for most workloads; $4/$20 per MTok; 1M context, 128k max output; adaptive thinking always on; default effort `medium` |
| Sonnet (workhorse) | **Sonnet 5.5** | `claude-sonnet-5-5` | "best combination of speed and intelligence"; $2/$10 per MTok; 1M context, 128k max output; adaptive thinking; default effort `high` |
| Haiku (fast / mechanical) | **Haiku 5.5** | `claude-haiku-5-5` | high-volume, latency-sensitive tasks; from $0.10/$0.50 per MTok; 1M context, 128k max output; default effort `medium` |

All four: reliable knowledge cutoff June 2026 (verified 2026-11-03).

**Not re-verified in the 2026-11-03 pass — do not quote until checked:** alias resolution per provider (`opus`, `sonnet`, `best` on the Anthropic API, AWS, Bedrock, Google Cloud, Foundry), fast-mode pricing, release dates, and the minimum Claude Code version each model needs. The 2026-08-21 values for the prior generation are historical and no longer apply to the current lineup.

**Fable routing caveat — needs re-measurement.** [`model-routing.md`](../rules/model-routing.md) keeps the Fable tier off the forked-reviewer fleet on the basis of a **launch-week (2026-06) observation on Fable 5** (28/28 structured-output subagent failures vs 0 on Opus). That evidence predates Fable 5.1 and Opus 5.5. **Treat the routing rule as unverified until re-measured.**

**The advisor tool** (last verified 2026-08-21, experimental, Anthropic API only; not re-checked for the current lineup): set `advisorModel` or `/advisor`, and the main model consults a stronger advisor at hard decisions.

## Prior generations

It is fine to mention older versions in **historical** contexts (CHANGELOG entries) or in explicit **"prior generation" / comparison** lines (e.g. "Opus 4.8's `high` does what Opus 4.7's `xhigh` did"). They must **not** be presented as the current / newest / default model.

- **Fable 5**, **Opus 5**, **Sonnet 5**, **Haiku 4.5** — prior generation (current until the Fable 5.1 / Opus 5.5 / Sonnet 5.5 / Haiku 5.5 lineup; superseded as of the 2026-11-03 verification).
- **Opus 4.8** — prior Opus generation (was the default until Opus 5 shipped Week 30, 2026).
- **Opus 4.7**, **Opus 4.6** — prior Opus generations.
- **Sonnet 4.6**, **Sonnet 4.5** — prior Sonnet generations; still what `sonnet` resolves to on some third-party providers (see alias table above).

The checker allows a line to mention an older version when it carries a marker such as `prior generation`, `retire`, `migrat`, `deprecat`, `or later`, `historical`, an `X.Y's` comparison, or an inline `<!-- model-allow -->` comment.

## Update protocol (when Anthropic ships a new model)

1. Update the table **and** the `<!-- CURRENT: ... -->` marker above, plus the "Last verified" date.
2. Run `./scripts/check-model-versions.sh` and fix every current-state surface it flags.
3. **Manually grep for superlatives** — `grep -rniE "newest|most capable" README.md CLAUDE.md guide/ docs/index.html .claude/rules/` — and re-verify each hit. The checker validates *version strings*; a claim like "X is the newest model" is a **semantic** assertion it can only partially catch (it flags `newest`/`most capable` lines that name a non-top tier, but tier-relative phrasings like "the newest Opus" are legitimately allowed). The 2026-06-09 Fable 5 launch made exactly this class of claim false while the gate stayed green.
4. Add a "Changed — model refresh" entry to `CHANGELOG.md`; leave historical CHANGELOG entries intact.
