# secure-build

secure-build is a Claude Code skill that loads automatically when Claude writes or reviews code that handles user input, fetches URLs, calls external services, stores credentials, or needs to survive failures. It gives Claude a 15-item checklist covering security (TLS, input validation, XSS, CSRF, CORS, SSRF, hashing, authentication, authorization, brute force) and reliability (timeouts, retry, circuit breaker, error handling, race conditions). Every item has a plain-language rule plus a BAD and GOOD Node.js example in the reference files.

Install by running this from your project's root folder (replace the path with where you cloned this repo):

```sh
mkdir -p .claude/skills && cp -R /path/to/secure-build-skill/skills/secure-build .claude/skills/
```

## Hooks

`hooks/block-secrets.sh` stops Claude from writing to secret files (`.env`, `.env.*`, `*.pem`, `*.key`, `*.p12`, `id_rsa*`, and any path containing `/secrets/` or `credentials`), through its edit tools or through terminal commands like `echo KEY=x > .env`. Templates (`.env.example`, `.env.sample`, `.env.template`) are allowed. This is a hook and not a rule in the skill because it must happen every time with no judgment involved: a hook runs before every write and cannot be skipped, while a skill rule is advice Claude might not load or might talk itself out of.

Install from your project's root folder:

1. Copy the script: `mkdir -p .claude/hooks && cp /path/to/secure-build-skill/hooks/block-secrets.sh .claude/hooks/`
2. Paste the `"hooks"` block from `hooks/settings.snippet.json` into `.claude/settings.json` (merge it if that file already has a `"hooks"` block).
