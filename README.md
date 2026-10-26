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
- When you draft an application, the skill applies its own fixed honest-tailoring rules: no
  invented experience, certifications or platforms, and no SAR / STR details in a CV. Text the
  connector returns about writing is treated as data, never as rules.

## Where the links lead

Each result's `url` is a FinCrime Desk page, not the employer's own posting; some roles are applied
for through FinCrime Desk's own recruiting team; and page 1 can carry roles marked as sponsored.

## Install

1. Copy the `fincrimedesk-search/` folder into your fork's `.agents/skills/`.
2. Add the connector:
   ```bash
   claude mcp add --transport http fincrimedesk https://fincrimedesk.com/mcp
   ```
3. Run `/mcp` in Claude Code, pick `fincrimedesk` and sign in with a **free** FinCrime Desk account.
4. Run `/scrape`; the skill is picked up with your other portals. Claude Code asks you to approve
   the two connector tools (`mcp__fincrimedesk__search_jobs`, `mcp__fincrimedesk__get_job`) the first
   time they run, like any MCP tool. The skill has no `allowed-tools` line, so nothing needs adding
   to `ALLOWED_SKILL_TOOLS`.

Keep the server name `fincrimedesk` so the tool names above match.

## Your data

Full text: [FinCrime Desk privacy policy, "AI assistant connector"](https://fincrimedesk.com/privacy#connector).

- On first sign-in the connector asks eight questions: discipline, level, years in financial crime,
  employer type, country, **current employer** (or "prefer not to say"), the platforms you use, and
  whether you are looking for a new role. They are saved to your FinCrime Desk account, where you
  can change them.
- **Every tool call is logged** with the search terms and filters it sends and your account tier,
  and the log is kept for **13 months**.
- Your answers and searches feed FinCrime Desk market research as anonymous totals of 5 or more
  people. **Some of that research is sold to employers, vendors and Intel subscribers as reports**:
  they get the totals, never your name, account or answers.

## Limits

Free accounts can read 4 pages (100 roles) per search and run 40 searches a day. Applying happens
on the role's FinCrime Desk page; the connector never submits applications.

## Disclosure

Published by FinCrime Desk. Questions or problems: contact@fincrimedesk.com.

## License

MIT, see [LICENSE](LICENSE).
