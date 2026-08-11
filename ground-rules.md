# Garden Brawl — Ground Rules

Read this first, every session, before touching `index.html`. It's the short version;
full incident writeups and detailed history live in `claude/garden-brawl-deployment.md`
and `claude/conventions.md`.

## 1. Deploy safety — the rule that matters most

**Fetch before you edit, not just before you push.** This repo gets edited by more
than one session/actor over time. GitHub's "Upload files" flow is a full-file
replace, not a merge — if your local copy is stale, uploading it silently reverts
everyone else's work with zero warning, no conflict marker. This has already
happened twice (2026-08-10, twice in one day).

- At the start of any session that will touch `index.html`: `git fetch origin main`
  and make sure your local file actually matches it before making edits.
- Right before every upload: `git fetch origin main` again and diff against what
  you're about to upload. If there's a difference you didn't make, stop and
  reconcile first — don't push through it.
- After every push: `git fetch` + confirm the diff is empty + `git reset --hard
  origin/main` to resync local history to the new commit (web-upload pushes create
  a new hash even for identical content — this is expected, not an error).
- Then verify the deploy actually happened: check the GitHub Actions "pages build
  and deployment" run completed, THEN load the live URL and confirm the specific
  change with a screenshot or a JS check in the console — not just "the commit
  succeeded."

## 2. Push mechanics

- This sandbox has no push credentials for SmolGh0st/garden-brawl — `git push`
  will always fail with 403. That's expected. The real deploy path is GitHub's
  web "Upload files" page via Claude in Chrome.
- Multiple Chrome browsers may be connected to this account, and not all of them
  are signed into GitHub with push access. Always confirm which browser with G
  (or use switch_browser) rather than assuming the first one in the list.
- The commit-message textbox on GitHub's upload page occasionally swallows typed
  text silently (field stays empty, commit goes through with a placeholder
  message). Screenshot after typing to confirm the text actually landed, every
  time, before clicking Commit.

## 3. Card design

- If a new card's rarity isn't stated, ask — don't guess or silently default.
- New cards do NOT automatically join `CORE_SET` or `STARTER_DECKS` — that's a
  deliberate opt-in decision each time, not automatic.
- Internal code identifiers ("plant", `PLANT_POOL`, DOM ids like `plantBar`) were
  deliberately kept after the Otherworld rebrand — only user-facing text/emoji
  changed. Don't be thrown off by "plant" still being in the code.

## 4. Testing before every push

- Playwright needs `chromium.launch({ executablePath: '/opt/pw-browsers/chromium'
  })` — no separate browser install required or wanted.
- Re-run existing test suites where they exist; zero regressions is the bar.
- Actually screenshot the visual result before calling something done. A class
  being present in the DOM doesn't guarantee it's visually rendering correctly —
  CSS specificity losses to higher-specificity context rules are a recurring,
  easy-to-miss bug here.

## 5. Communication

- Plain English, keep it honest — don't just agree if something looks like a
  mistake (flag it and ask, the way the "6 copies vs. mana cost" mixup got caught
  instead of silently "fixed").
- One dated, topic-named summary per session goes in `conversations/<date>-<topic>/`
  per the project's standing instructions.

## Where the rest of the detail lives

- `claude/garden-brawl-deployment.md` — full deploy history, incident writeups, changelog
- `claude/conventions.md` — standing card-design rules
- `claude/otherworld-faction-proposal.md` — faction design notes
