# GSA-GATEWAY

**Name is provisional.** This repo consolidates GSA-lineage content that
was previously scattered across many separately-archived repos with no
unified home. Renaming a GitHub repo afterward is trivial (Settings →
rename), so this name wasn't a blocker on doing the consolidation.

## Contents

- `secure/` — the full former [SECURE](https://github.com/wking53214/SECURE)
  repo: a working SECURE-sandbox + GSA-adapter kernel.
- `uztc/` — the full former [UZTC](https://github.com/wking53214/UZTC)
  repo: the Universal Zero-Trust Construct, a 7-layer registry including
  a named Provenance layer.
- `wrapper/` — the full former [WRAPPER](https://github.com/wking53214/WRAPPER)
  repo: the GSA Universal Interlock Wrapper + sandbox kernel. Confirmed
  origin session for `secure/`'s sandbox-kernel content — keep
  `wrapper/artifact_3.py` as canonical; it has real line breaks where the
  copy under `secure/artifact_1.py` was flattened.
- `governance-stack/` — governance-stack material pulled from two other
  repos:
  - From DGK: the `SovereignGovernanceStack` architecture sketches and a
    CITADEL-derived processor/router. All now under
    `governance-stack/archive/` — see the cleanup note below.
  - From EDDP (formerly Data_files): `governance_os_security_source.py`/
    `_adapter.py` (a working `BaseGate`/`BoundaryGate`/`InvariantGate`
    pattern), `solvar_stability_governance_source.py`/`_adapter.py`/
    `_cleanup.py` (connects to "Fortress/SOLVAR" per DIT's own
    provenance notes), and `quorum_state_governance_source.py`/
    `_adapter.py` (moved here provisionally as GSA-lineage content
    regardless of the still-open question of whether it's also ATS's
    missing `gov4_kernel` dependency — checked once already: the import
    names didn't line up cleanly, see ATS's `ats_seam_inventory.md`).
  - `_source.py` files are the flattened raw pastes (preserved evidence,
    several do not parse); the matching `_adapter.py` files are the working
    reconstructions.

### governance-stack cleanup, step 1 (2026-09-02)

The first reconciliation pass over `governance-stack/` — no longer "a
separate, later pass":

- **`code-repo-governance-and-gsa-core.py`** — added `GsaCoreController` +
  `GsaTemporalDoorwayGate` (reconstructed from CODE's flattened paste) and
  the registry/decorator, `gsa_deep_freeze`, and `@dataclass` the file
  needed to import at all.
- **`vanguard-behavioral-simulation.py`** (new) — VANGUARD's behavioral
  kernel, reconstructed from its flattened source. Its
  `PipelineCycleManager` is renamed `VanguardBehavioralPipeline` here (name
  clash with the unrelated one in the GSA core file).
- **`sovereign_kernel.py`** (new) — a working `UnifiedSovereignKernel` that
  wires perimeter (VANGUARD) → linguistic (a labelled pass-through hook, not
  yet built) → master (GSA doorway handshake) → execution (quorum
  consensus). The running version of what the archived sketches described.
- **`agent-factory-tactical-agents.py`** — one-line import fix (`Any`).
- **`governance-stack/archive/`** (new) — 5 files moved out with a README:
  the three `sovereign-governance-stack*` / `unified-sovereign-kernel-wrapper`
  sketches, the diverged `citadel-processor-router-flattened.py` variant, and
  the unreferenced `resilience-config-dataclass.py`.

Still not done: the linguistic scrub (needs CITADEL's engine), the
`_source.py` reconstructions, and reconciling `quorum_state_governance`
against ATS's `gov4_kernel`.

The original three repos (`SECURE`, `UZTC`, `WRAPPER`) still exist on
GitHub but are now empty of code — left in place for William to review
and delete manually, not auto-deleted.

### governance-stack cleanup, step 2 (2026-09-03)

- **`code-repo-governance-and-gsa-core.py`** — fixed two pre-existing bugs
  (not introduced by step 1): undefined `logger` across five classes, and
  `PipelineCycleManager` mutating an immutable `MappingProxyType` default.
  A third gap, four methods referencing an undefined `SYSTEM_GLOBALS`, is
  documented but not fixed — out of scope, no definition exists anywhere.
- **`sre_system_resilience_evaluator_adapter.py`** (new) — SRE reconstructed
  from `sre-system-resilience-evaluator-flattened.py` (confirmed non-parsing).
  Along the way, SOLVAR's `LyapunovStabilityModule` and SRE's
  `SystemStabilityValidator` turned out to implement the identical weighted-
  deviation energy formula with identical thresholds (`1e-4`/`1e-2`) — the
  same design, reskinned per source repo, not a coincidence.
- **`resilience_stability_kernel.py`** (new) — that shared formula, extracted
  once, property-verified, and reused by the SRE reconstruction instead of
  duplicated inline. SRE's own weights, field mapping, and output are
  unchanged from a literal reconstruction (regression-checked against the
  original numpy formula); its thresholds are now configurable via a
  `StabilityThresholds` dataclass instead of hardcoded literals. SOLVAR's own
  file is untouched.
- **`ure-universal-resilience-engine-flattened.py`** — moved to `archive/`
  as evidence-only: unreferenced by anything in this repo, and its regime
  classifier is hardcoded rather than derived from its inputs. See
  `governance-stack/archive/README.md`.
- **`sovereign_kernel.py`** — wired the SRE stability precheck in as step 0,
  before the perimeter, per the design in the archived v1/v2 sketches — with
  one documented divergence: verification found `EvaluationVerdict.CRITICAL`
  is mathematically unreachable through SRE's `evaluate_system_telemetry()`
  as originally designed (DEGRADED's energy gate always trips first, proven
  by exhaustive search — see that file's `__main__`). Rejecting on CRITICAL
  alone would be a decorative check that can never fire, so the kernel
  rejects on DEGRADED or CRITICAL instead. SRE's own file, weights, and
  thresholds are untouched; this is a wiring-layer decision only.

Still not done: the `_source.py` reconstructions, and the
`quorum_state_governance` / ATS `gov4_kernel` question.

### governance-stack cleanup, step 3 (2026-09-03)

- **`citadel_v1.2.py`** (new) — vendored verbatim from
  [CITADEL](https://github.com/wking53214/CITADEL) (commit
  `81f0b817e864e5fdf73743e859d23321d355119e`), not the diverged model-retry
  variant already in `archive/`. Vendored rather than a live dependency: this
  repo has no packaging infrastructure anywhere, and every other
  governance-stack layer is already loaded as a local sibling file the same
  way. CITADEL stays its own separate repo deliberately — it's a
  general-purpose LLM-output enforcer, not GSA-specific, unlike everything
  else folded into this consolidation.
- **`sovereign_kernel.py`** — `_linguistic_scrub` is wired to CITADEL's real
  `Citadel.enforce()` instead of the pass-through placeholder. Verified by
  running it: a request with hedging/identity language is actually rewritten
  end to end through the master and execution layers.

Checked while deciding how to wire CITADEL in: no code anywhere in this repo
actually imports CITADEL's real engine today. The "Citadel" name in a couple
of `governance-stack` files (`governance_os_security_adapter.py`,
`agent-factory-tactical-agents.py`) is an unrelated stub doing a single
substring check, not CITADEL's regex engine — another instance of the
naming-coincidence pattern already noted for GOV4. This README's claim below
that `wrapper/`'s content "includes 'Zero-Trust Citadel Linguistic
Interceptors' as a component" isn't borne out by `wrapper/`'s actual files
(no match for "linguistic" or "interceptor" anywhere in them) — likely
aspirational language from the original transcript that was never
implemented. And CITADEL's own README states STRIDE, the one repo it
documents as a real historical consumer, has already been deleted — the
"Related repos" section below, which still describes STRIDE as a live
separate repo, is stale. Both left as-is, flagged for William rather than
silently fixed, alongside the other loose ends from this reconciliation pass
(the now-empty SECURE/UZTC/WRAPPER repos, the still-provisional repo name).

Still not done: the `_source.py` reconstructions, and the
`quorum_state_governance` / ATS `gov4_kernel` question.

## Related repos, not folded in here

- [CITADEL](https://github.com/wking53214/CITADEL) — a Deterministic LLM
  Output Enforcement Engine, substantial enough to stay its own repo.
  `wrapper/`'s content includes "Zero-Trust Citadel Linguistic
  Interceptors" as a component, consuming CITADEL's enforcement logic.
- [STRIDE](https://github.com/wking53214/STRIDE) (renamed from CLIP) — a
  gateway system, also substantial enough to stay its own repo, also
  consuming CITADEL's enforcement logic.

The initial consolidation (commits through `f0934b1`) was a pure content
move — files relocated, git history not preserved cross-repo, no code
edits. The `governance-stack/` cleanup described above is the first pass of
the follow-up code work.
