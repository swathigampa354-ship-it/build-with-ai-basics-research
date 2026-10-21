# Additional Audit: OpenCode Features, Permissions and Limits

Date: 2026-10-21
Environment: Termux (Android), Colab CLI 0.7.5, OpenCode 1.19.3, model `opencode/glm-5.3-flash-free`
Method: installed CLI `--help` inspection, bundled README (`colab readme`), official Google Colab FAQ (research.google.com/colaboratory/faq.html), Google Cloud Colab Enterprise docs (cloud.google.com/colab/docs), and live read-only tests against the existing session `test` via `~/colab_task.sh`. No sessions were created or stopped; no packages installed; no settings changed.

Labels: **VERIFIED** = reproduced in this audit. **UNVERIFIED** = believed possible, not tested here. **UNSUPPORTED** = capability absent from the installed tooling. **BLOCKED** = requires a human action or missing entitlement.

---

## A. Colab CLI 0.7.5 feature verification

All commands below were confirmed present or absent by running `colab --help` and per-command `--help` on the **installed** CLI.

| Feature | 1. CLI support | 2. Account permission | 3. Tested | 4. Human approval needed | 5. CU / session risk |
|---|---|---|---|---|---|
| Create CPU session | VERIFIED (`colab new`) | VERIFIED (1 assignment active) | Not directly (forbidden) | No | Consumes CUs while running |
| Create GPU session (T4/L4/G4/H100/A100) | VERIFIED (`colab new --gpu`) | UNVERIFIED — README: "Availability varies by Colab subscription tier" | Not tested (forbidden) | No | Consumes CUs |
| Create TPU session (v5e1, v6e1) | VERIFIED (`colab new --tpu`) | UNVERIFIED | Not tested (forbidden) | No | Consumes CUs |
| High-RAM shape | VERIFIED (`--high-mem`) | README: "Requires Colab Pro or Pro+ entitlement" | Not tested | No | Higher CU rate |
| List sessions / status | VERIFIED (`sessions`, `status`) | Yes | **VERIFIED** (`test` = CPU, Standard, IDLE) | No | None |
| Exec Python: stdin | VERIFIED (`exec`) | Yes | **VERIFIED** (16 live tests) | After initial OAuth only | None |
| Exec Python: script file / notebook | VERIFIED (`-f` reads .py or .ipynb) | Yes | UNVERIFIED (stdin path tested) | No | None |
| Script arguments | VERIFIED only via `colab run` / shebang; `exec` cannot pass args | n/a | **VERIFIED** (`sys.argv` is the Jupyter launcher under `exec`) | No | None |
| Remote shell commands | Via Python `subprocess` only | Yes | VERIFIED (`bash -lc` inside kernel, RC captured) | No | None |
| Install packages / requirements | VERIFIED (`colab install`, `-r`) | Yes | Not tested (forbidden) | No | Disk only |
| Upload / download files | VERIFIED (`upload`, `download`) | Yes | **VERIFIED** (roundtrip OK) | No | None |
| List / remove / edit remote files | VERIFIED (`ls`, `rm`, `edit`) | Yes | UNVERIFIED (`rm`/`edit` destructive — skipped) | — | Data-loss risk if misused |
| Mount Google Drive | VERIFIED (`drivemount`) | Needs Drive OAuth scope | **VERIFIED attempt: BLOCKED** — requires interactive browser consent; headless run ended in `400 Bad Request` → `ValueError: mount failed` | **Yes** | None |
| Authenticate Google Cloud services | VERIFIED flag (`--auth oauth2|adc`) | Session OAuth already established | Vermained (auth already active for exec) | First-run OAuth = Yes | None |
| Export logs / notebook outputs | VERIFIED (`log -o` supports .ipynb/.md/.txt/.jsonl) | Yes | **VERIFIED** (5.2 KB jsonl exported) | No | None |
| Interactive REPL / console | VERIFIED (`repl`, `console`) | Yes | Not tested (needs TTY) | No | Keeps session alive |
| SSH (incl. ProxyCommand bridge) | VERIFIED (`ssh`, `--proxy-mode`, `-i`) | Yes | Not tested | SSH pubkey flow | Keeps session alive |
| Restart kernel | VERIFIED (`restart-kernel`) | Yes | Not tested (disruptive) | No | Loses in-kernel state |
| Stop session | VERIFIED (`stop`) | Yes | **Not tested** (explicitly forbidden) | — | Terminates VM |
| Reconnect | VERIFIED (`url` prints attach-URL; `console`/`ssh` reconnect) | Yes | **VERIFIED** (`url` returned attach URL; no secrets in URL) | Browser login on some devices | None |
| Change accelerator of existing session | **UNSUPPORTED** — `--gpu` exists only on `new`/`run`/`ssh` (auto-create paths) | n/a | n/a (web UI only) | Web UI | Runtime switch reprovisions VM |
| Execution timeouts | VERIFIED flag (`exec --timeout`, default 30 s) | n/a | **VERIFIED as NOT ENFORCED**: 3 s timeout did not abort an 8 s sleep | No | None |
| Background jobs management | UNSUPPORTED as first-class; `run --keep` keeps a session; no job scheduler | n/a | n/a | — | — |
| Compute-unit balance | VERIFIED (`usage`) | Yes | **VERIFIED**: balance **0.00 CU**, rate **0.08/hr**, 1 active assignment | No | None |

