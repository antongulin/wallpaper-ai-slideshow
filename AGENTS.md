# AGENTS.md

Root DOX contract for this repository. It is binding for the whole tree: every work product, instruction, and durable doc must stay understandable from this file alone.

## Purpose

Windows desktop application that generates wallpapers with the OpenAI DALL-E 3 API and sets them as the desktop background.

- One Tkinter/ttkbootstrap GUI module with a system-tray menu (`pystray`).
- Each generated image is upscaled toward 4K with Pillow and rotated on a user-chosen timer.
- Distributed as a PyInstaller build driven by `wallpaper_ai_slideshow.spec`.

## Ownership

Maintainer and author: Anton Gulin (i@anton.qa, https://www.anton.qa).

- The repository is normally archived and read-only. Work only while a task explicitly restores it, and restore the archived state when that task ends.
- Every change lands on a task branch and a pull request. Executable changes (Python, `.spec`, workflow YAML) never go straight to `main`.
- Merge small cohesive pull requests with squash, subject to the repository's actual merge settings and required checks.
- Tracked files:

| Path | Role |
| --- | --- |
| `wallpaper_ai_slideshow.py` | Application: GUI, tray, generation, upscaling, wallpaper rotation |
| `wallpaper_ai_slideshow.spec` | PyInstaller build recipe, including Windows version-info metadata |
| `config.py` | Shared pricing constants for DALL-E 2 and DALL-E 3 |
| `requirements.txt` | Runtime and packaging dependencies |
| `app_icon.ico` | Application icon |
| `README.md` | User-facing description, costs, requirements, contact |
| `LICENSE` | MIT license, 2024 Anton Gulin |
| `.github/workflows/build.yml` | Windows build and release workflow |
| `.github/workflows/code-review.yml` | Caller for the shared Robin review workflow |
| `AGENTS.md` | Root agent instructions and DOX index |
| `docs/dox-LICENSE.txt` | Upstream DOX MIT notice |

## Local Contracts

### DOX discipline (binding)

- **Subtree contracts.** An `AGENTS.md` in a folder is that subtree's contract. The root applies everywhere; the nearest file controls local detail; no child may weaken this root.
- **Read before editing.** Read this root, then walk from the repository root to every path you intend to touch, reading each `AGENTS.md` on that route (including any child whose scope contains the path). Do not rely on memory: re-read the chain in the current session before editing, and re-read it after a long interruption.
- **Update after a meaningful change.** Run a DOX pass before the task is done: update the nearest owning `AGENTS.md` when a change alters purpose, ownership, structure, contracts, workflows, inputs, outputs, permissions, constraints, side effects, artifacts, or user preferences; update parents when their structure or child index changes; delete stale or contradictory text immediately; report docs intentionally left unchanged and why.
- **Hierarchy.** This root is the rail: project-wide rules plus the Child DOX Index. Child files own domain-specific rules and their own Child DOX Index. Each parent says what its direct children cover and what it keeps for itself.
- **Child shape.** When a child is created, use this order: Purpose, Ownership, Local Contracts, Work Guidance, Verification, Child DOX Index. Leave Work Guidance or Verification empty rather than inventing standards or checks.
- **Closeout.** Re-check changed paths against the chain, update affected docs and indexes, remove stale text, run existing verification where relevant, and state what was not verified.

### Shared workflow and attribution

- Shared agent workflow: read `~/.agents/instructions/WORKFLOW.md`, or its canonical copy at https://github.com/antongulin/baseline/blob/main/baseline/02-agents.md. Link these rules instead of copying them here.
- The DOX framework is MIT-licensed upstream software. The verbatim notice is kept at [`docs/dox-LICENSE.txt`](docs/dox-LICENSE.txt) and must stay byte-identical to upstream.

### Identity

- Author and copyright identity in this repository is **Anton Gulin**. The former handle does not appear in the tracked tree.
- Copyright year `2024` and application version `1.0.0` are historical facts and stay as they are.
- `LICENSE` already carries the correct holder. `README.md` already documents the current author, website, email, and links.

## Work Guidance

- **Identity-only edits.** Change the literal string and nothing else. Preserve every other byte, including comments, imports, runtime functions, packaging calls, and whitespace.
- **`wallpaper_ai_slideshow.spec` is not importable.** Executing or importing it deletes `build/` and `dist/`, then runs a PyInstaller `Analysis`. Never run it to check a change; parse it statically.
- **`version_info=VSVersionInfo` in the spec** is an existing keyword argument in the recipe, unchanged by identity-only edits. Keyword support and embedding these strings in an EXE are unverified here. A successful Windows build and resource inspection are needed before claiming either.
- **Dependencies are loose.** `requirements.txt` lists `pillow`, `requests`, `pywin32>=306`, `psutil>=5.9.0`, `cryptography>=41.0.0`, `pystray>=0.19.0`, `ttkbootstrap>=1.10.0` with no lockfile. Never state installed, resolved, or verified versions.
- **Windows-only at import time.** `win32api`, `win32con`, `win32gui`, `win32process`, `win32event`, `winerror`, and the desktop wallpaper calls mean the module cannot be imported on Linux or macOS. Do not attempt it as a check.
- **Runtime state stays out of Git.** The app writes `api_key.enc` and `encryption_key.key` in the working directory, `generated_wallpapers/` with `generated_wallpapers/metadata.json`, and scratch files under the system temp directory. API keys are stored encrypted with Fernet on the user's machine; never commit, print, or log key material.
- **Generation is paid.** A wallpaper calls the DALL-E 3 API, which the README prices at $0.040 per HD image. Never trigger generation, and never run the application, as part of verification.
- **`build.yml` is out of scope for unrelated tasks.** It runs on `windows-latest`, uses the retired `actions/upload-artifact@v3`, and publishes a GitHub Release on every push to `main`. Do not run or modernize it, and treat that auto-publish as a risk before any push to `main`.
- **`code-review.yml` calls the shared Robin reusable workflow.** Preserve its triggers, secrets, and permission grant; pass `runner` as a JSON-encoded string.

## Verification

- No test suite, linter, or CI check inspects module metadata. Do not add one for an identity-only edit, and never report the application as executed.
- `build.yml` is the only build automation, and it is Windows-only; it is not a verification step for this repository on any other platform.
- Safe static checks for metadata-only changes, run from the repository root:
  - `git grep -niE "anton[[:space:]]+rosh"` must return no tracked match.
  - `python3 -c "compile(open('wallpaper_ai_slideshow.py').read(), 'wallpaper_ai_slideshow.py', 'exec')"` parses the module without importing it.
  - `python3 -c "import ast; ast.parse(open('wallpaper_ai_slideshow.spec').read())"` parses the spec without running it.
  - `ruby -ryaml -e 'YAML.load_file(".github/workflows/code-review.yml")'` parses the review caller.
- Static parsing proves only that text parses and unchanged surrounding bytes still match. It does not prove compiled Windows resources, runtime behavior, or a release artifact.

## Child DOX Index

None. One owner, no nested durable boundary. Add a child `AGENTS.md` only when a directory gains its own rules, workflow, or quality bar, such as a future test suite.

## Source Provenance

- Framework: [agent0ai/dox](https://github.com/agent0ai/dox) at revision `765ae4ac02cc884eefcd41a3d0f71941721adb89`, retrieved 2026-11-04.
- Upstream `AGENTS.md` blob `6f3bf78cc8d9e8ccfc5223313c136ae265c380a7`; upstream `LICENSE` blob `6db3417bfb43a3e4e83afe0782414cca1f222053`.
- The discipline above is adapted to this project; the local license notice is copied verbatim from that revision.
