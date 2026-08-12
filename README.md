# AI Slop Recognition Assessment

A self-contained, single-file quiz app for training people to spot "AI slop" — formulaic, vague, inflated, or structurally artificial writing patterns in model-generated text.

No build step, no backend, no dependencies. Open the HTML file in a browser and it works.

## Features

- **20 questions** across 3 formats: yes/no slop detection, module identification, and select-all-that-apply
- **4 failure-mode modules**: formulaic language, vague substance, wordy/jargon, unnecessary framing
- Live timer, progress bar, and a question navigator for jumping between questions
- Auto-scored results with a question-by-question breakdown and explanation for every answer
- **80% passing threshold** (configurable)
- Submission history stored in the browser (`localStorage`), with CSV/JSON export
- Hidden admin dashboard for reviewing past attempts
- Optional **Google Sheets integration** for centralized result tracking

## Getting started

1. Download `slop-quiz.html` from this repo
2. Open it directly in any modern browser — or host it as a static file (GitHub Pages, Netlify, S3, etc.)

There's nothing to install or configure.

## Usage

- Click **Start Assessment** to begin
- Answer each question, then use **Next** / **Previous** or the question navigator to move around
- Submit at the end to see your score and a full explanation of each answer
- Retake as many times as you like — each attempt is saved locally

### Admin view

Click the bottom-right corner of the page 3 times to open the submissions dashboard, where you can review, export, or clear past attempts. This data is stored per-browser — see the tracking section below if you need one shared view across everyone who takes the quiz.

---

## Tracking who passed (Google Sheet setup)

By default, results only save in the browser of the person taking the quiz — there's no shared view. To collect every result into one Google Sheet you control, do a one-time setup:

1. **Create a Google Sheet** (any name, e.g. "GTMA Slop Assessment Results")
2. In the Sheet, go to **Extensions → Apps Script**
3. Delete any starter code in the editor, then paste in the contents of `apps-script.gs` (included in this repo)
4. Click **Deploy → New deployment**
5. Click the gear icon next to "Select type" and choose **Web app**
6. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
7. Click **Deploy**, then **Authorize access** and approve the permissions (this is your own script, so it's safe to allow)
8. Copy the **Web app URL** it gives you (ends in `/exec`)
9. Open `slop-quiz.html` and find this line near the top of the `<script>` section:
   ```js
   const SHEET_WEBHOOK_URL = "";
   ```
   Paste your Web app URL between the quotes:
   ```js
   const SHEET_WEBHOOK_URL = "https://script.google.com/macros/s/XXXXXXXX/exec";
   ```
10. Save and re-upload `slop-quiz.html` wherever you're hosting it

From then on, every person who finishes the quiz has their name, email, score, and pass/fail status appended as a new row in your Sheet — automatically, in real time. No login required on their end.

**Notes:**
- The quiz asks for the taker's name and email before they can start when a webhook URL is configured
- If the Sheet can't be reached (e.g. no internet), the result still saves in their browser and they'll see a message asking them to use the **Export** button on the admin dashboard as a backup
- You can revoke or redeploy the Web app at any time from the Apps Script editor
- This setup is optional — if you leave `SHEET_WEBHOOK_URL` blank, the quiz works exactly as before, saving only to the local browser

---

## Customizing

Everything lives in one file, `slop-quiz.html`:

- **Questions** — edit the `questions` array in the `<script>` section. Each entry needs `id`, `type` (`single` or `multi`), `module`, `text`, `options`, `correct`, `explanation`, and `pattern`
- **Passing score** — change the `PASSING_SCORE` constant
- **Colors/branding** — all styling is driven by CSS custom properties at the top of the `<style>` block (`--paper`, `--ink`, `--accent`, `--correct`, `--incorrect`, etc.)

## File structure

```
ai-slop-assessment/
├── slop-quiz.html       # The main quiz app (single file, self-contained)
├── apps-script.gs       # Google Apps Script for Sheet-based result tracking
└── README.md            # This file
```

## Notes

- Submission data is stored in `localStorage` and is local to each browser/device — it isn't synced or backed up anywhere unless you set up the Google Sheets webhook
- No analytics, tracking, or external requests are made by the page when the webhook URL is left blank

## License

Add your preferred license here.
