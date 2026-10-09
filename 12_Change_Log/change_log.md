# Change Log
Every infrastructure change, with rollback. Documentation-only commits are not listed here; git history covers those.

| ID | Date | System | What changed | Why | Commands/actions | Previous state | New state | Restart? | Verification | Rollback |
|----|------|--------|--------------|-----|------------------|----------------|-----------|----------|--------------|----------|
| C-001 | 2026-10-09 | Hosting account hfppyjna, web root | Moved the abandoned Nextcloud 35 install and its early data dir out of the web root into ~/retired/ | D-005 step 1: end public exposure of an unmaintained install that pointed at the live data directory; no data touched | `mkdir -p ~/retired && mv ~/public_html/cloud_v35_old ~/retired/ && mv ~/clouddata_v35 ~/retired/` (run by Tony in cPanel Terminal) | public_html/cloud_v35_old (992 MB) and ~/clouddata_v35 (111 MB) present; cloud_v35_old reachable at longviewhub.io/cloud_v35_old/ | both under ~/retired/; URL now 404 | No | Tony reported "done". Follow-up check: `ls ~/retired` and a browser hit on longviewhub.io/cloud_v35_old/ should 404 | `mv ~/retired/cloud_v35_old ~/public_html/ && mv ~/retired/clouddata_v35 ~/` |
