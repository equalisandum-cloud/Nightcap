# The Last Nightcap

A murder-mystery game for teaching abduction, deduction and induction in a research design course. Teams play in the browser. The characters are voiced by a small language model that runs on each team's laptop, so no server is needed.

Files:

- `index.html` is the game. The case, the characters and all settings are at the top of its script, under "CASE AND SETTINGS".
- `dashboard.html` is for you in the plenary. It holds the solution, so do not link to it from the game.

## Put it online with GitHub Pages

1. Create a new repository on GitHub, for example `nightcap`. It must be public for Pages on a free account.
2. Upload `index.html`, `dashboard.html` and this README (Add file, then Upload files, then Commit).
3. In the repository go to Settings, then Pages. Under "Build and deployment" choose "Deploy from a branch", branch `main`, folder `/ (root)`, and save.
4. After a minute or two the game is at `https://<your-username>.github.io/nightcap/` and the dashboard at `.../nightcap/dashboard.html`.

## Test it yourself

Open the game with `?test` at the end of the address, for example `https://<your-username>.github.io/nightcap/?test`. This shows test controls to unlock all files, trigger the mid-game event and reset the game.

1. Play once in scripted mode. This shows the whole teaching flow without the model: the unlock rules, the bridge cards, the log and the final argument.
2. Then play in AI mode in a recent Chrome or Edge. The first load downloads the model (roughly 1 to 2 GB). Try several models from the list and compare the characters.
3. Submit a final argument, download the file and load it into the dashboard.

What to look for with the model: does it keep to the facts, does it reveal things too early, and are the characters distinct enough to be worth interviewing? Any answer that mentions a clock time the character does not know, or that confesses, is rejected and regenerated once. After that the scripted line is used.

## Before class

- Change `eventCode` in the settings if you want a different code. Read it out after about 25 minutes. It releases the planted card, the cloakroom statement and a bridge card.
- Ask teams to open the game the day before and load the model once, so it is cached. Twenty-five laptops downloading at the same time on campus wifi will be slow.
- Teams of about four, one laptop per team. Scripted mode is a fallback for laptops without WebGPU.

## Collecting submissions (optional)

Without any setup, each team downloads a file when it submits and sends it to you, for example by uploading it to the course page. The dashboard reads several files at once.

To collect automatically with a Google Form:

1. Create a form with one question of the type "Paragraph", for example "Submission".
2. Use "Get pre-filled link" (in the form's menu), type anything in the answer and copy the link. It contains `entry.` followed by a number. That is the `entryId`.
3. The `action` is the form's address with `/viewform` replaced by `/formResponse`.
4. Put both in `googleForm` in the settings.
5. Link the form to a Google Sheet. In the Sheet choose File, then Share, then Publish to web, pick the response sheet and CSV, and paste that address into the dashboard.

Test the form with one submission before class. Only team names and their reasoning are collected, no student names.

## Known limits

- The source of the game is visible to anyone who looks. It contains the characters' cover stories, though not the solution, which is only in the dashboard. Tell students that reading the source is against the rules of the game.
- The web-llm library is loaded unpinned. Once the game works, pin the version you tested by changing `webllmUrl` to include `@` and the version number, so a later update cannot break it.
- Small models are uneven. If the characters are too weak, the same game can use a larger model on a classroom server with a few changes to the model connection.

## Changing the case

Everything lives in the settings block of `index.html`:

- `DOCS` holds the evidence files. `tier` controls when each is released: 0 from the start, 1 after a hypothesis and a linked prediction, 2 after a second hypothesis and a recorded test, and `"event"` after the code.
- `CHARACTERS` holds the four suspects. Each topic has keywords, a brief for the model, a scripted line, and optionally a `locked` version used until its requirement is met. A `testimony` is added to the team's notes when the topic comes up.
- `BRIDGES` holds the four reflection cards and what triggers them.

If you change facts, check the timeline across all files, and update the solution in `dashboard.html`.
