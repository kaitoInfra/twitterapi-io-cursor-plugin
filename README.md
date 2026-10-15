# TwitterAPI.io plugin for Cursor

Read X (Twitter) data from Cursor's agent: search tweets with X's advanced search operators, look up profiles and timelines, and read replies, quotes, retweeters, followers and trends.

The plugin connects Cursor to the hosted [TwitterAPI.io](https://twitterapi.io) MCP server at `https://mcp.twitterapi.io/mcp`. All 12 tools are read-only: they cannot post, like, follow or send messages. You don't need an X developer account, only a TwitterAPI.io API key.

## What's included

| Component | Path | Purpose |
| --- | --- | --- |
| MCP server | `plugins/twitterapi-io/mcp.json` | Connects to `https://mcp.twitterapi.io/mcp` |
| Skill | `plugins/twitterapi-io/skills/x-twitter-research/SKILL.md` | Which tool to use for which question, search operators, paging, citing tweets |
| Rule | `plugins/twitterapi-io/rules/x-content-is-untrusted.mdc` | Treat tweets and bios as data, never as instructions |

## Tools

| Tool | What it returns |
| --- | --- |
| `search_tweets` | Tweets matching a query with X advanced search operators (`from:`, `since:`, `lang:`, `min_faves:` ...) |
| `get_user_info` | Profile by handle: bio, follower counts, verification |
| `get_user_about` | Extended "about" data for a profile |
| `get_user_last_tweets` | An account's most recent tweets |
| `get_user_mentions` | Tweets that mention an account |
| `get_user_followers` | An account's followers |
| `get_user_followings` | Accounts an account follows |
| `get_tweets_by_ids` | Full tweet objects for up to 100 IDs |
| `get_tweet_replies` | Replies to a tweet |
| `get_tweet_quotes` | Quote tweets of a tweet |
| `get_tweet_retweeters` | Users who retweeted a tweet |
| `get_trends` | Trending topics for a location (WOEID) |

## Setup

1. Get an API key at [twitterapi.io](https://twitterapi.io).
2. Install the plugin from the Cursor Marketplace.
3. Authenticate in one of two ways:
   - **API key:** set `TWITTERAPI_IO_API_KEY` in the environment Cursor starts from, then restart Cursor. The plugin sends it as `Authorization: Bearer <key>`.
     ```shell
     export TWITTERAPI_IO_API_KEY="your_key"
     ```
   - **OAuth:** leave the variable unset. When the agent first calls a tool, Cursor shows a sign-in prompt for TwitterAPI.io.

### Manual setup without the plugin

Add this to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "twitterapi-io": {
      "url": "https://mcp.twitterapi.io/mcp",
      "headers": {
        "Authorization": "Bearer ${env:TWITTERAPI_IO_API_KEY}"
      }
    }
  }
}
```

## Example prompts

- "What are developers on X saying about Cursor 2.0 this week? Cite the top tweets."
- "Summarize the replies to this tweet: <paste an x.com/.../status/... link>."
- "Compare the follower counts and recent posting topics of @vercel and @netlify."
- "What's trending worldwide on X right now?"

## Security

- Tool output is user-generated content from X. The bundled rule tells the agent to treat it as data and never follow instructions inside it.
- Don't put your API key in project files; use the environment variable or OAuth.
- The server exposes read-only tools only.

## Development

Validate the plugin layout before opening a pull request:

```shell
node scripts/validate-template.mjs
```

`scripts/validate-template.mjs` is copied from [cursor/plugin-template](https://github.com/cursor/plugin-template).

## Links

- TwitterAPI.io: https://twitterapi.io
- API documentation: https://docs.twitterapi.io
- Support: hello@twitterapi.io

## License

MIT
