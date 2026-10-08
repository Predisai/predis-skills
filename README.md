# Predis skills for Claude

Make ads in Claude with Predis. Connect Predis once, then add only the skills you want.

## Set up

1. **Add the Predis connector.** In Claude, open Settings, then Connectors, then Add custom connector. Name it Predis, paste the URL below and sign in with your Predis account.

   ```
   https://mcp.predis.ai/mcp
   ```

2. **Add the skills you want.** Download a skill's zip from [Releases](../../releases/latest), then open Settings, Capabilities, Skills and upload it. Repeat for each skill you want.

   In Claude Code, copy a skill folder into `~/.claude/skills/` instead.

3. **Ask.** Type `/` and pick a skill, or just ask: "Make 4 static ads for this product page."

## Skills

| Skill | What you get |
|---|---|
| `link-to-ads` | 4 static ads for Meta from a product page |
| `ugc-ad` | A 20-second creator-style video ad with the hook up front |
| `product-video` | An 8-second studio video of one product |
| `reference-ad` | Your product in the structure of an ad you like |
| `carousel` | A 7-slide carousel from a blog post or idea |
| `hook-variants` | 5 new opening hooks on your video ad, rest unchanged |
| `headline-variants` | 6 headlines on the same visual, ready to split test |
| `fix-frame` | One shot in a video ad replaced, the rest untouched |
| `thumbnail` | YouTube thumbnails and Reels or Shorts covers |
| `viral-formats` | Short videos in proven trending formats |

## Good to know

- Each generation uses credits from your Predis plan. Claude tells you the cost before it starts.
- Skills that join or trim video clips use ffmpeg when it is available. Where it isn't, you get the clips and the exact edit to make.
- Predis makes the media. Publishing ads depends on the other tools you connect.

## License

MIT. See [LICENSE](LICENSE).
