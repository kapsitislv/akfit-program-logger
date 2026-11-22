# Program data model · `akfit-program/2`

One JSON document per program. The same document drives the e-book generator and the Program Logger. Every user-facing string is bilingual: `{ "lv": "…", "en": "…" }`.

```jsonc
{
  "schema": "akfit-program/2",
  "id": "demo",
  "version": "0.1.0",
  "name": { "lv": "Demo programma", "en": "Demo program" },
  "weeks": 2,
  "daysPerWeek": 2,
  "intensity": "rir",            // "rir" (reps in reserve) or "pct" (% of 1RM; then "oneRM" lists the lifts to ask for)
  "chartRole": "main",           // which role's lifts appear in the progress charts
  "mapRoles": ["main", "support"],

  "blocks": [                    // phases of the program
    { "id": "b1", "name": {…}, "weeks": [1, 2], "goal": {…} }
  ],

  "weekNotes": [                 // one per week: what to focus on and the progression rule
    { "week": 1, "block": "b1", "focus": {…}, "rule": {…} }
  ],

  "roles": {                     // prescription by role, changing week by week
    "priority": {
      "name": {…},
      "rx": [
        { "weeks": [1], "sets": 2, "reps": "10", "rir": "3", "tempo": "2:0:1:0" },
        { "weeks": [2], "sets": 3, "reps": "10", "rir": "2", "tempo": "2:0:1:0" }
      ]
    }
  },

  "sessions": [
    {
      "id": "A",
      "name": {…},
      "duration": "~60 min",
      "exercises": [
        {
          "id": "a1",
          "role": "priority",      // takes its sets/reps/intensity from roles.priority for the current week
          "unit": "reps",
          "rest": {…},
          "cue": {…},              // technique cue
          "easier": {…},           // regression if the exercise is too hard
          "variants": [            // the exercise can change between blocks
            { "weeks": [1, 2], "name": "Squat variation" }
          ]
        }
      ]
    }
  ],

  "warmup": [ { "id": "w1", "name": {…}, "dose": {…}, "cue": {…} } ],

  "modules": [ … ]               // optional: add-on blocks the client chooses (focus muscle group, sport)
}
```

Why it is shaped this way:

- **Roles, not per-exercise numbers.** Sets, reps and intensity are set per role and week, so changing the progression for all "main" lifts is one edit, not one per exercise.
- **Variants by week.** An exercise slot keeps its place in the session while the movement progresses (for example a goblet squat in block 1, a barbell squat in block 2).
- **Text next to data.** Cues, rules and goals live with the numbers they explain, so the e-book and the logger always say the same thing.
