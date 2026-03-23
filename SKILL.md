---
name: youtube-summary
version: 1.0.0
description: |
  Fetch a YouTube video transcript and provide a structured summary.
  Usage: /youtube-summary <youtube-url> [--lang <language>]
allowed-tools:
  - Bash
  - Write
  - AskUserQuestion
---

# YouTube Summary Skill

You are tasked with fetching a YouTube video transcript and producing a structured summary.

## Steps

1. **Parse arguments**: Read `$ARGUMENTS`. Extract the YouTube URL and an optional language flag (`--lang <language>`). If no URL is provided, use the AskUserQuestion tool to ask the user for it. If no `--lang` flag is provided, default to **English**.

2. **Fetch the transcript**: Run the following command via Bash:
   ```bash
   uv run --no-project --with youtube-transcript-api python ~/.claude/skills/youtube-summary/fetch_transcript.py "$URL"
   ```
   Replace `$URL` with the actual YouTube URL.

3. **Handle errors**: If the JSON output contains an `"error"` key, report the error to the user in a friendly way and stop.

4. **Summarize**: Using the transcript text, produce a structured summary **in the chosen language** with this format:

   ### 🎬 Video Summary

   **Overview**
   A brief introductory paragraph summarizing the video's topic and main message.

   **Key Points**
   - Bullet points covering the main concepts discussed in the video
   - Each point should be concise but informative

   **Notable Quotes**
   > Notable quotes or significant phrases from the video (if any stand out)

   Translate section headings to match the chosen language (e.g., "Panoramica", "Punti chiave", "Citazioni notevoli" for Italian). If the transcript is in a different language than the chosen one, still produce the summary in the chosen language.

5. **Offer to save**: After presenting the summary, ask the user if they want to save it as a Markdown file. If yes, write it using the Write tool to a reasonable filename based on the video ID (e.g., `youtube_summary_<video_id>.md`).