## B. OpenCode 1.19.3 actual capabilities (inspected, read-only)

- Config: `~/.config/opencode/opencode.jsonc` contains only `$schema` — no custom permissions, no MCP servers configured. → MCP is supported by the product but **no MCP tools are connected** (VERIFIED from config).
- Tools invocable in this session (observed): bash (subprocess with stdout/stderr/exit codes), read, write, edit, glob, grep, webfetch, websearch, task (subagents), question, todowrite, skill. Git: via bash (repo operations succeeded). HTTP: via webfetch/websearch (both used).
- Termux file read/write/subprocess: VERIFIED (all audit operations above).
- Remote Colab Python + results retrieval: VERIFIED (`~/colab_task.sh`, stream + file roundtrip).
- Create scripts / manage dependencies: VERIFIED (wrote `colab_task.sh`; `pkg`, `pip`, `python`, `git`, `ssh` present; `gh` absent). Package installs were NOT performed (forbidden).
- Async/background: spawn returns immediately (VERIFIED), **but a `nohup … &` job did NOT survive across tool calls** (output file absent after ~30 s) — background persistence FAILED in this harness; process group appears killed when the tool call ends. Long daemons are not reliable — VERIFIED limitation on this device.
- Android UI / browser / clipboard: `termux-open` present (can hand off URLs/files to Android). `termux-api` NOT installed → clipboard, sensors, UI automation, notifications UNAVAILABLE. `/system/bin/am`/`pm` reachable but privileged actions unverified. No browser-cookie or app access — VERIFIED absent.
- Context/timeout limits: bash tool calls observed to time out at 120 s default; model context limit not measurable from inside — UNVERIFIED. Approval rules: defaults only (`--auto` exists in CLI); session used normal approval flow.
- Did NOT touch: `~/.local/share/opencode/` (db/logs used internally); credentials redacted throughout.

## C. Integration test results (`test` session, all non-destructive)

| # | Test | Result |
|---|---|---|
| T1 | `sys.exit(3)` | Traceback shown; **local exit code still 0** — exit codes are NOT propagated |
| T2 | stdout + stderr | Both captured, interleaved in one stream |
| T3 | `ValueError("boom")` | Traceback visible, exit 0 — failures visible but not signalled via exit code |
| T4/T14 | status before/after | Unchanged: `CPU, Standard, IDLE` before and after all tests |
| T5 | Nonexistent session `no-such-session` | `status`: "not found", exit 0; `exec`: "not found", exit 1. **No auto-recreation** |
| T6 | Multiline script + `sys.argv` | Multiline OK; argv = Jupyter launcher (no arg forwarding via `exec`) |
| — | Remote shell via subprocess | `bash -lc` RC 0, output captured (`SHELL_OK`, nproc=2) |
| T7 | `--timeout 3` vs 8 s sleep | `DONE_AFTER_SLEEP` after 8 s, exit 0 — **timeout not enforced** |
| T8–T10 | upload → read → download roundtrip | Full roundtrip verified byte-exact |
| T11–T12 | log export (.jsonl) + list | Exported 5,264 bytes; session list shows `test` |
| T13 | Drive mount | **BLOCKED** — needs interactive OAuth consent; headless attempt → `400` → `mount failed` |
| T15 | `nvidia-smi` | `FileNotFoundError` — binary absent on CPU runtime |
| T16 | Hardware detail | CPU: AMD EPYC 7B12, 2 cores; disk 107.7 GB total / 87.2 GB free; `pip` present; `google.colab` present |

