---
name: build-a-website
description: Create a new website with Mondovo from a description of a business — gather a strong brief, generate three designs, help the user choose one, and publish it. Use when the user wants a new site, landing page or online presence.
---

# Build a website

Mondovo designs the site. Never write HTML or React for the user yourself.

## 1. Check first
Call `list_sites` if the user might already have a site for this business, so you
do not create a duplicate.

## 2. Gather the brief
Call `get_intake_questions`. Pre-fill every answer you can infer from the
conversation, then ask only the real gaps, a few at a time, and show your
assumptions so the user can correct them. The quality of the site depends on the
brief, so get: business name, what they sell, location, who the customers are,
what makes them different, the tone of voice, and the one action the site should
drive (the `goal`, for example "book a consultation").

Leave colours and mood open unless the user has a clear taste — then the three
designs come back genuinely different. If they want a site like one they have
seen, pass its address as `design.source_url`.

For a ready-made business (a directory, for instance), call `list_packs` and use
a `pack_slug` instead of a brief.

## 3. Create
Call `create_site`. It returns a `site_id` and starts building three designs.
This takes 3 to 15 minutes. Tell the user it is underway and roughly how long
it takes, and give a short update when the status changes. Poll `get_site` about
every 10 seconds when you need to know it has finished; only `ready` means all
three designs are done.

## 4. Choose a design
Call `list_variations` with the `site_id`. Look at each design's thumbnail and describe it in a
sentence so the user can compare them. Let the user choose — never choose for them. If screenshots are
still rendering, call it again in about a minute.

## 5. Publish
Only when the user asks. Suggest a subdomain from the business name (lowercase,
hyphens) and confirm it. Call `publish_site` with the chosen design's
`variation_id`. Publishing turns that design into its own site and returns a new
`site_id` — use it from then on. Poll `get_site` on the new id about every 15
seconds until `published`, then share the live link. Never call `publish_site`
again to check on a publish that is already running.

If the response is `contact_info_needs_confirmation`, show the user the contact
details on the site and ask whether they are right before continuing.
