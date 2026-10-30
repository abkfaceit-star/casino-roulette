# casino-roulette

Проект с подключённым MCP-сервером [Purple Flea Casino](https://github.com/purple-flea/casino-mcp)
(provably fair рулетка, кости, coin flip, crash, blackjack, plinko) и скиллом для Claude Code.

## Что внутри

- `.mcp.json` — регистрация MCP-сервера `purple-flea-casino` (запуск через `npx -y @purpleflea/casino-mcp`).
- `.claude/skills/casino-mcp/SKILL.md` — скилл: как пользоваться инструментами казино, параметры рулетки, порядок ставки.
- `.env.example` — переменные окружения.

## Запуск

1. Нужен Node.js 18+.
2. Экспортируй ключ (необязательно, можно создать аккаунт инструментом `register`):

   ```bash
   export PURPLE_FLEA_API_KEY=sk_live_...
   ```

3. Открой проект в Claude Code, выполни `/mcp` и одобри сервер `purple-flea-casino`.
4. Попроси Claude, например: «поставь 1 доллар на красное в рулетке».

Внимание: сервер работает с реальными деньгами (USD/криптовалюта). Ключ API не коммить — он берётся из окружения.