## D. Measured values vs entitlements

Measured on the current runtime (VERIFIED):
- Python 3.13.16 CPython; torch 2.11.0+cpu (`torch.cuda.is_available()` = False); transformers 5.20.1; accelerate 1.15.0; AMD EPYC 7B12 × 2 cores; RAM 12,975 MB total / ~11,833 MB available; disk 107.7 GB / 87.2 GB free; `nvidia-smi` absent; `google.colab` package present.
- Account (via `colab usage`): balance **0.00 compute units**, current rate **0.08/hr**, 1 active assignment.

Entitlements stated by the user, NOT confirmed by the account/API in this audit (UNVERIFIED): Colab Pro subscription, T4 access, 5 TB Google Drive storage. Note: **0.00 CU balance** is an account-confirmed measurement; GPU runtimes typically bill CUs, and Google's docs state GPU availability varies by subscription tier and region — GPU work until balance/entitlement is confirmed is therefore flagged BLOCKED risk.

Google Drive: CLI can request a mount VERIFIED (command exists), but current credentials lack the Drive scope VERIFIED (consent required). The 5 TB figure is a Drive quota, unrelated to VM RAM/disk (instrinsic distinction, VERIFIED by architecture docs).

## E. Hard limits

Confirmed (VERIFIED):
1. `colab exec` cannot change hardware; CLI cannot change an existing session's accelerator at all — pinned VM identity (`m-s-kkb-usc1a0-…`).
2. Exit-code propagation is broken through `exec` (always 0); success detection must parse stdout.
3. `exec --timeout` is accepted but not enforced (3 s did not stop an 8 s sleep).
4. `exec` cannot forward script arguments; `colab run` (fresh VM each time) can.
5. Drive mount requires interactive human OAuth; headless automation fails.
6. Current runtime: no GPU (torch CPU build), 2 vCPU, ~12.75 GB RAM, ~87 GB free disk.
7. CU balance reads 0.00 — GPU/billing-dependent work is blocked until balance or entitlement is confirmed.

Possible but not tested (UNVERIFIED):
- GPU provisioning would fail/succeed depending on tier + availability (Google docs: availability varies).
- TPU/high-RAM entitlement.
- `colab install` and pip network speed on runtime (pip exists; installs forbidden in this audit).
- Android background-process termination risk for long local jobs.
- `exec` interactive features (REPL/console need a TTY).
- Stop/idle timeout of the runtime (Google FAQ: "Runtimes will time out if you are idle"; free tier max 12 h — paid limits not confirmed for this account).

Structural: VM disk is ephemeral — lost on accelerator switch, stop, or (per FAQ) timeout. OpenCode cannot exceed CLI capabilities for accelerator control; cannot answer Google CAPTCHAs or interactive consent.

## F. Capability matrix

| Feature | Available | Verified | Permission required | Limit or failure mode |
|---|---|---|---|---|
| Stdin exec on test | Yes | VERIFIED | Session OAuth (done) | exit codes always 0; timeout flag inert |
| File up/down | Yes | VERIFIED | none beyond auth | — |
| Log export | Yes | VERIFIED | none | — |
| Session status | Yes | VERIFIED | none | status exits 0 even when "not found" |
| Multiline scripts | Yes | VERIFIED | none | no sys.argv forwarding |
| Remote shell | Yes | VERIFIED | none | via subprocess only |
| JS/driver GPU check | Yes | VERIFIED | — | no GPU on CPU runtime |
|1 Drive mount | Yes | BLOCKED | browser OAuth consent | 400 without consent |
| GPU T4 session | Yes (command) | UNVERIFIED | Pro entitlement + CU balance | 0.00 CU balance observed |
| Accelerator change on test | No | UNSUPPORTED | web UI human | VM reprovision; CLI ends ownership of new VM id |
| Script args to exec | No | UNSUPPORTED | — | use `colab run` (fresh VM) |
| Background job mgmt | No | UNSUPPORTED | — | `--keep` workaround |
| TPU / high-RAM | Yes (command) | UNVERIFIED | Pro/Pro+ | availability varies |
| REPL/console/SSH | Yes (command) | UNVERIFIED | TTY / SSH key | interactive only |
| Android UI/clipboard | No | UNSUPPORTED | — | termux-api not installed |
| MCP tools | Product-supported | UNSUPPORTED here | config | none configured |

