# Posting comments to a GitLab merge request

## The budget

Treat it as a limit, not a target.

- **One blocker:** the ask in a sentence, then three to five sentences — what breaks,
  when it breaks, and the suggested fix.
- **Non-blocking notes:** one or two sentences each, two or three notes in total.
- **Something done well:** one sentence, naming the decision rather than the person.
- Anything beyond that belongs in the report, not on the merge request.

**What to cut, in this order.** The walk-through of the mechanism — the author knows
their own codebase, so name the file, the line and the consequence, not the call
chain. Framework internals. Anything visible in their own diff. Checklist categories
with nothing to say. Restating the task back at them. And every recommendation with no
failure scenario behind it.

**What survives the cut:** `file:line`, what actually breaks, and the fix.

Two habits that keep it short. Group the non-blocking notes into one paragraph instead
of a bullet per finding. And when the same problem shows up across several merge
requests of one stack, say so once and propose a single fix for the whole feature
rather than repeating the explanation in each.

The reason this is a rule and not a preference: a wall of generated text costs the
author more time to read than the fix takes to make, and it buries the one thing that
actually blocks them.

## The shape of a comment

Tag the author first — their username comes from the merge request JSON
(`.author.username`). Lead with the ask, then the detail:

```
@username, please resolve the conflicts in <file>.

Some details: <what breaks, when, and the suggested fix>
```

In English, whatever language the review was discussed in.

## Anchoring a comment to a line

A comment on the line it is about is worth several paragraphs of explanation of where
to look. Use a general comment only when there is no line to attach to.

Inline comments need a **nested JSON `position` object**, sent with `--input`. The flat
form (`-f 'position[position_type]=text'`) is silently ignored by GitLab: the API
answers 201 and you get a general comment instead, which is how this goes wrong
without anyone noticing.

1. Get the diff refs:

   ```sh
   glab api projects/<url-encoded-path>/merge_requests/<iid>
   ```

   Take `.diff_refs` — `base_sha`, `head_sha`, `start_sha`.

2. Build the payload:

   ```json
   {
     "body": "<comment text>",
     "position": {
       "position_type": "text",
       "base_sha": "...",
       "head_sha": "...",
       "start_sha": "...",
       "new_path": "<file path>",
       "new_line": 42
     }
   }
   ```

   For a deleted line use `old_path` and `old_line` instead.

3. Post it:

   ```sh
   glab api -X POST -H 'Content-Type: application/json' \
     projects/<url-encoded-path>/merge_requests/<iid>/discussions \
     --input comment.json
   ```

4. **Verify.** The note in the response must have `"type": "DiffNote"`. A general
   comment has `"type": null` — if that is what came back, delete it
   (`glab api -X DELETE .../notes/<note_id>`) and post again with a corrected
   position. Do not leave both.

## Before posting anything

Show the developer what you are about to post and wait, unless they said to post
directly. A comment cannot be unsent from the author's notification, and a wrong one
costs more than the minute it takes to read it first.
