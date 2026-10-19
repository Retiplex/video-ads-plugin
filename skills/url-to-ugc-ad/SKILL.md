---
name: url-to-ugc-ad
description: Research a brand and its customers, turn the findings into a creative strategy, then produce a UGC-style video ad with an AI actor through the Retiplex MCP server. Use this whenever someone wants a video ad, UGC ad, TikTok/Reels/Shorts ad, product video or creative for paid social for a product, store or brand — especially when they give a product URL or brand name and say things like "make an ad for this", "create a UGC video", "I need ad creatives for my Shopify store", or "research this brand and make a video". Use it even if they don't mention Retiplex or research.
compatibility: Uses the Retiplex MCP server (https://app.retiplex.com/mcp, OAuth), which the url-to-ugc-ad plugin bundles, and web search or web fetch for the research phase. Video generation needs a paid Retiplex plan.
---

# URL to UGC ad — research-driven video ads with Retiplex

Most AI ads fail for the same reason most human ads fail: they say generic things
("high quality", "game changer") to nobody in particular. This skill fixes that by
doing the creative research a good UGC strategist would do first, then feeding the
specific findings (who buys, what hurts, the words customers actually use, the proof
that exists) into Retiplex, which writes the script, casts the actor, plans the
scenes and renders the vertical video.

The work has three phases with one user checkpoint between each:

1. **Research** the brand, product, customers and competitors (free, web only).
2. **Decide** the angle with the user (format, hook, language, quality, length).
3. **Build** the ad in Retiplex, review the script and storyboard, render it.

## Phase 0 — Before anything else

Call `get_account`. It returns the credit balance, the plan, whether the account can
generate videos (`canGenerateVideos`) and the longest script allowed.

- The url-to-ugc-ad plugin ships the Retiplex connector with this skill, so its tools are normally
  present. The first call opens a Retiplex sign-in in the browser (in Claude Code,
  the user runs `/mcp` and picks Retiplex); new users can create an account there
  or at https://app.retiplex.com/sign-up. You can't sign in for them.
- If the Retiplex tools are not available at all (the skill was installed on its
  own, or the connector is turned off), tell the user to enable the Retiplex
  connector, or add the remote MCP server `https://app.retiplex.com/mcp` themselves
  (Claude: Settings → Connectors → Add custom connector). Setup for every client:
  https://app.retiplex.com/help/agents. You can still do the research phase while
  they connect.
- If `canGenerateVideos` is false, say so up front: a free account can set up the
  project (product, brief, actor, script) but the storyboard and video need a paid
  plan. Give them the subscription URL from the result. Don't discover this after
  20 minutes of work.

## Phase 1 — Creative research

Goal: a short **Creative Research Brief** the user can read in two minutes, where
every claim traces back to something you found. Read
`references/research-playbook.md` for where to look, what to extract and the brief
template.

Scale the depth to the request. A product URL and "make me an ad" gets a focused
pass (roughly 5–10 searches/fetches): the product page, reviews, one or two
community sources, a look at competitors. "Do proper research first" or a launch
campaign gets a deeper pass. Do the research yourself rather than delegating it;
each finding usually tells you where to look next.

What matters most, in order:

1. **What the product actually is and does**, including price and the offer.
2. **Who buys it and why**, in their words: the problem they had, what they tried
   before, what convinced them, what they worried about.
3. **Proof that exists**: review count and rating, specific results customers
   report, press, certifications, guarantees. Only real, checkable proof.
4. **The competitive context**: how competitors and the category usually advertise,
   so the ad can sound different.
5. **Brand voice and look**: how the brand talks, its visual style, who appears in
   its own content.

Customer language is the most valuable thing you'll find. A review that says "I
stopped hiding my hands in photos" beats any benefit statement you could write.
Quote it in the brief (short excerpts) and carry it into the script.

### Honesty rules for the research

The ad will show an AI actor speaking as if they were a customer, so the script must
not put words in their mouth that no real customer could truthfully say.

- Never invent results, numbers, ratings, awards or testimonials. If you can't find
  proof for a claim, the ad doesn't make it.
- Note anything the product page claims that you couldn't verify, and keep it out of
  the script unless the user confirms it.
- For health, supplements, skincare actives, finance, weight loss or anything
  regulated, flag it in the brief: these categories have strict rules on claims and
  on ads that look like testimonials. Keep claims to what the brand can substantiate.
- Mention that most ad platforms (Meta, TikTok, YouTube) require or encourage
  labelling AI-generated people; the user decides how they publish, but they should
  know.

## Phase 2 — Choose the angle with the user

Present the brief, then propose **two or three distinct angles**, each one sentence
plus a hook line, and recommend one. An angle is a combination of format, audience,
core message and hook — see `references/retiplex-mapping.md` for how research maps
to Retiplex's formats and funnel stages.

In the same message, ask for the settings Retiplex needs before it can start. If
your client has a multiple-choice question tool, use it so this is one round:

- **Angle** — which one (your recommendation first).
- **Spoken language** — en, cs, nl, de, it, pl, sk or es, plus accent if it matters.
  Skip asking when the market makes it obvious.
- **Video quality** — `standard` (affordable) or `premium` (Seedance 2.5, most
  realistic, about 28 credits per second of video).
- **Length** — suggest one from the angle (15–25 s for cold audiences, 30–45 s for
  storytime or objection-heavy products), within the account's maximum.

If the user said to just go ahead, pick the recommended angle and the defaults
(language from the market, standard quality, suggested length), say what you picked
in one line, and continue. Still confirm costs before the storyboard and the video.

## Phase 3 — Build the ad in Retiplex

Follow the steps in order. After every call, `get_project` (or the call's own result)
says the current step and `nextTool`; trust it if it disagrees with this list. Every
step that spends credits: say what it costs in one line before you run it.

| Step | Tool | Typical cost |
|---|---|---|
| Create project | `create_project` | 5 credits with a `product_url` |
| Classify photos | `view_images` → `update_product_media` | free |
| Creative brief | `set_creative_brief` | a few credits only if facts are left to deduce |
| Actor | `set_actor` | about 11 (14 on premium) |
| Optional cast / moments | `add_subject`, `add_moment` | free; a generated subject image ~8 |
| Script | `generate_script` | about 6; free when you pass your own |
| Storyboard | `generate_storyboard` | about 10 + ~5 per scene |
| Video | `start_video_generation` | `estimatedVideoCredits` from `get_project` |

**1. Create the project.** Pass `product_url` (imports name, description and up to 3
photos), `language`, `language_accent` if chosen, and `video_quality`. Add better
photos you found during research as `media` with their `angle` (a clean front shot
matters most). If the product has no usable photos, ask the user for some before
going further — the storyboard is built from them.

**2. Classify the photos.** About a minute after creation, `get_project` lists
imported photos with what they `needs`. Look at them with `view_images` and tag each
with `update_product_media` (its view, or a description for `detail` shots and for
digital products). Remove photos that are collages, heavy with text, or show a
different product. The storyboard waits for this.

**3. Set the creative brief** from the chosen angle. Fill every field yourself from
the research so nothing has to be guessed: `format`, `target_audience` (specific:
who, situation, what they've tried), `benefit_to_highlight` (one benefit, in
customer language, with the proof), `primary_ad_goal`, `funnel_stage`. For a
`custom` format, write the full concept into `idea`, including the hook line and the
proof points to use.

**4. Cast the actor** with `set_actor`. Base the archetype on who actually buys and
speaks in the reviews and on the market the ad runs in, not on stereotypes; if the
research doesn't make it clear, leave `archetype` out and Retiplex casts someone
fitting the audience. Show the user the portrait (`previewUrl`, or the project card)
and offer one recast if they don't like it. On premium the actor must be cast on
premium; if the quality changes later, recast.

**5. Optional moments.** Add a moment only when the research points to a visual the
ad can't do without: the texture of a cream, the product working, an unboxing that
reviewers rave about. One or two at most; `product-shot-faceless` or
`product-action` with a short `text_overlay` carrying the key proof work well.
Everything else, let the script and storyboard decide. Add moments before the script.

**6. Script.** Call `generate_script` with `target_seconds`. Then audit it against
the research before showing it:
- Does the hook land in the first sentence and use the angle's hook idea?
- Does it use real customer language and real proof?
- Does it make any claim you couldn't verify? Remove it.
- Is it in the project's spoken language and the right length?

If it falls short, either call `generate_script` again for a variation, or edit it
and save your version with `generate_script(script=...)` (free; spoken text only).
Show the user the final script with a one-line note on what it's built from, and get
an OK. Changing the script after the storyboard means regenerating the storyboard,
so settle it here.

**7. Storyboard.** Confirm the cost, then `generate_storyboard` (pass the chosen
`video_quality`). It runs 1–3 minutes: poll `get_project` every 20–30 seconds until
the storyboard has scenes. Review the scenes against the research — the product is
shown correctly, nothing visual contradicts the brand, no scene implies a result the
product can't deliver — and summarise them for the user. The project card
(`show_project`) displays them.

**8. Render.** Read `estimatedVideoCredits` from `get_project`, tell the user, and
after they confirm call `start_video_generation` with that number as `max_credits`.
Rendering takes 10–30 minutes; poll `get_video_status` every couple of minutes, not
faster. The user also gets an email when it's done. If the status reports a failure,
follow what it says: `resume_video_generation`, or finishing in the app via
`appUrl`. Credits for failed steps are refunded automatically.

**9. Hand over.** Share the finished video URL and the project's `appUrl`, plus one
line on the angle it tests. Suggest the natural next step: a second variation on a
different angle from the brief, since ads are tested in sets, not one at a time.

## Rules that keep the output right

- **Retiplex makes every image.** The actor portrait, subject images, scenes and the
  video come from Retiplex. Show the URLs it returns; never generate, draw or
  substitute an image yourself, because it wouldn't be the one in the ad.
- **Credits are the user's money.** Say the cost before each spending step, and
  never start the video without explicit confirmation of the estimate.
- **One project at a time.** At most three videos can render at once per account.
- **When a tool refuses with a step message,** call `get_project` and follow
  `nextTool` instead of retrying.
- **Uploaded videos** only appear in the ad through a `custom-footage` moment.
- **Local files** (Claude Code, Cursor and other agents that can run commands): use
  `create_media_upload`, PUT the file with the returned `curl`, then
  `finalize_media_upload`.

## Making variations

When the user wants more than one ad, reuse the research and make a new project per
angle. Keep the actor consistent across a test set unless the angle calls for a
different person, and change one thing at a time (hook, format or audience) so the
results tell them something.
