<p align="center">
  <img src="pgp.png" alt="Robby" width="240" />
</p>

<h1 align="center">Robby</h1>

<p align="center">
  <strong>Bankr-style AI agent. Robinhood rails.</strong><br />
  Talk in English. Robby trades in your Agentic account.
</p>

<p align="center">
  <a href="https://agent.robinhood.com/mcp/trading"><img src="https://img.shields.io/badge/MCP-robinhood--trading-00ff55?style=flat-square" alt="MCP" /></a>
  <img src="https://img.shields.io/badge/agent-Bankr%20lane-111111?style=flat-square" alt="Bankr lane" />
  <img src="https://img.shields.io/badge/account-Robinhood%20Agentic-00c805?style=flat-square" alt="Agentic" />
  <img src="https://img.shields.io/badge/mode-you%20own%20the%20fills-ff3b30?style=flat-square" alt="You own the fills" />
</p>

Robby is an agentic trading agent built in the same lane as [BankrBot](https://bankr.bot) — a chat-native executor that does not stop at research. You tell it what to do. It sizes, routes, and places the ticket.

Bankr did this for crypto wallets. Robby does it for **Robinhood Agentic Trading** through the official [Trading MCP](https://agent.robinhood.com/mcp/trading).

This is not a Robinhood product, not Bankr, and not an advisor. It is a third-party agent that speaks the public Trading MCP so an AI can trade directly for you inside a dedicated Agentic account.

---

## What this is

Robinhood Agentic Trading lets a third-party AI agent connect to a dedicated brokerage account over MCP (Model Context Protocol). The agent is not a chatbot with a quote widget taped on. It can read the book and **place orders** in the Agentic account.

Robby is that agent.

| | Bankr | Robby |
| --- | --- | --- |
| Surface | Web terminal, X, Telegram, CLI | Cursor, Claude, ChatGPT, Codex, Grok, any MCP host |
| Venue | Self-custody wallet / chain | Robinhood Agentic brokerage account |
| Pipe | Agent API / SDK | `https://agent.robinhood.com/mcp/trading` |
| Moves | Swaps, limits, DCA, TWAP, bridges | Equities, options, crypto (where enabled), automations |
| Control | You still own every fill | You still own every fill |

MCP is the pipe. Instead of answering questions, an agent with MCP access takes actions on your behalf — portfolio reads, order previews, live tickets.

```
you  ──prompt──►  Robby  ──MCP──►  Robinhood Agentic account
                      │
                      └── read: all RH accounts
                          write: Agentic only
```

---

## What Robby can do

Once the Trading MCP is connected and the Agentic account is open, Robby can ask about portfolio value, buying power, and account info, then help with investing — including placing the order types Robinhood exposes to the agent.

**Build books**
> Look through news and industry reports to build a portfolio that represents little-known tickers across the AI supply chain.

**Automate**
> Buy $100 of ROAR every time the price decreases 2% or more in 1 day.

**Rebalance**
> Rebalance my portfolio to achieve a 20% allocation in ROAR and 80% allocation in HMNI.

**Risk**
> Look at my portfolio and tell me what risks I’m exposed to.

**Tape / thesis**
> Why is ROAR up today?
> Look at news, social sentiment, and recent quotes to build a bull and bear thesis for ROAR.

Those prompts are the same class of command Bankr users already fire (`buy $20 of PEPE`, `DCA`, `limit`, `every day at 9am check my book`). Robby maps that language onto Robinhood tickets instead of swaps.

> Examples are informational. They are not a recommendation or endorsement.

---

## Connect Robby to Robinhood

Endpoint (Streamable HTTP):

```
https://agent.robinhood.com/mcp/trading
```

Auth is OAuth 2.1 (authorization code + PKCE). No client secret. One desktop approval per host. Robinhood does not publish the full tool schema — hosts discover tools at runtime via `initialize` then `tools/list`. The one publicly named tool is `review_equity_order` (simulate / pre-trade warnings). Everything else comes off the live server.

You cannot open an Agentic account from a phone. If the MCP flow starts on mobile, copy the onboarding URL into a desktop browser.

### Cursor

1. Give the agent `https://agent.robinhood.com/mcp/trading`
2. Settings → Cursor Settings → Tools & MCPs → Connect

This repo ships `.cursor/mcp.json` so Cursor picks up `robinhood-trading` locally.

### Claude Code

```bash
claude mcp add robinhood-trading --transport http https://agent.robinhood.com/mcp/trading
```

Then `/mcp` → `robinhood-trading` → authenticate.

### Claude Desktop

Settings → Connectors → Add custom connector → paste the MCP URL.

### ChatGPT

Settings → Security & login → Developer Mode → Plugins → `+` → paste the MCP URL.

### Codex

Settings → MCP servers → Streamable HTTP → paste the MCP URL.

CLI:

```bash
codex mcp add robinhood-trading --url https://agent.robinhood.com/mcp/trading
```

Then `/mcp` → `robinhood-trading`.

### Grok

Chat → `+` → Add connector → Custom → paste the MCP URL.

### Anything else that speaks MCP

Same URL. If the host supports Streamable HTTP MCP, Robby / that host can sit on the same pipe.

---

## Open the Agentic account

A Robinhood Agentic account is a self-directed individual investing account. You can have up to 10 self-directed individual accounts including this one.

Required:

1. A primary individual investing account in good standing
2. Connect an agent (Robby, Claude, Cursor, Codex, …) to the Trading MCP
3. Complete the onboarding that auto-opens after OAuth — desktop only

Robby can only **place** in the Agentic account. Reads can span all of your Robinhood accounts.

---

## What the agent can see

When Robby is connected to the Trading MCP it gets **read** access to:

- All Robinhood accounts, including account numbers
- Positions and balances
- Transactions and order history
- Watchlists and scans

Writes (orders) stay inside the Agentic account. Full stop.

---

## Crypto

To have Robby place crypto in the Agentic account you need:

- A Robinhood Crypto account that corresponds to the Agentic account
- The updated Crypto customer agreement + agentic disclosures accepted

Robby cannot open Crypto for you. It can point you at the app. After you accept, crypto trading on the Agentic account turns on.

Robby can **trade** crypto pairs that Robinhood lists. It cannot transfer, stake, or lend. Those stay in the app.

**State restrictions.** Agentic crypto is not available everywhere, including New York.

- Move *into* a restricted state → new crypto tickets stop. Open orders are not cancelled.
- Move *out* of a restricted state → access restores after 45 days, with a fresh agreement.

---

## Wallet surface

Robby’s execution venue is Robinhood, not a self-custody wallet. MetaMask (and the usual provider stack — `@metamask/sdk`, providers, JSON-RPC engine, onboarding) is optional identity / Robinhood Wallet adjacency, not the fill path.

Bankr signs on-chain. Robby submits through the Trading MCP into the Agentic book. Do not confuse the two rails.

---

## You are the risk

You are responsible for every trade this agent places. You are in charge. When Robby sends an order, the investment decision is yours.

Before it acts, you can review the ticket. If you tell it to go without asking, it can fill without another confirmation.

Agentic trading is a different product than clicking a ticket yourself:

- AI agents misread instructions, size wrong, and run on stale tape
- Strategies can fail fast and be hard to kill in real time
- You can lose the entire sleeve
- Robinhood does not guarantee agent output and is not on the hook for third-party model errors
- You assume the risk of whatever model host you plugged in (Claude, ChatGPT, Grok, Cursor, …)

This product is not appropriate for everyone. Read Robinhood’s own [Agentic Trading overview](https://robinhood.com/us/en/support/articles/agentic-trading-overview/) and disclosures before you connect anything.

Brokerage: Robinhood Financial LLC (member SIPC) / clearing via Robinhood Securities, LLC (member SIPC). Crypto: Robinhood Crypto, LLC. Crypto is not FDIC insured or SIPC protected.

---

## Troubleshooting

1. Confirm the host actually attached `https://agent.robinhood.com/mcp/trading`
2. Disconnect / reconnect the Trading MCP
3. Follow the host’s own MCP debug docs
4. Ask the agent to list its Robinhood tools — if the list is empty, you are not on the pipe
5. If the agent says the error is on the Robinhood side, that is a Robinhood Support ticket, not a Robby bug

Most failures are a dead MCP session. Re-auth on desktop.

---

## Repo

Robby lives at [github.com/Coinvoro/Robby](https://github.com/Coinvoro/Robby).

Pixel mascot is `robby.png`. Wire the Trading MCP, open the Agentic account, then talk to it like Bankr — except the fills print on Robinhood.
