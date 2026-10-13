# Facts & Education YouTube Automation

Generate a draft YouTube Short and a long-form video with AI-written narration, text slides, captions, and upload metadata. GitHub Actions runs this workflow on Tuesday, Thursday, and Saturday. **It does not upload videos to YouTube**: download the artifacts, review/edit them, then upload manually.

## What it creates
- `shorts.mp4` — vertical 1080×1920 draft
- `longform.mp4` — horizontal 1920×1080 draft
- `script.md` — narration and fact-check checklist
- `metadata.json` — suggested titles, descriptions, tags
- `thumbnail.png` — simple draft thumbnail
- `shorts.srt` / `longform.srt` — approximate editable captions (check timing)

Caption timing is approximate and needs review. The visual style is intentionally a clean text-led explainer. Replace the generated slides with licensed footage or your own visuals during editing.

## 1. Create the repository
1. Create a GitHub repository, e.g. `facts-youtube-automation`.
2. Add these files and folders from this project.
3. In **Settings → Secrets and variables → Actions → New repository secret**, add:
   - `OPENAI_API_KEY` — an API key from your OpenAI API account.
4. In **Settings → Actions → General**, ensure Actions are enabled.
5. Commit the files to the default branch.

No YouTube API credentials are required for this manual-upload workflow.

## 2. Run locally (optional)
Requires Python 3.11+ and FFmpeg installed and available on PATH.

```bash
python -m venv .venv
# macOS/Linux:
source .venv/bin/activate
# Windows PowerShell:
# .venv\Scripts\Activate.ps1

pip install -r requirements.txt
export OPENAI_API_KEY="your-key"   # PowerShell: $env:OPENAI_API_KEY="your-key"
python -m src.generate --format both
```

You can also use `--format shorts` or `--format longform`.

## 3. Run on GitHub
- Open **Actions → Generate YouTube Drafts → Run workflow** to trigger it manually.
- Scheduled runs are Tuesday, Thursday, Saturday at 15:00 UTC.
- GitHub-hosted runners use UTC for cron schedules. GitHub may delay scheduled jobs during high load; scheduled workflows run from the default branch.
- After completion, open the run and download the `youtube-drafts` artifact. It contains MP4s and metadata.

## 4. Edit and upload
1. Watch the full video and check every factual claim against reliable sources.
2. Edit in your preferred editor (e.g. DaVinci Resolve, CapCut, Premiere).
3. Replace placeholder text slides with licensed visuals where appropriate; verify music and image licenses.
4. Use `metadata.json` as a starting point for title, description, and tags.
5. Upload the edited file manually in YouTube Studio.

## YouTube Data API integration
This project deliberately does not call the YouTube Data API because your chosen workflow is manual upload. `metadata.json` prepares the title, description and tags for YouTube Studio without needing OAuth credentials. If you later add automated uploads, use the official YouTube Data API `videos.insert` method with OAuth authorization and explicitly set the intended privacy status. Do not place OAuth tokens or API secrets in source control. New/unverified API projects can have upload restrictions; check current YouTube API requirements before enabling upload automation.

## Cost, safety, and quality notes
- OpenAI API usage is billed separately from a ChatGPT subscription.
- Generated facts can be wrong or oversimplified. Verify claims before publishing.
- Do not use copyrighted footage/music unless you have permission or a suitable license.
- Review platform policies on repetitive, mass-produced, reused, and synthetic content. AI assistance alone does not guarantee monetization eligibility.
- Never commit API keys. If a key is exposed, revoke it and create a new one.