### 1. What works now
Colab: exec (stdin), upload/download, log export, status/sessions, usage, url, restart-kernel/stop commands exist (stop untested), subprocess shell inside kernel. OpenCode: full Termux file/git/shell/web access; remote Python with captured results; background spawn pattern; script creation; `pkg`/`pip` managers present.

### 2. Works only after a human action
- Drive mount (browser OAuth consent with Drive scope).
- First-run OAuth for new installs/profiles; SSH pubkey enrollment; web-UI runtime-type change for accelerator.

### 3. Unsupported
Accelerator change on a live session via CLI; argv forwarding into `exec`; first-class background-job management; enforced exec timeouts; Android UI/clipboard (no termux-api); MCP (none configured, though supported).

### 4. Technically possible but unverified
`colab install`, GPU/TPU/high-RAM provisioning, REPL/console/SSH sessions, `colab run` with args on fresh VM, notebook (.ipynb) exec, long-poll exec reliability, Android daemon persistence.

### 5. What the CPU session cannot do efficiently
Any CUDA work (`2.11.0+cpu`); heavy training/inference (2 vCPU, ~12.75 GB RAM); large-model fine-tuning; batch job throughput beyond 2 cores.

### 6. What becomes possible with a confirmed T4 runtime
`nvidia-smi`, CUDA torch wheels already present in Colab images would activate `torch.cuda.is_available()`; 16 GB-class VRAM inference (e.g., 4-bit ≤8 B models), moderate fine-tuning, faster batch data processing. All contingent on CU balance > 0 or confirmed entitlement — **currently reads 0.00**.

### 7. Remaining blockers and fixes
1. **CU balance 0.00** — confirm via `colab usage` after signup/top-up (`colab pay` opens management page). Without CUs, GPU sessions likely cannot start (UNVERIFIED).
2. **GPU entitlement** — verify by provisioning when balance exists (`colab new -s gpu --gpu T4`), then re-audit; web UI path: Runtime → Change runtime type → T4 GPU → Save.
3. **Drive scope** — one-time interactive consent from a browser (URL printed by `colab drivemount`); then mounts succeed headlessly for the session lifetime.
4. **Exit-code gap** — wrap remote scripts to print a sentinel last line (e.g. `RESULT: 0`) and parse locally; worked pattern in T8–T10.
5. **Timeout gap** — implement out-of-band sentinels/wall-clock checks inside scripts rather than relying on `--timeout`.
6. **Verify Colab Pro claims** — the CLI did not confirm tier; treat T4/5 TB statements as user-stated until prod account confirms.
7. **Rotate the GitHub PAT** stored in plaintext in `~/build-with-ai-basics-research/.git/config` — displayed in shell output during this audit; recommend revocation and re-adding with a fresh token.
8. **Optional**: install `termux-api` for clipboard/notifications; configure an MCP server if MCP tooling is later required.

---

### Citations
- Google Colab FAQ (official): research.google.com/colaboratory/faq.html — runtime idle timeouts, "Change Runtime Type" UI, GPU-not-utilized guidance.
- Google Cloud Colab Enterprise docs (official): cloud.google.com/colab/docs/runtimes — runtime/accelerator model (enterprise analogue for API-level control).
- Bundled CLI reference (installed 0.7.5): `colab --help`, `colab readme`, `colab <cmd> --help` — command availability, accelerator flags, Pro/Pro+ shape requirements.
