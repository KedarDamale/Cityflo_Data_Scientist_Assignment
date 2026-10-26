# Delivery guide: Data Scientist - Analytics & Tracking

## Assignment and current status

- Applied role: **Data Scientist - Analytics & Tracking**.
- Cityflo MCP `position_id`: **`data-engineer`**. Use this exact value for transcript upload and submission.
- Official bundle: https://careers.cityflo.com/takehomes/data-engineer/
- Timebox: **8-12 focused hours**.
- Goal: **For each route, as of 07:45 IST on 2026-06-17, is the bus late, by how much, and what should operations do?**

This folder contains the original downloaded assignment documents and data, preparation notes, empty `src/` and `outputs/` work areas, and an empty steering ledger. Implementation and final decisions still need to be produced. The application, candidate profile, resume, and agent configuration were already submitted; the take-home deliverable has not been submitted.

`BRIEF.md`, `HANDOFF.md`, and `DATA_GUIDE.md` are the downloaded source documents. This delivery guide explains the workflow and a suggested project layout; suggested filenames and folders do not add grading requirements. Download URLs and original SHA-256 hashes are in `sources.json`.

## Where to put your work

Work in this Desktop folder as its own project and, when ready, its own repository. Suggested public repository name: `cityflo-data-scientist-takehome`.

```text
Cityflo-Data-Scientist/
  BRIEF.md             # original assignment
  HANDOFF.md           # original ops request
  DATA_GUIDE.md        # original column reference
  data/                # original downloaded inputs
  src/                 # write your implementation here
  outputs/             # generated verdicts, ranking and safe evidence
  README.md            # add exact setup and run instructions
  DECISION.md          # add your final decision record
  decisions.jsonl      # starts empty; add real steering decisions
  delivery.md          # this guide
  sources.json         # original file download URLs and hashes
  local-transcripts/   # create locally for raw logs; ignored by Git
```

Choose a dependency file appropriate to your implementation, such as `pyproject.toml` or `requirements.txt`. The layout is a suggestion, not a framework requirement.

## What to do while solving

Start a dedicated coding-agent session in this folder so the solve and its raw transcript are easy to identify. Use your normal agent throughout; agent use is required. Record real user steering as it happens.

1. Read `HANDOFF.md`, `BRIEF.md`, and `DATA_GUIDE.md`. The fixed evaluation instant is `2026-06-17T07:45:00+05:30`. Monday and Tuesday are historical context; Wednesday is the live morning.
2. Build a runnable slice that ingests GPS pings, trips, bookings, routes, and stops, then accepts a route and an as-of timestamp. A script, SQL views, or a notebook is acceptable; choose the storage and implementation you can defend. An MCP server is not a required deliverable for this role.
3. Put implementation code in `src/`; optional SQL and notebooks can go in `sql/` and `notebooks/`. Put dependency declarations and exact reproduction commands in the project root. Explain the data model and joins in your run documentation.
4. Define what "late" means, how you handle device time versus ingestion time, and what information was actually available at the as-of moment. Check those choices against the underlying rows.
5. Produce one route verdict at the fixed timestamp using exactly these action categories: `PUSH_LATE <n> min`, `HOLD`, `CALL_DRIVER`, or `NO_VERDICT` with a one-line reason. In the brief, `HOLD` means running fine/do nothing. Do not silently reinterpret it as an instruction to hold a connection.
6. Give one ranked worry-order for Priya's follow-up, supported by the consequences and evidence for each call.
7. Evaluate the reconciliation directions in `HANDOFF.md` against the brief's question and the evidence. Record how you handle them and any conflicts in `DECISION.md`; the notes are not a substitute for checking the data.
8. Keep the solution able to accept another day's extract and timestamp for the live round. If you hit the timebox, stop and record remaining work in the decision record.

For each consequential call in `DECISION.md`, cover all four items:

1. **Rule:** a stranger can apply it to another extract and reproduce the call.
2. **Where your rule breaks:** cite a specific route, trip, or ping ID and explain why you keep the rule despite that case.
3. **Cost and who bears it:** quantify the operational impact from your numbers where possible.
4. **What would change your mind:** name the observation that would flip the call.

Write a committed decision that Priya can defend. The grading emphasis is judgment and its defense; parsing files and selecting a framework are only the starting point.

## Steering ledger and raw transcript

The supplied `decisions.jsonl` is empty intentionally. Each real entry must contain `decision`, `quote`, and `anchor`. The following is a schema illustration, not a real entry to submit:

```json
{"decision":"<the consequential decision>","quote":"<verbatim words the user actually sent>","anchor":"<matching turn or stable location in the raw transcript>"}
```

