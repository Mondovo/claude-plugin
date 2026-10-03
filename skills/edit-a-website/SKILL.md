---
name: edit-a-website
description: Change an existing Mondovo website — text, prices, images, layout, styling, new pages — and undo changes. Use when the user wants to update, fix, redesign part of, or add to a site they already have.
---

# Edit a website

Find the site with `list_sites` (or use the `site_id` already in the
conversation). Then pick the lightest tool that does the job:

| The user wants | Use |
|---|---|
| Different words, prices, FAQ answers, links or image addresses | `patch_variation_content` — instant, no redesign |
| A layout, style or section change ("make the hero darker", "add testimonials") | `refine_site` with the request in plain words |
| A new page (About, Services, Contact…) | `add_page`, or `add_pages` for several |
| To go back to how it was | `list_versions`, then `restore_version` |

- `refine_site`, `add_page` and `add_pages` return a `job_id`. Poll `get_job` about
  every 10 seconds until it finishes, and tell the user when it is done. A finished job can still list items in `failed` — tell the
  user about each one.
- To show a reference image for a change, upload it with `upload_asset` and pass
  the returned address in `image_urls`.
- If a site is busy with another change you get `site_busy` with that job's id —
  wait for it, then try again.
- Every change is saved in the site's version history and can be undone. If a
  response carries `history_warning`, tell the user the change saved but was not
  recorded before making more edits.
- Changes are not live until the site is published again. Offer to publish after
  a round of edits (see the `build-a-website` skill, step 5).
