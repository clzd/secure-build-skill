# secure-build

secure-build is a Claude Code skill that loads automatically when Claude writes or reviews code that handles user input, fetches URLs, calls external services, stores credentials, or needs to survive failures. It gives Claude a 15-item checklist covering security (TLS, input validation, XSS, CSRF, CORS, SSRF, hashing, authentication, authorization, brute force) and reliability (timeouts, retry, circuit breaker, error handling, race conditions). Every item has a plain-language rule plus a BAD and GOOD Node.js example in the reference files.

Install by running this from your project's root folder:

```sh
mkdir -p .claude/skills && curl -sL https://github.com/clzd/secure-build-skill/archive/main.tar.gz | tar -xz -C .claude/skills --strip-components=2 secure-build-skill-main/skills/secure-build
```

## Hooks

`hooks/block-secrets.sh` stops Claude from writing to secret files (`.env`, `.env.*`, `*.pem`, `*.key`, `*.p12`, `id_rsa*`, and any path containing `/secrets/` or `credentials`), through its edit tools or through terminal commands like `echo KEY=x > .env`. Templates (`.env.example`, `.env.sample`, `.env.template`) are allowed. This is a hook and not a rule in the skill because it must happen every time with no judgment involved: a hook runs before every write and cannot be skipped, while a skill rule is advice Claude might not load or might talk itself out of.

Install from your project's root folder:

1. Download the script: `mkdir -p .claude/hooks && curl -sL https://github.com/clzd/secure-build-skill/archive/main.tar.gz | tar -xz -C .claude/hooks --strip-components=2 secure-build-skill-main/hooks/block-secrets.sh`
2. Paste the `"hooks"` block from [`hooks/settings.snippet.json`](hooks/settings.snippet.json) into `.claude/settings.json` (merge it if that file already has a `"hooks"` block).
