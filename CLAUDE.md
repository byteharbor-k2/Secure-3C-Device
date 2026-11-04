# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **documentation-only** research/ops handbook (written mostly in Chinese), not a source-code repo. There is no build, lint, or test tooling. `tools/` is a placeholder and `apk/` holds only an install guide. The subject is privacy hardening of one device: OPPO Reno9 5G / ColorOS 15 (Android 15, Bootloader locked, no official unlock), done without root or flashing, using `ADB + network hardening + app replacement + space isolation`.

Keep the existing language (Chinese prose, English commands/package names) when editing docs.

## Layout

- `README.md` — project home: threat model, hardware baseline, four-layer defense summary, conclusions, citation list. Other docs must not contradict its conclusions.
- `AGENT.md` — the existing collaboration handbook (written for Codex). Its rules apply here too; it is the source for the conventions below. Note `README.md` links to it as `CODEX.md`, which does not exist, so fix those links when touching them.
- `docs/01-system-analysis/` — AOSP vs ColorOS multi-user / System Cloner (User 10) analysis, including `system-clone-limitations.md`.
- `docs/02-network-security/` — DNS and WebRTC leak prevention, proxy/VPN.
- `docs/03-app-management/` — `Chapter_1/` (debloat logs, disabling install scanning, app stores), `Chapter_2/` (email, GMS), and `user_main_system.md` / `user_system_clone.md` per-user app lists.

## Key conclusion to preserve

The System Cloner was originally the "main battlefield", but testing showed it cannot enable developer options directly, has limited ADB availability once switched to it, and has per-user VPN routing problems. Deploy across **main system / System Cloner / PC-side TUN proxy** depending on the scenario. Docs may keep both the initial design and the retrospective, but must not present the clone as the only or default option.

## Documentation conventions

- Label content as one of: theory/analysis, on-device verified result, or unverified hypothesis.
- When a conclusion changes, edit the doc directly instead of leaving stale text. After any directory change, fix indexes (`docs/README.md`, per-section READMEs) and relative cross-links first.
- Give rollback steps for every ADB operation.
- Redact: never record IMEI/IMSI/Android ID/serial numbers. Use placeholders such as `<PHONE_IP>` and `<PC_IP>` for LAN IPs and ports. Keep screenshots and logs free of real account names, phone numbers, and payment info.

## ADB safety boundary

Helping with package lists, command lists, rollback scripts, and sorting `pm` results (`disable-user` / `suspend` / failed) is fine. Confirm with the user before running or recommending `pm disable-user|uninstall|suspend`, `settings put global ...`, `adb install`, or anything that could affect stability, payments, or calling. By default, do not recommend removing or disabling `com.android.*` core components or core `com.google.*` GMS services, and do not batch-clean without a backup and a recorded rollback.

## Maintenance priorities

1. `README.md` strategy and conclusions, kept consistent with the chapters.
2. `docs/03-app-management/Chapter_1/1_debloat-main-system.md` (test records and status table).
3. `docs/01-system-analysis/system-clone-limitations.md`.
4. Relative paths and cross-references across all docs.
