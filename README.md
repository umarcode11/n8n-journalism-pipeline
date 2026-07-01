\# n8n Journalism Intelligence Pipeline



An automated intelligence-gathering system built in \[n8n](https://n8n.io) (a visual workflow automation tool) for investigative journalism. It monitors incoming tips and public YouTube commentary, scores them for editorial relevance using AI, and routes high-priority items to a daily briefing.



Built by Umar Cheema, an investigative journalist covering civil-military relations, judiciary accountability, NAB, PTI/PMLN dynamics, AJK governance, press freedom, and corruption in Pakistan.



\## What it does



\*\*1. Gmail Scoring Pipeline\*\* (`Journalism Intelligence Pipeline.json`)

Monitors a Gmail inbox hourly for incoming tips and messages. Each email is scored 0–100 by an AI model (Claude Haiku via OpenRouter) against a defined set of beat criteria. High-scoring items (50+) are logged to a Google Doc and Google Sheet, and trigger a Google Calendar follow-up reminder. Lower-scoring items are logged to Sheets only, for reference.



\*\*2. YouTube Monitor\*\* (`YouTube Monitor.json`)

Tracks seven Pakistani political/current-affairs YouTube channels via RSS feeds. When a new video is published, it fetches the transcript (via Supadata API), and an AI model extracts key takeaways, the main argument, notable claims, and whether the story warrants follow-up — posted directly to Slack.



\*\*3. Daily Slack Digest\*\* (`Daily Slack Digest.json`)

Runs once daily at noon UTC (5pm PKT). Reads that day's high-scoring items from the Gmail pipeline's Google Sheet and posts a formatted summary to a dedicated Slack channel, giving a single daily briefing of anything worth acting on.



\## Architecture 



Gmail inbox ──┐

├──> AI Scoring (Claude Haiku/OpenRouter) ──> Google Sheets / Docs / Calendar

YouTube RSS ──┘                                                      │

▼

Daily Slack Digest (noon UTC) 



\## Tech stack

\- \*\*n8n\*\* — workflow orchestration (self-hosted, localhost)

\- \*\*OpenRouter / Claude Haiku 4.5\*\* — AI scoring and content analysis

\- \*\*Supadata API\*\* — YouTube transcript extraction

\- \*\*Google Workspace\*\* — Sheets, Docs, Calendar (via OAuth)

\- \*\*Slack API\*\* — daily digest delivery



\## Setup

1\. Import each workflow JSON into your own n8n instance

2\. Replace placeholder credentials (`YOUR\_OPENROUTER\_API\_KEY`, `YOUR\_SUPADATA\_API\_KEY`) with your own API keys

3\. Connect Google (Gmail, Sheets, Docs, Calendar) and Slack credentials via n8n's credential manager

4\. Activate all three workflows



\## Assignment mapping



This project was built as a replacement for the course's standard HR-recruitment-pipeline assignment (webhook → CV screening → routing → notifications → logging). The same 12-stage architecture is applied here to a real editorial use case instead of a synthetic HR scenario:



| Assignment stage | This implementation |

|---|---|

| Webhook trigger | Gmail poll trigger (hourly) + YouTube RSS triggers (7 channels) |

| Data extraction \& validation | Set/Code nodes parsing email and video metadata |

| Screening criteria loader | Beat criteria (civil-military, judiciary, NAB, PTI/PMLN, AJK, press freedom, corruption) built into the AI Agent prompt |

| AI screening agent | OpenRouter/Claude Haiku, 0–100 relevance scoring |

| Scoring \& routing | Conditional routing: 50+ → Sheets + Docs + Calendar; below 50 → Sheets only |

| Calendar scheduling | Google Calendar follow-up event for high-priority tips |

| Sheets logging | Full audit trail in Google Sheets |

| Daily digest | Slack digest, noon UTC daily |

| Content analysis agent | Separate AI Agent for YouTube transcript analysis (takeaways, claims, follow-up recommendation) |



Two elements from the original brief were substituted rather than replicated directly: a Notion database (replaced by Google Docs/Sheets, which better fit the existing toolchain) and an outbound email agent (replaced by Slack notifications, the actual channel used for daily editorial review).



\## Note on credentials

API keys in these workflow files are placeholders. Real credentials are managed locally via n8n's credential store and are never committed to this repository. 

