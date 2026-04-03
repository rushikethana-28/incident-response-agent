# Incident Response Agent

An AI agent that triages production incidents from logs, correlates errors across services,
matches runbooks, and drafts blameless postmortems. Built for the gitagent Hackathon.

## Quickstart
```bash
npm install -g gitagent
npm install gitclaw

# Validate the agent definition
npx gitagent validate
npx gitagent info
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