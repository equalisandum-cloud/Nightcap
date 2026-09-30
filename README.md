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

## Exercises before and after, and research consent

Each team starts with a warm-up exercise of about five minutes, before the method guide, and does a similar one after submitting its final argument. There are two parallel versions (A and B, set in `EXERCISES`). Each team gets them in random order, so any difference between before and after is not just a difference between the two versions. Each exercise has an open part (explanation, two predictions, a rival and an observation that tells them apart) and four statements to classify as abduction, deduction or induction. The classification is scored automatically. The open part needs coding by hand. The session clock starts after the warm-up. To turn both exercises off, set `exercises: false`.

The final argument form asks whether every member of the team agrees to research use. The answer is stored as `researchConsent` ("yes" or "no") in the submission, next to `mode` (AI or scripted) and the model. Every submission is still used in the plenary. Set `researchContact` to the person students contact to withdraw.

In the dashboard you can show only teams that agreed, and "Export exercises for blind coding" downloads two files for those teams only: a coding file with the written answers in random order under random codes, and a key file linking codes to team, before or after, version and mode. Give coders only the coding file.

Team-level consent is a practical compromise. For publication, check with RUC's data protection officer and research ethics guidance whether individual consent, collected separately, is required.

## Research settings

At the top of the settings in `index.html`:

- `cohort` and `caseVersion` are stored with every submission. Change `cohort` each time you run the course, and `caseVersion` whenever you change the case.
- `sections` is an optional list of classes or streams. If filled in, teams choose one at the start.
- `experiment` runs a randomised comparison between teams. With `active:true`, each team is assigned an arm at random, or by the link it opens: `.../nightcap/?arm=structured` or `?arm=free`. Handing out the two links to alternate teams gives balanced arms. The default arms compare the structured log (fields and sentence frames) with a plain log. Both keep the method guide and bridge cards, so the comparison isolates the structure of the log. Leave it off in the pilot year.

What the game records without asking students for anything extra: time spent on the method guide, when each file was opened, every interview question and answer (with whether the AI answered, was regenerated or fell back to the scripted line), hypothesis status changes over time, bridge-card answer times, when new files were released, and how long each exercise took. The team's downloaded file holds everything. The Google Sheet receives the core submission and the trace in two rows, shortened if needed to fit a cell.

The only additions students notice are the team size at the start and three agreement questions (1 to 5) after the follow-up exercise.

## Languages

The game runs in English or Danish. Teams choose on the start screen, and a link ending in `?lang=da` opens directly in Danish (`?retest&lang=da` for the Danish follow-up). `defaultLanguage` in the settings sets what opens without a choice. Every submission records its language.

All texts that students read are translated: the case files, the characters' scripted answers and statements, the method guide, the bridge cards, the exercises and the interface. The instructions the AI characters receive stay in English, because small models follow English instructions more reliably, and the model is told to answer in Danish. This keeps the AI's instructions identical across the two languages. Danish AI answers will still be weaker than English ones, so test Danish AI mode before relying on it.

The Danish exercises are translations of the English ones. Before comparing scores across languages, have a second person check the translation, ideally by translating the Danish items back into English, and compare item difficulty across languages in the pilot data.

For balanced randomisation in both languages, hand out links such as `?lang=da&arm=structured` and `?lang=da&arm=free` alternately in the Danish class, and the English equivalents in the English class. Use `sections` to record class and teacher.

## Laptop speed and AI mode

AI mode runs on each team's own laptop, so speed depends on its graphics. Apple M-series laptops and laptops with a separate graphics card are usually fine. Laptops with built-in Intel or AMD graphics are slower, and older ones can be too slow to play with.

The game adapts in four ways:

- On built-in graphics it preselects a small model (1B to 1.5B), which is several times faster than the 3B models.
- After loading, it runs a speed test and estimates the seconds per answer. If that exceeds `speed.ok` (25 seconds by default), the team is offered a smaller model, scripted characters, or to continue anyway.
- Answers appear word by word while they are generated, are limited to `maxAnswerTokens`, and use a short instruction text. The first slow compilation happens during the speed test, not on the first real question.
- Phones and graphics cards without 16-bit shader support get 32-bit model versions, and unreadable output is caught and replaced by scripted answers.

The speed test result, the graphics vendor and the average seconds per AI answer are stored with each submission and included in the research export, so hardware can be taken into account in the analysis.

Plan scripted mode as the baseline that works everywhere. If AI characters for every team matter, a classroom server with one proper graphics card is the reliable alternative.

## Individual follow-up later in the course

Open `.../nightcap/?retest` in a later session, for example as a warm-up exercise. Each student answers individually on their own device, with a third version of the exercise (C) and an individual research consent question. The team name is optional. It gives an individual, delayed measure next to the team measures from the game.

## Dashboard exports

- **Export exercises for blind coding**: the written exercise answers from consenting teams and individuals, in random order under random codes, plus a separate key.
- **Export research data**: a table with one row per consenting team (conditions, process measures, scores, ratings), and a full data file with logs, interviews and event traces.

## Extra columns in the Google Sheet (optional)

The whole submission lands in one cell, and it begins with the team, the type (final or post), the research answer and the mode, so these are visible there. To get them in separate columns, add four short-answer questions to the form (for example Team, Type, Research consent, Mode), find their entry IDs with a pre-filled link, and put them in `googleFormColumns`.

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
