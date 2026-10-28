# niroblabs

Claude Code / agent skills by **Saifur Rahman Nirob** ([@snirob786](https://github.com/snirob786)).

Each folder is a self-contained [agent skill](https://agentskills.io/specification) (a `SKILL.md` Claude loads on demand).

## Skills

| Skill | Use it when… |
|-------|--------------|
| **go-to-market** | You have a product or idea and need branding or marketing — naming, visual identity, positioning against a competitor, cheap growth tactics, or a full go-to-market plan. Runs as a senior CMO orchestrating 12 specialist roles through a gated pipeline. |
| **viral-idea-generator** | You want viral product ideas for a market (game or app/site), or want to research *why* something went viral and find trending content to borrow. |
| **viral-feature-advisor** | You have an existing product and want improvements plus new features that make it spread — viral loops, shareable artifacts, referral, network effects. |
| **phase-reorganizer** | A project's PRD/roadmap has drifted from reality and you want the phases re-bucketed: pull everything done into one pile, re-divide the rest by priority. |
| **ad-hunter** | You have a URL (a landing page, redirect, or "pay" link) and want to find the paid ads pointing at it and where they run — Google, Meta, TikTok, LinkedIn. Traces redirects, reads click-IDs and tracking pixels, then searches public ad-transparency libraries and pulls the creatives. |

## Install

Clone and copy (or symlink) the skills you want into your personal skills directory:

```bash
git clone https://github.com/snirob786/niroblabs.git
cp -R niroblabs/go-to-market ~/.claude/skills/
```

They then load automatically, or you can invoke one with `/go-to-market`.

## License

MIT — see [LICENSE](LICENSE).
