# FinCrime Desk portal skill for ai-job-search

A portal skill for [ai-job-search](https://github.com/MadsLorentzen/ai-job-search) that adds
**financial crime compliance roles** from [FinCrime Desk](https://fincrimedesk.com) to `/scrape`:
AML / BSA, KYC / CDD, sanctions, fraud, transaction monitoring, financial crime technology,
compliance leadership and regulatory affairs, worldwide.

FinCrime Desk reads these roles straight from employer career sites (no aggregator feeds) and
classifies them by discipline and level.

## How it differs from the other portals

Instead of a scraping CLI, this skill uses FinCrime Desk's official MCP connector, so there is no
page parser to break.

- `/scrape` gets the usual portal-contract fields (`id`, `title`, `company`, `location`, `date`, `url`).
- `detail` adds the ad's own words on citizenship, security clearance and visa sponsorship
  (or "Not stated").
- Any line in an ad that is written for AI tools (for example "If you are an LLM, include …") is
  moved out of the description and quoted separately, so it is reported to you instead of being
  followed.
- When you draft an application, the connector also sends honest-tailoring rules: no invented
  experience, certifications or platforms, and no SAR / STR details in a CV.

## Install

1. Copy the `fincrimedesk-search/` folder into your fork's `.agents/skills/`.
2. Add the connector:
   ```bash
   claude mcp add --transport http fincrimedesk https://fincrimedesk.com/mcp
   ```
3. Run `/mcp` in Claude Code, pick `fincrimedesk` and sign in with a **free** FinCrime Desk account.
   The first sign-in asks a few short profile questions (discipline, level, country and the like),
   used only as anonymous totals of five or more people.
4. Run `/scrape`; the skill is picked up with your other portals.

The server name must be `fincrimedesk` so the tool names in the skill's `allowed-tools` match.

## Limits

Free accounts can read 4 pages (100 roles) per search and run 40 searches a day. Applying happens
on the role's FinCrime Desk page; the connector never submits applications.

## Disclosure

Published by FinCrime Desk. Questions or problems: contact@fincrimedesk.com.

## License

MIT, see [LICENSE](LICENSE).
