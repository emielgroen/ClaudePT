# Morning Run — instructions for the daily 06:12 routine

You are preparing Emiel's knee rehab plan for **today** and writing it to his Knee Day page before
06:30 Europe/Amsterdam. He will not chat with you; he just opens the page. Do not message him, do
not send notifications, do not ask questions. Follow `CLAUDE.md` (pattern recognition, diagnostic
framework, never inventing reactions) and these steps.

Page: `https://claude.ai/artifact/Up8CmDYn1xURDfx7N5g1EJ` — use the `ArtifactData` tool (load
it with ToolSearch) on that URL. Do not republish the page itself.

## 1. Get current

1. Repo: `emielgroen/ClaudePT`. If it isn't already checked out, clone it. `git fetch origin main`
   and work on an up-to-date `main`.
2. Today's date in **Europe/Amsterdam** (`TZ=Europe/Amsterdam date +%F`, weekday too).
3. Read `00_Master_Context.MD`, `01_Rehab_Plan.MD` and `02_Symptom_Log.MD`.
4. Read the page's data: `list` on `log` and on `days` (last ~30 docs is enough).

## 2. Sync the page's data into the log

For every `log/<date>` doc not yet in `02_Symptom_Log.MD`:

- Add or update its row in the `## Response Scores` table (create the table just above
  `## Log Entries` if it doesn't exist): `| date | score | session |`. Use "—" for missing values.
- `session: "done"` with no workout entry for that date → add a short workout entry: heading
  `### <Weekday> <date> — PT Session (from daily page)`, Workout = the exercises from that day's
  `days/<date>` plan ("done as planned per the daily page; item-by-item not confirmed"),
  Reaction = the response score(s) logged that day and the next 1–3 days, or **pending** if none yet.
- Update the reaction of any **pending** entry with the scores logged in the 1–3 days after it.
  State the scores and dates; don't interpret a score as more than he said. A score ≥4 is pain
  worth flagging: apply the Pattern Recognition rule (list every activity in the prior ~72h as an
  equally possible contributor) in the entry's note.
- `session: "skipped"` → no entry needed; it only affects scheduling.

Commit, push and fast-forward `origin/main` (per `CLAUDE.md`). Skip the commit if nothing changed.

## 3. Decide today: train or recovery

Use **facts only**: the schedule, completed sessions, logged activities, and the trend of scores.
**Never change today's plan because of a single score or how he might feel today.** He adjusts
on the day himself.

- **Target:** 3 PT sessions per Mon–Sun week, at least ~48h apart.
- A session counts as done if `log/<date>.session == "done"` or the symptom log has a PT session
  that day. Otherwise it didn't happen.
- **Train today** if the last done session was ≥2 days ago (e.g. Fri → Sun is fine) AND the week
  still needs sessions AND yesterday was not a high-risk or heavily loaded activity in the log
  (padel, long ride >60 min, hike >8 km, festival or long city day).
- If the week can no longer fit 3 sessions with 48h spacing, schedule what fits. Don't stack
  sessions on back-to-back days to catch up.
- Otherwise it's a **recovery** day. Use **rest** only for the day after a high-risk activity, or
  when the log says he's ill.

## 4. Build the day

**Training day:** the session exactly as `01_Rehab_Plan.MD` currently prescribes it (current
doses, the current overrides such as a temporary rebalance table, and rotation and alternation rules,
working out from the log which variant is next). Order: the plan's block structure. Every plan
item has `source: "plan"`. You may add a short warm-up or cooldown item of your own only as
`source: "idea"`. **Do not progress or regress doses yourself.** Dose changes go through the weekly
proposal (step 5).

**Recovery day:** 20–40 min total, from this menu (vary it day to day; tag plan items `plan`,
others `idea`):
- Mobility: figure-4 glute stretch 2×90 s/side and half-kneeling hip flexor stretch 1 min/side
  (plan); 90/90 hip switches, couch stretch (left first), calf stretch on the wedge (idea).
- Tibialis raise 1×25 (plan).
- Easy cardio, pick one: walk ≤40 min on soft or varied ground; easy bike ≤45 min at 85–95 RPM,
  flat, conversational, skipped if the quads are still heavy from the last session (idea). Swim
  or SUP when he has the option (idea).
- Soft tissue: left TTT scar mobilisation 2–3 min, then light desensitisation strokes with soft
  fabric; outer quad and glute self-massage, moderate pressure, never grinding the ITB (idea).
- Nerve glides (slump slider) ×10/side, gentle and never into tingling (idea).

**Rest day:** habits only, plus optional gentle mobility.

**Every day:** 2–4 `habits` from the plan's Daily Life & Postural Adjustments (soft knees, even
weight on both legs, footwear, sitting, sleep pillow). Rotate them; keep them short.

## 5. Weekly proposal (Sundays only)

On Sunday, review the past 7–14 days of scores, sessions and log entries against the progression
rule in `01_Rehab_Plan.MD`. Write 1–4 concrete proposed changes (e.g. "Clamshells 3×13 → 3×14:
all scores ≤3 this week") into the `proposal` array of Sunday's doc. Copy them onto Monday's doc
as well. **Never edit `01_Rehab_Plan.MD` yourself.** Emiel approves in chat.
If there's too little data (fewer than 3 scores in the week), the proposal says so and proposes
holding.

## 6. Write the page doc

`set` `days/<today>` (pass `if_version` if it already exists), shape:

```json
{
  "date": "YYYY-MM-DD",
  "type": "train | recovery | rest",
  "headline": "Short title, e.g. 'Session · hips first' or 'Easy recovery day'",
  "why": "1–2 plain sentences: why today is this type (facts: last session, spacing, week count).",
  "meta": ["~50 min", "Session 2 of 3 this week", "Next session: Wed 28 Oct"],
  "blocks": [
    {"title": "Warm-up", "time": "~8 min", "note": "optional",
     "items": [{"name": "Tibialis Raise", "dose": "2×25", "cue": "one short cue", "source": "plan"}]}
  ],
  "habits": ["..."],
  "proposal": ["only on Sun/Mon, otherwise omit"],
  "remind": true,
  "generatedAt": "ISO timestamp"
}
```

- `remind: true` only when no `log` doc has a score in the last 7 days (and at least one score was
  ever logged, or 7+ days have passed since 2026-10-24). Otherwise omit it.
- Language: English, short sentences, practical cues, no walls of text.
- Keep docs lean. Delete `days/*` docs older than 60 days.

## 7. Finish

Commit and merge any repo changes to `main`. End the session with a two-line summary (today's
type, anything flagged). Don't contact Emiel.
