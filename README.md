# DataOrigin plugin

Find the B2B trade shows that fit your business, see which exhibiting companies match your ideal customer profile and prepare a shortlist of people to approach, from your AI assistant.

DataOrigin is a database of trade shows and of the companies that take part in them and their representatives. That includes exhibiting companies, attending companies, speakers and their representatives. The assistant works with your own DataOrigin account. Create one at https://dataorigin.es.

This repository holds two things.

- The connection to the DataOrigin server, which gives the assistant its tools.
- One skill, the trade show analyst, which tells the assistant how to use those tools in order.

Connecting the server gives you the tools. The skill comes only with the full plugin. Each section below says which one you get.

## What the assistant can do

- Read your account and your credit balance.
- Read your ideal customer profile. It cannot change it.
- Search trade shows by text, sector, country, region and dates, and sort them.
- Open one trade show and see how many exhibiting companies, attending companies and speakers DataOrigin has registered for it.
- Search the companies of one trade show by role, country, industry, category, size and text.
- Open one company with the representatives your account can see.
- Save trade shows, companies and filters to your account.
- Give you the link to a trade show or a company in the DataOrigin app.
- Read your usage and billing history.

With the trade show analyst skill you give the assistant the website of your product or company. It works out who you sell to, finds the trade shows that fit by sector, country and date, and ranks them with reasons and numbers. For the trade show you choose, it shortlists the exhibiting companies that fit and says why. When DataOrigin has representatives for a company and your account can see them, it names the person to approach. Not every company has a representative on record.

## Install

The server address is `https://api.dataorigin.es/mcp`. You sign in with your DataOrigin account in the browser the first time. You do not need to copy a token.

### Claude on the web and the desktop app

This connects the server. The skill is not included.

1. Open Connectors in Claude.
2. Select Add and then Add custom connector.
3. Type DataOrigin as the name, paste the server address and select Connect.
4. Sign in with your DataOrigin account and authorise the connection.

### Claude Code

To connect only the server, run

```
claude mcp add --transport http dataorigin https://api.dataorigin.es/mcp
```

Then run `/mcp` inside Claude Code and sign in to dataorigin.

To use the full plugin with the skill, download this folder and start Claude Code with

```
claude --plugin-dir ./dataorigin-plugin
```

### Cursor

This connects the server. The skill is not included.

1. Add this to `~/.cursor/mcp.json`, or to `.cursor/mcp.json` in your project.

```json
{
  "mcpServers": {
    "dataorigin": {
      "url": "https://api.dataorigin.es/mcp"
    }
  }
}
```

2. Open Cursor Settings, MCP, and select Connect next to dataorigin. From the terminal, run `cursor-agent mcp login dataorigin`.
3. Sign in with your DataOrigin account and authorise the connection.

### ChatGPT

This connects the server. The skill is not included.

1. Open ChatGPT Plugins at https://chatgpt.com/plugins.
2. Select Add and then Add custom MCP server.
3. Type DataOrigin as the name, paste the server address and leave OAuth as the authentication.
4. Accept the notice and select Create as a plugin.
5. Select Connect, sign in with your DataOrigin account and authorise the connection.
6. In a chat, type `@DataOrigin` before your question.

## Things to ask

- "Which food trade shows take place in Spain in the next six months? Order them by number of exhibitors."
- "Here is my website. Which trade shows should I attend in 2027 and why?"
- "Show me the Italian exhibitors of Fruit Attraction with more than 50 employees."
- "For these five companies, who is the person I should talk to?"

## Credits

Searching and reading do not consume DataOrigin credits. Two actions do.

- Unlocking a trade show.
- Unlocking one company.

The assistant gives you the exact cost before each one. Current prices are in the DataOrigin app, under Settings, Pricing.

Both tools answer with the cost before they unlock anything, and they unlock only when the assistant sends a confirmation. The skill instructs the assistant to show you that cost and to ask for your approval before it continues. Read what the assistant proposes before you approve it.

When data is locked, the skill instructs the assistant to say so and to give the price. Unlocking a company shows what DataOrigin has for it. It does not guarantee that a contact exists for every company.

Unlocking from the assistant works in Claude, ChatGPT, Cursor and clients that run on your own computer, such as Claude Code. In any other client the assistant can search, read and save, and you unlock in the DataOrigin app.

The assistant cannot buy credits, change your plan or delete your account. You do that in the DataOrigin app.

## Where the data comes from

The data about companies and their representatives comes from what companies and trade shows publish on their websites and platforms, and from external providers that supply it under contract. DataOrigin processes the professional contact data of trade show participants on the basis of legitimate interest, under article 6.1.f of the GDPR and article 19 of Spanish Organic Law 3/2018. If you appear in DataOrigin and want your data corrected or removed, write to info@dataorigin.es. The full policy is at https://dataorigin.es/en/privacy/.

## Disconnect

You can disconnect the assistant at any time in the DataOrigin app, under Settings, MCP.

## Links

- Documentation https://dataorigin.es/en/docs/mcp/
- Guides https://dataorigin.es/en/docs/guides/
- Website https://dataorigin.es
- Privacy https://dataorigin.es/en/privacy/
- Terms https://dataorigin.es/en/terms/
- Support https://dataorigin.es/en/contact-us/ and info@dataorigin.es

[![mcptry](https://mcptry.com/badge/es.dataorigin/dataorigin)](https://mcptry.com/servers/es.dataorigin/dataorigin)

## License

MIT. See the LICENSE file.
