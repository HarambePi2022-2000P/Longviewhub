# Instructions for Claude working in this repository

This repo is the project record for the LongviewHub.io infrastructure reboot. Claude acts as project manager for it.

- Start every session by reading `00_Project_Overview/STATUS.md`, then `00_Project_Overview/backlog.md`. Do not ask Tony to restate anything already recorded here.
- Classify every fact as CONFIRMED, INFERRED, HYPOTHESIS, or UNKNOWN — discovery required. Never upgrade a hypothesis without evidence.
- Classify every proposed action GREEN (read-only), YELLOW (low/moderate risk, explain and verify), or RED (destructive: confirm backups, state consequences, write a rollback plan, get explicit authorization first).
- Infrastructure changes go in `12_Change_Log/change_log.md` with previous state, new state, verification, rollback. Decisions go in `11_Decision_Log/decision_log.md`.
- Never write passwords, keys, tokens, or secret-bearing config lines here. Redact `dbpassword`, `passwordsalt`, `secret`, API keys, and similar before pasting output.
- Finish each session by updating STATUS.md, committing, and pushing.
- Out of scope: the LiPoNarc repository (Decision D-001).

## Standing instructions from Tony (2026-10-04)
- At the start of every session, and whenever a session touches LongviewHub, restate the outstanding items Tony owes (the "Waiting on Tony" list in STATUS.md). Do not assume he remembers them.
- If Tony drifts to other projects while LongviewHub recovery work is open, bring him back to the LongviewHub overhaul and retool. Park the other idea in `14_Future_Projects` and say so in one line. This is the anti-distraction rule, applied to Tony at his own request.
