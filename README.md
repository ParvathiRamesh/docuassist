# DocuAssist

An agentic Post Project Document (PPD) generator. DocuAssist takes a project
proposal, an epic, and user stories, then generates a draft PPD as a downloadable
Word (`.docx`) file.

It runs entirely in the browser — there is no backend and no build step. The whole
app is a single self-contained HTML file.

## How it works

DocuAssist uses two agent passes before you see a result:

- **Input Analyst Agent** — scores your input for completeness before generating.
- **Quality Judge Agent** — reviews the generated draft before presenting it.

The app calls the Anthropic API directly from the browser using a key that you
enter at runtime. The key is held in `sessionStorage` for the current browser
session only and is never written to disk or committed anywhere.

## Requirements

- A modern browser.
- An Anthropic API key (get one at https://console.anthropic.com → API Keys →
  Create key). Each user supplies their own key.

## Running locally

Because it is a single static file, you can open it directly:

1. Open `index.html` in your browser.
2. Paste your Anthropic API key on the setup screen.
3. Fill in the project proposal, epic, and user stories.
4. Generate, review, and download the `.docx`.

To serve it over a local web server instead of opening the file directly:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying to Vercel

The app is static, so Vercel needs no build configuration.

**Using the CLI:**

1. Place the file in an empty folder, named `index.html` so it loads at the root URL.
2. Install the CLI: `npm i -g vercel`
3. From inside the folder, run `vercel` and follow the prompts (accept the defaults).
4. Run `vercel --prod` to promote the deployment to production.

**Using GitHub:**

Push the folder to a repository, then in the Vercel dashboard choose
**Add New → Project**, import the repo, and accept the defaults. Every push
redeploys automatically. No build command is required.

The Anthropic API call is made from the visitor's browser, not from Vercel's
servers, so there is nothing extra to configure for it to work.

## A note on the API key

Because DocuAssist calls the Anthropic API directly from the browser, anyone who
opens a deployed instance enters their own key, and that key is exposed to their
own browser session. This is appropriate for internal testing, where each tester
uses their own key. It is not a pattern for a public production product, where the
key should be kept server-side behind a backend that proxies the API calls.

## Tech

- Plain HTML, CSS, and JavaScript — no framework, no build.
- [JSZip](https://stuk.github.io/jszip/) for assembling the `.docx` file in the
  browser. JSZip is dual-licensed under MIT or GPLv3.

## License

Add a license of your choice here.
