# Morning Run — instructions for the daily 01:07 routine

You are preparing Emiel's knee rehab plan for **today** (the day that has just started — the run
fires around 01:07 Europe/Amsterdam) and writing it to his Knee Day page. He will not chat with
you; he just opens the page in the morning. Do not message him, do
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

For every `log/<date>` doc that is new **or changed** since the last sync (compare `updatedAt`; Emiel
can back-fill or edit any of the last 7 days):

- Add or update its row in the `## Response Scores` table (create the table just above
  `## Log Entries` if it doesn't exist): `| date | score | session | note |`. Use "—" for missing values.
- `session: "done"` with no workout entry for that date → add a short workout entry: heading
  `### <Weekday> <date> — PT Session (from daily page)`, Workout = the exercises from that day's
  `days/<date>` plan ("done as planned per the daily page; item-by-item not confirmed"),
  Reaction = the response score(s) logged that day and the next 1–3 days, or **pending** if none yet.
- Update the reaction of any **pending** entry with the scores logged in the 1–3 days after it.
  State the scores and dates; don't interpret a score as more than he said. A score ≥4 is pain
  worth flagging: apply the Pattern Recognition rule (list every activity in the prior ~72h as an
  equally possible contributor) in the entry's note.
- `session: "skipped"` or `"none"` → no entry needed; it only affects scheduling.
- **`note`** (free text) is Emiel's own report for that date, **equivalent to him telling you in chat**.
  Parse it the way `CLAUDE.md` says to parse a chat report: a described workout or activity becomes
  or updates an entry; a reaction updates the relevant entry (only the entry it is about); pain gets
  the Pattern Recognition treatment. Quote or closely paraphrase; don't add what he didn't say. If a
  note mentions an upcoming event (e.g. padel on a date), add it to "Known upcoming events" in this file.
  Put a short version in a `note` column of the `## Response Scores` table too.

Commit, push and fast-forward `origin/main` (per `CLAUDE.md`). Skip the commit if nothing changed.

## 3. Decide today: train or recovery

### 3a. Pre-planned days and known events — check first

- If `days/<today>` already exists with `"locked": true`, it was planned together with Emiel in chat.
  **Don't overwrite it.** Still do step 2 (sync) and step 5 (Sunday proposal: `update` only the
  `proposal` field), then finish.
- **Known upcoming events** (added from chat; remove them once they're past):
  - **Mon 2026-10-26: padel, 60 min, very low-key with colleagues** (High Risk list). That day is
    locked. **Sun 10/25** (the day before): if it qualifies for training, it's **Session B** (hip & trunk,
    lowest knee load), not A; otherwise recovery. **Tue 10/27** (the day after): recovery only.
    The next session after that is ≥48h after the padel, so Wed 10/28 at the earliest. Rotation
    continues from there (A next if Sunday was B).
- **Warm-ups before padel or other sport:** ~5 min, no equipment, done on site (e.g. tibialis raise against a wall, leg swings, easy shuffles). Emiel found a 12-min banded warm-up too long and heavy (2026-10-24).
- General rule for any known high-risk event (padel, long ride, long hike): no quad-heavy session
  (A or C) the day before, recovery the day after, the next session ≥48h after the event.

### 3b. Normal days

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

**Training day:** the next session in the **A → B → C** rotation (see "Weekly Structure" in
`01_Rehab_Plan.MD`; find the last one done from the log and the `days/*` docs — the first one after
2026-10-24 is **A**). Put the session letter and focus in the headline. Build it exactly as `01_Rehab_Plan.MD` currently prescribes it (current
doses, the current overrides such as a temporary rebalance table, and rotation and alternation rules,
working out from the log which variant is next). Order: the plan's block structure. Every plan
item has `source: "plan"`. You may add a short warm-up or cooldown item of your own only as
`source: "idea"`. **Do not progress or regress doses yourself.** Dose changes go through the weekly
proposal (step 5).

**Recovery day:** 20–40 min total, from this menu (vary it day to day; tag plan items `plan`,
others `idea`):
- Stretching & mobility (always include 10–15 min): figure-4 glute stretch 2×90 s/side and half-kneeling hip flexor stretch 1 min/side
  (plan); 90/90 hip switches, couch stretch (left first), calf stretch on the wedge (idea). **Calf stretch: same dose both legs** (Emiel, 2026-10-24 — even though the right calf is tighter)
- Tibialis raise 1×25 (plan).
- Easy cardio, pick one: walk ≤40 min on soft or varied ground; easy bike ≤45 min at 85–95 RPM,
  flat, conversational, skipped if the quads are still heavy from the last session (idea). Swim
  or SUP when he has the option (idea).
- **No soft-tissue work** (scar mobilisation, self-massage, foam rolling) — Emiel doesn't want it
  on the menu (2026-10-24). Stretching is the part he values; make it the core of every recovery
  day. Nerve glides are also left out until he decides on the nerve question from the review.

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
