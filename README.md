# LongviewHub Retool

Working documentation for the LongviewHub.io infrastructure reboot: discovery, stabilization, rationalization, modernization, security, automation, documentation, validation.

Start at **`00_Project_Overview/STATUS.md`**.

| Folder | Holds |
|--------|-------|
| `00_Project_Overview` | STATUS (entry point), BASELINE (A–H model from 2026-10-03), backlog with active queue and blocker log |
| `01_Architecture` | Architecture notes and diagrams (empty until discovery completes) |
| `02_Server_Inventory` | Hosts, OS, access paths (empty until discovery completes) |
| `03_Service_Registry` | One entry per service; UNKNOWN fields are explicit |
| `04_Networking_DNS` | Domain and DNS registry |
| `05_Nextcloud` | Nextcloud baseline, upgrade path, app inventory (empty until baseline is pasted) |
| `06_Backups_Recovery` | Backup design and restore procedures |
| `07_Security` | Security review and findings |
| `08_Automation` | Scheduled tasks, scripts, alerts |
| `09_Monitoring` | What is watched and how |
| `10_Runbooks` | Step-by-step procedures; discovery checklist lives here |
| `11_Decision_Log` | Consequential decisions with alternatives and reversibility |
| `12_Change_Log` | Every infrastructure change with rollback |
| `13_Known_Issues` | Risk register and known issues |
| `14_Future_Projects` | Parked ideas that would otherwise derail the reboot |

Rules: no passwords, keys, or tokens in this repo, ever. Record where a credential lives, never its value. Unknowns are written as "UNKNOWN — discovery required", not guessed.
