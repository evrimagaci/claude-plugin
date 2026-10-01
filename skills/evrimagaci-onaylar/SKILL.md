---
name: evrimagaci-onaylar
description: Handle Evrim Ağacı (evrimagaci.org) approval requests and permission prompts correctly. Use when an Evrim Ağacı connector tool answers that it is waiting for the user's approval, returns a confirmation id or an evrimagaci.org approval link, states an ATP cost, or says a permission is missing; and when the user says they approved or rejected something, or asks whether an Evrim Ağacı action went through ("onayladım", "reddettim", "işlem oldu mu?").
---

# Approvals and permissions on Evrim Ağacı

Some actions do not run when a tool is called. They wait for the user's own decision on evrimagaci.org, where the user is logged in:

- every action that spends ATP, Evrim Ağacı's in-site points (the connector never spends money);
- publishing text under the user's name (comments, posts, questions, answers, edits, chat messages, direct messages, dictionary terms, publishing a draft, sharing a reading list) while the user's "approve what is published" preference is on, which is the default.

Nothing runs until the user approves on the site. A request expires 30 minutes after it is made.

## When a tool answers "Waiting for the user's approval"

The tool result is marked as an error, but it is not a failure: it means the action waits. The text names what will happen, the cost, a link and a confirmation id. The same values are in `_meta["evrimagaci.org/confirmation"]`: `id`, `url`, `expires_at` (Europe/Istanbul time), `summary` and `atp`.

1. Tell the user, in their language:
   - what will happen: the summary, as the tool gave it;
   - the cost: exactly the ATP amount the tool gave, or that it is free when there is no amount. Do not round, estimate or convert it;
   - the link, exactly as the tool gave it, and the deadline;
   - that the action has not happened yet.
2. Stop there. Do not call the same tool again: the connector hands back the same request and the action still waits. Do not change the arguments to get around the approval.
3. Keep the confirmation id for the next step.

Example reply:

> This comment waits for your approval on Evrim Ağacı. Cost: free. Open https://evrimagaci.org/uye/yapay-zeka/onay/<id> and choose Onayla (approve) or Reddet (reject) before 14:35 (Istanbul time). Nothing is published until you approve. Tell me when you have decided.

Pending requests are also listed on https://evrimagaci.org/uye/yapay-zeka under "Onayınızı bekleyenler".

## When the user says they decided

Call `confirmation_status` with the confirmation id, once. Then report by status:

| Status | Meaning | What to tell the user |
|---|---|---|
| `done` | The action ran. | The result (for example the URL of the new comment) and the ATP it cost. |
| `failed` | The user approved, but the site refused it: for example the price changed after the approval, the balance is short, or a site rule applies. | The reason in `error`. Offer a new request only if the user still wants the action. |
| `unknown` | The user approved, but the call broke off with an error partway through. It may or may not have taken effect. | That the outcome is unknown. Ask the user to check on evrimagaci.org before any new request, so nothing runs twice. |
| `pending` | Not decided yet. | The link and the deadline again. Do not check in a loop; wait for the user. |
| `approved` | Approved and still running. | That it is running; check once more when the user asks. |
| `rejected` | The user declined. | That nothing happened. Do not ask again unless the user asks. |
| `expired` | Nobody decided in 30 minutes. | That nothing happened. A new call makes a new request, only if the user wants it. |

## Never

- Never say an action is done, published, bought or sent before `confirmation_status` returns `done`.
- Never approve on the user's behalf, ask for their password, or suggest a way to skip the approval. The approval page needs the user's own login.
- Never suggest turning off the "approve what is published" preference to save time. If the user asks how, it is under "Tercihler" on https://evrimagaci.org/uye/yapay-zeka. ATP spends always wait, whatever the preference.

## When a permission is missing

Searching and reading public content needs no sign-in and no permission. Account reads and each kind of write need the user to sign in, and a permission the user grants per connection:

| Permission | Label on evrimagaci.org | Covers |
|---|---|---|
| `account:read` | See my account | saved items, reading history and lists, follows, notifications, ATP and UP, drafts |
| `library:write` | Organize my library | saving, reading-list items, following topics, marking notifications read |
| `social:write` | Interact for me | reactions, upvotes, reports to moderators, unfollowing |
| `content:write` | Write for me | comments, posts, questions, answers, edits, drafts, term suggestions, shared lists |
| `chat:read`, `chat:write` | Read Sohbet, Write in Sohbet for me | Sohbet community channels |
| `messages:read`, `messages:write` | Read my direct messages, Send direct messages for me | direct messages |
| `atp:spend` | Spend ATP | every action that costs ATP, each one approved on the site |

When a tool needs a sign-in or a permission the connection lacks:

1. Tell the user, in one sentence, which permission the task needs and why.
2. Claude then asks the user to sign in to Evrim Ağacı and allow it. With a personal key instead of a connection, the tool names the permission and the page https://evrimagaci.org/uye/yapay-zeka where the user allows it.
3. After the user allows it, call the tool again once. This is different from an approval request, which you never retry.

Ask only for the permissions the task needs. The user can remove any permission, or limit it to an hour, a day or a week, on https://evrimagaci.org/uye/yapay-zeka.
