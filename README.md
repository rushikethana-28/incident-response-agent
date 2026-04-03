# Incident Response Agent

An AI agent that triages production incidents from logs, correlates errors across services,
matches runbooks, and drafts blameless postmortems. Built for the gitagent Hackathon.

## Quickstart
```bash
npm install -g @open-gitagent/gitagent
npm install gitclaw

npx @open-gitagent/gitagent validate
npx @open-gitagent/gitagent info
```

## Usage

### Triage an incident from logs
```js
import { createAgent } from 'gitclaw';

const agent = await createAgent('./');

const result = await agent.run(
  `We have an incident. Here are the logs from api-gateway and auth-service
   in the last 30 minutes: <paste logs>`
);
```

## Repo Structure
```
incident-response-agent/
├── agent.yaml
├── SOUL.md
├── RULES.md
├── skills/
│   ├── parse-logs/SKILL.md
│   ├── correlate-errors/SKILL.md
│   ├── query-runbooks/SKILL.md
│   ├── suggest-remediation/SKILL.md
│   └── draft-postmortem/SKILL.md
├── runbooks/
└── postmortems/
```
## Adding Runbooks

Drop `.md` files into `runbooks/` following this naming convention:
`<service-name>-<failure-pattern>.md`

The `query-runbooks` skill will automatically find and match them.

Example: `runbooks/auth-service-cascade-failure.md`

## Key Design Decisions

- **Blameless by default** — RULES.md prohibits naming individuals in postmortems
- **Human-in-the-loop for destructive actions** — all irreversible steps are flagged `[CONFIRM]`
- **Transparent reasoning** — the agent distinguishes confirmed findings from hypotheses
- **Git-native** — postmortems are committed to `postmortems/`, runbooks live in the repo