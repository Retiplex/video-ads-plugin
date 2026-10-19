# Mapping research to Retiplex

How the findings become Retiplex inputs: format, funnel stage, brief fields, actor,
moments and script.

## Formats

`list_formats` returns the current list; at the time of writing it is:

| Format | Use it when the research shows… | Watch out for |
|---|---|---|
| `problem-solution` | a sharp, recognisable pain and a clear fix. The default for most DTC products. | The pain must be real and in customer words, not invented drama. |
| `testimonial-review` | many reviews with consistent, specific experiences. | The actor speaks as a customer: use only experiences real reviews report. |
| `before-after-transformation` | a visible change (skin, space, hair, a messy desk). | Only when the change is believable and reviewers report it. Regulated in health/beauty — keep it modest. |
| `reaction-video` | a surprising first impression: taste, texture, a clever mechanism, price-to-quality. | Works best for products with a "wait, what?" moment. |
| `storytime` | a relatable situation that comes up in reviews or community threads. | Longest watch time; give it 30–45 s. The product arrives naturally, not as the hero. |
| `custom` | none of the above fits, or the angle is unusual (contrarian, comparison, founder story). | Write the whole concept into `idea`: hook, structure, proof points, tone. |

## Funnel stage

- `top-of-funnel` — cold audiences who don't know the brand. Lead with the problem
  or a story, earn attention, sell softly. Short: 15–25 s.
- `mid-funnel` — they know the problem; show why this product works (mechanism,
  proof, comparison with what they tried). 20–35 s.
- `bottom-funnel` — retargeting people who visited; handle the main objection and
  push the offer (guarantee, discount, bundle). 15–30 s.

If the user doesn't say, assume top-of-funnel: most people asking for a UGC ad want
new customers.

## `set_creative_brief` fields

Fill them all from the research. Anything left out gets deduced from the product page
(which costs credits and loses your research).

- `target_audience` — specific person and situation, e.g. "Women 30–45 with
  sensitive skin who've tried drugstore moisturisers that sting, shopping online
  after a dermatologist told them to simplify their routine." Not "women 18–65".
- `benefit_to_highlight` — one benefit, in customer language, with its proof.
  E.g. "No more stinging — 1,200+ reviews mention it's gentle enough for eczema-prone
  skin."
- `primary_ad_goal` — the action: "Shop now", "Try it risk-free (60-day guarantee)",
  "Download the app".
- `funnel_stage` — as above.
- `idea` (`custom` only) — the full concept in prose: who's speaking, the hook line,
  the beats in order, the proof to use, the tone, how it ends.

## Actor (`set_actor`)

The archetype should look like a believable customer for the market the ad runs in.
Base it on evidence: who leaves the reviews, who appears in the brand's own content,
who the user says their customers are. When the evidence is thin, leave `archetype`
out and let Retiplex cast for the audience, or ask the user.

- `age` and `sex`: from the audience, not the stereotype of the category.
- `clothing`: matches the setting and voice (gym top for fitness, knit sweater for a
  cosy home product, work shirt for a B2B tool). Upper body only.
- `hairstyle` / `facialFeatures`: natural and everyday for UGC; avoid model-polished
  looks unless the brand is premium beauty.
- `language` / `language_accent`: the market's language, set before the brief and
  script if the project wasn't created with it.

## Moments (`add_moment`)

Optional. Use one when the research points to a visual that sells:

- `product-shot-faceless` — close look: texture, label, detail reviewers mention.
- `product-action` — the product moving or working, from its photos.
- `product-shot` — the actor holding or using it.
- `b-roll` — the setting or mood (a messy bathroom shelf for the "before").
- `custom-footage` — the brand's own clip, required for any uploaded video to appear.

Tag who's in frame with `@[Name]` from `get_project`'s `cast`. Add a short
`text_overlay` with the proof (e.g. "4.8★ from 3,000+ reviews") only if that proof is
real. Keep the total within the script's length.

## Script

Spoken UGC runs about 2.3–2.7 words per second in English (slower for languages with
longer words). A 20 s ad is roughly 45–55 words.

What a good script built from research has:

- **Hook (first sentence):** the angle's hook, specific and spoken naturally.
- **Body:** the problem or story in customer language, the failed alternative, then
  the product with one mechanism or proof point. One benefit, not five.
- **Close:** the offer and the action from `primary_ad_goal`, said like a person,
  not a banner ("they do a 60-day refund, so honestly just try it").

Remove anything that sounds like ad copy ("revolutionary", "game-changer",
"elevate your routine"), any number you can't source, and any experience no
reviewer reported. Write in the project's spoken language; don't translate an
English draft word for word.