Copy quotes exactly and cross-check every anchor against the actual exported transcript. Agent proposals are not user overrides; if the user kept a proposal, record that honestly. There is no minimum count. Do not invent user messages or backfill fictional steering.

Keep the original raw solve-session transcript, including the actual messages and tool interactions. A hand-written summary, reconstructed conversation, or this delivery guide is not a transcript. Locate/export the genuine session file using your agent's session storage and verify it belongs to this assignment. If the solve spans sessions, preserve the original files and resolve the multi-session upload format with Cityflo rather than dropping part of the work. Keep raw logs out of the public Git repository; upload them through Cityflo's dedicated session-log URL.

## Deliverables to finish

- The runnable analysis/pipeline and its dependency file; include any SQL or notebook needed to reproduce it.
- `README.md`: setup, data model/joins, exact run command, as-of handling, and how to rerun on another morning.
- `outputs/verdicts_0745.csv` (suggested filename): one row per route with route, verdict, and note. Include the same table in the decision record.
- `outputs/worry_order.csv` (suggested filename), or an equivalent reproducible ranked worry-order in the decision record.
- `DECISION.md`: the verdict table, worry-order, lateness definition, data-quality/reconciliation decisions, and defended consequential calls. The brief requires a decision record; this filename is our convention for clarity.
- `decisions.jsonl`: genuine steering decisions with verbatim user quotes and transcript anchors.
- The complete raw solve-session transcript, uploaded separately through Cityflo MCP.

## After solving: reproduce and publish the code

1. Rerun the commands in your solution's `README.md` from a fresh environment. Check that they regenerate the documented verdicts and ranking, and that the evidence references and steering anchors are correct.
2. Review the public files, dependencies, and any example client configuration. Keep credentials, local virtual environments, raw session logs, and machine-specific configuration out of Git. The included `.gitignore` provides a starting point.
3. Publish a separate public GitHub repository or gist containing this role's code, dependency declarations, decision record, steering ledger, run instructions, and the permitted inputs/fixtures needed for reproduction. For any input retained locally, document its official source and how to obtain it. GitHub is the simplest repository option; Cityflo accepts a repo/gist deliverable URL.
4. Confirm that someone who is not signed in to your account can open the URL, read the code and decisions, and follow the run instructions. A local Desktop path is not a submission URL.

If you use GitHub CLI, run these commands **only after the solution is ready to publish**, from this folder. They create a new public repository; change the name if it is already taken:

```bash
git init -b main
git add .
git commit -m "Complete Cityflo take-home"
gh repo create cityflo-data-scientist-takehome --public --source . --remote origin --push
```

An existing repository may need a normal commit and push instead. Record the actual final URL for the next step.

## After publishing: upload the transcript and submit to Cityflo

These are **MCP calls for your agent**, not terminal commands. The exact submission order is:

1. Ask the agent to request a fresh session-log upload URL:

```json
{"tool":"get_session_log_upload_url","arguments":{"position_id":"data-engineer","filename":"data-engineer-session.jsonl"}}
```

2. Keep both returned values: `upload_url` and `session_log_key`. Upload the genuine raw transcript file with an HTTP PUT to that URL. For example, after setting `CITYFLO_UPLOAD_URL` to the returned URL and `CITYFLO_SESSION_FILE` to the real local file:

```bash
curl --fail --show-error --upload-file "$CITYFLO_SESSION_FILE" --write-out '\nHTTP %{http_code}\n' "$CITYFLO_UPLOAD_URL"
```

Confirm HTTP 200 before submitting. Request another URL if the short-lived URL expires. Never put the temporary upload URL in the public repository.

3. Ask the agent to submit this role's actual public URL and the key from the successful upload:

```json
{"tool":"submit_assignment","arguments":{"position_id":"data-engineer","deliverable_url":"<actual public repository or gist URL>","session_log_key":"<key from the successful transcript upload>","notes":"<brief run instructions, final commit, and material limitations>"}}
```

4. Read the tool response and confirm Cityflo accepted the submission. A published repository alone is not a completed Cityflo submission. Repeat this process separately for the other role using its own URL, role ID, transcript, and upload key.

You can ask your connected agent: "Submit my completed Data Scientist - Analytics & Tracking take-home from this repository, using its genuine raw solve-session transcript." Provide the final repository URL and the session identity/file when needed.

## Prepare for the live round

After submission, Cityflo schedules a 15-minute live round with a fresh fourth morning and one new constraint. Run your existing slice, defend the worry-order, identify the call you trusted least, and explain what would change it.
