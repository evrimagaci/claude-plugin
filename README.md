# Evrim Ağacı for Claude

[Evrim Ağacı](https://evrimagaci.org) is Turkey's largest independent popular science platform. This plugin lets Claude search and read its editor-reviewed Turkish articles, curated Q&A answers, featured blogs, science dictionary, podcast show notes and Akademi reading lists, and cite every source it uses. With your permission, Claude can also save articles, publish in your name and follow approvals on evrimagaci.org.

The plugin holds instructions (skills and commands) and one connector entry. It contains no code.

## What it does

| Skill | Use it to |
|---|---|
| `evrimagaci-kaynak-kontrolu` | check a scientific claim against Evrim Ağacı sources: supported, contradicted or insufficient evidence, with a URL for every point |
| `evrimagaci-konu-ozeti` | summarize a science topic from Evrim Ağacı articles, with sources |
| `evrimagaci-okuma-plani` | build an ordered reading plan for a beginner, intermediate or advanced reader, preferring an existing Akademi list |
| `evrimagaci-bugun` | get a short daily digest: new articles in the topics you follow, picks, unread notifications, the astronomy picture and the question of the day |
| `evrimagaci-onaylar` | handle actions that wait for your approval on evrimagaci.org, and permission prompts |
| `evrimagaci-yazma` | publish, edit, delete and undo comments, posts, questions, answers, messages and drafts, only with text you approved word for word |

Commands (in Claude Code and Cowork):

- `/evrimagaci:bugun` shows the daily digest.
- `/evrimagaci:kaynak-kontrolu <claim>` checks one claim.

Claude answers in the language you write in. The sources are in Turkish. Claude quotes them briefly and links each one.

## How to use it

1. Add the plugin, then add the **Evrim Ağacı** connector from the plugin's **Connectors** tab (in Claude Code, the connector loads with the plugin, and `/mcp` shows it).
2. In claude.ai and Claude Desktop, search and read public content right away. That needs no account. Claude Code connects from your computer, so it asks you to sign in when the connector first loads: run `/mcp`, select the `evrimagaci` server and select **Authenticate**.
3. In claude.ai and Claude Desktop, sign in only when a tool needs your account: your library, notifications, and anything that writes. Claude then asks you to sign in with your Evrim Ağacı account (free: https://evrimagaci.org/uye-ol). The consent screen shows the assistant's name and the permissions it asks for.
4. Each kind of account access has a permission of its own. Claude asks for one only when a tool needs it, and you decide each time.
5. Ask in your own words, or run a command.

Example prompts:

- "Is it true that we only use 10 percent of our brains? Check it against Evrim Ağacı."
- "Evrim Ağacı'na göre CRISPR nedir? Kaynaklarıyla özetle."
- "I want to learn evolution from scratch. Make me a reading plan from Evrim Ağacı."
- "Bugün Evrim Ağacı'nda ne var?"

You can see every connected assistant, change or time-limit its permissions, see its last actions and remove the connection at https://evrimagaci.org/uye/yapay-zeka.

## Permissions, approvals and undo

| Permission | Lets Claude |
|---|---|
| None, no sign-in | search and read public content |
| See my account | read your saved items, reading history and lists, follows, notifications, ATP and UP, drafts |
| Organize my library | save and unsave, edit your reading lists, follow topics, mark notifications read |
| Interact for me | react, upvote, report content to moderators, unfollow |
| Write for me | publish comments, posts, questions and answers under your name, edit and delete them, keep drafts |
| Read Sohbet, Write in Sohbet for me | read and write in Sohbet community channels |
| Read my direct messages, Send direct messages for me | read and send direct messages |
| Spend ATP | take actions that cost ATP, each one approved by you on evrimagaci.org |

- **Approvals.** Every action that spends ATP, and everything published in your name while "approve what is published" is on (the default), waits for you on evrimagaci.org. Claude gives you the link and the exact cost. Nothing runs until you approve there.
- **ATP is not money.** ATP is Evrim Ağacı's in-site point balance, earned on the site. The connector never makes payments with money.
- **Label.** Content published or edited through an assistant shows the small label "Asistan aracılığıyla" ("via an assistant"), unless you turn it off.
- **Undo.** Every action is listed under "Son işlemler" on https://evrimagaci.org/uye/yapay-zeka. Reversible actions have an undo button there for seven days. Deletes are final.

## What data it sends

- The plugin runs no code of its own. Claude sends data only through the Evrim Ağacı connector, to `https://evrimagaci.org/mcp`, a server that Evrim Ağacı runs. The plugin sends nothing to any other service.
- For each tool call, Claude sends only that tool's arguments: search words, document ids or URLs, and for account tools and writes, the action's content (for example the text of a comment you approved or a list item). The connector does not read your conversation or your other files.
- Evrim Ağacı keeps a record of each action taken through the connector (the tool, a short summary and a link), shown to you under "Son işlemler" and deleted after 180 days. Unpublished drafts are deleted after 90 days without a change.
- Evrim Ağacı stores each approval request with its full arguments (for example the text of a message or a comment) and its outcome. It deletes the request 30 days after it was made.
- Each tool call writes one event to Evrim Ağacı's operational logs. The event holds your account id (none when you are signed out), the assistant, the connection's client id, the tool name, the outcome, the duration and the network address that sent the call. For some tools it also holds the number of results or the type of the document read. It holds no arguments and no results. Evrim Ağacı keeps these logs for 60 days. Search queries and access tokens are not written to the logs.
- When you use the connector without signing in, Evrim Ağacı counts the requests from each network address (for IPv6, from each /64 network) in a one-minute window, and its tool calls in a one-day window. The counter keeps the address for at most one day. Tool calls from Claude's shared servers count together in one shared one-minute window, so the counter keeps no address for them.
- How Evrim Ağacı handles personal data: the privacy policy at https://evrimagaci.org/s/6964.

## Troubleshooting

- **A tool asks you to sign in.** In claude.ai and Claude Desktop, select **Connect** and sign in to Evrim Ağacı. In Claude Code, run `/mcp`, select the `evrimagaci` server and select **Authenticate**. If Claude does not continue by itself, ask again.
- **"Sign in to continue" when you are signed out.** The limit for use without an account is reached. Sign in. Your account has its own quota.
- **A permission is missing.** Claude asks you to connect again. Allow the permission on the consent screen. You can also allow it for the connection at https://evrimagaci.org/uye/yapay-zeka. The change applies to the next request.
- **An action waits for your approval.** Open the approval link that Claude gives you and select Onayla (approve) or Reddet (reject). Waiting requests are also listed under "Onayınızı bekleyenler" at https://evrimagaci.org/uye/yapay-zeka. A request expires after 30 minutes. After that, ask Claude again to make a new one.
- **Something else is wrong.** Read the setup steps at https://evrimagaci.org/yapay-zeka, or write to info@evrimagaci.org.

## Links

- About the connector and setup: https://evrimagaci.org/yapay-zeka
- Privacy policy: https://evrimagaci.org/s/6964
- Terms of use: https://evrimagaci.org/s/6961
- Content license: https://evrimagaci.org/s/6955
- Support: info@evrimagaci.org

## License

The plugin's files (these instructions and the connector entry) are released under the MIT License (see `LICENSE`). The MIT License does not cover the content on evrimagaci.org: articles, answers, dictionary entries and other site content keep their own license, at https://evrimagaci.org/s/6955.
