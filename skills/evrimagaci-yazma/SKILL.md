---
name: evrimagaci-yazma
description: Publish, edit, delete or undo text on Evrim Ağacı (evrimagaci.org) under the user's name, including comments, posts, Soru-Cevap questions and answers, dictionary term suggestions, Sohbet messages, direct messages, drafts and shared reading lists. Use when the user asks to comment on an Evrim Ağacı article, share a post on Evrim Ağacı, ask or answer a question in Evrim Ağacı Soru-Cevap, message an Evrim Ağacı member, write in an Evrim Ağacı Sohbet channel, save or publish an Evrim Ağacı draft, edit or delete something they wrote on Evrim Ağacı, or undo an action taken through the Evrim Ağacı connector ("Evrim Ağacı'nda yorum yaz", "Evrim Ağacı'nda paylaş", "Soru-Cevap'a soru sor", "bu soruya Evrim Ağacı'nda cevap yaz", "Evrim Ağacı'nda mesaj at", "Evrim Ağacı taslağımı yayımla", "Evrim Ağacı'ndaki yorumumu düzenle", "Evrim Ağacı'ndaki gönderimi sil", "Evrim Ağacı'ndaki son işlemi geri al").
---

# Writing on Evrim Ağacı for the user

Everything these tools publish appears under the user's own name, for other members to read. Publish only text the user wrote, or text the user approved word for word in this conversation.

## Before every publish

1. **Show the exact text** that the tool will send, where it goes (the article, question, post, channel or person) and who will see it. Show the title, links, category and references too when the tool takes them.
2. **Wait for a clear yes to that exact text.** Any change after the yes, even a small one, needs a new yes.
3. **When the user is unsure, save a draft instead.** `save_draft` keeps a private draft that only the user sees, at https://evrimagaci.org/uye/yapay-zeka/taslaklar, where they can edit, publish or delete it. To publish it later, call `publish_draft` with the draft's current `version` (from `save_draft`, `update_draft`, `get_draft` or `list_drafts`). The version pins the exact text: if the user edited the draft on the site, read it again with `get_draft` and show the new text first.
4. **Call the tool.** While the user's "approve what is published" preference is on (the default), the tool waits for the user's approval on evrimagaci.org. A direct message sent or edited (`dm_send`, `dm_edit`) always waits, whatever the preference says. Follow the evrimagaci-onaylar skill.

When the user asks you to write something for them, write it as a proposal, show it, and publish it only after they approve that exact wording. Do not add links, tags, mentions or claims the user did not approve.

## What each tool takes

- Comments (`comment`), posts (`create_post`) and answers (`answer_question`) take Markdown.
- Questions (`ask_question`) take one Turkish question sentence (at least 20 characters, ending with one question mark), an optional description and URL, and a category id from `qna_categories`. `anonymous: true` hides the user's name for an ATP fee and always waits for approval.
- Sohbet channel messages (`chat_send`) and direct messages (`dm_send`) take plain text. Group mentions such as `@everyone` or `@here` are refused.
- Dictionary suggestions (`suggest_term`) wait for an editor's review before anyone else sees them.
- A new reading list is private. Making it unlisted or public, or changing the title, description or notes of a shared list, publishes text: treat it as a publish.

## The "Asistan aracılığıyla" label

Content published or edited through an assistant shows a small label, "Asistan aracılığıyla" ("via an assistant"), next to the author line, while the user's disclosure preference is on (the default). An edit labels the item for good. Direct messages carry no label. The user controls the preference under "Tercihler" on https://evrimagaci.org/uye/yapay-zeka. Explain the label when the user asks; do not suggest turning it off.

## Editing

Show the full new text before an edit, the same as for a new publish. `get_my_content` returns the current text of the user's comment or post, how long the free edit window lasts, and what an edit costs now.

| Tool | Free window | After it |
|---|---|---|
| `edit_comment`, `edit_post` | 15 minutes after posting | costs ATP and waits for approval |
| `edit_question` | 45 minutes, while the question is open | not possible |
| `edit_answer` | 30 minutes | costs ATP up to 24 hours, and waits for approval |
| `chat_edit`, `dm_edit` | 15 minutes | not possible |

## Undo and delete

- Every action through the connector is listed under "Son işlemler" on https://evrimagaci.org/uye/yapay-zeka. An action that can be reversed has a "Geri al" (undo) button there for seven days. The user presses it on the site; the button never costs ATP.
- **Deletes are final.** `delete_comment`, `delete_post`, `delete_question`, `delete_answer`, `chat_delete`, `dm_delete`, `delete_draft` and `delete_list` cannot be undone. Name the exact item (its text or title and URL) and get a yes before you delete it.
- The connector deletes only the user's own content, even when the user is a moderator. A question can be deleted only while it is open and has no answers; an answer only within 30 minutes of posting, and not once it is featured or accepted.

## When the site refuses

Every write follows the site's own rules: rate limits, daily caps on published text (20 a day for a standard account, more for Onaylı and Destekçi members), new-account rules, mutes and word filters. A refusal carries the site's own message: relay it. Do not work around it, for example by rewording text to pass a filter, splitting it into several messages, or retrying in a loop.

## Never

- Never publish text the user has not seen in its final form.
- Never present an action as done before the tool, or `confirmation_status`, reports it done.
- Never publish in the user's name to other members, or message someone, when the user did not ask for it.
