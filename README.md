# AI appointment setter that follows up: how CloseBot keeps quiet leads replying and puts them on your calendar

Most "AI appointment setter" searches are really about one thing. You don't have a lead problem, you have a silence problem. Someone fills in a form, replies to a DM, or picks up one message and then vanishes. A human setter follows up twice and moves on. The lead sits in the CRM until a database reactivation campaign nine months later.

So the question worth asking isn't "which AI replies fastest" — that's table stakes. It's which one keeps going after nobody replies, how many times it tries, and whether it sounds like a person or a drip sequence.

CloseBot is one of the few tools in this category where follow-up is treated as the main product rather than a feature checkbox. Here's what it actually does, what it costs, and where it stops helping you.

## What "follows up" means in practice

There are three roughly different things vendors mean by AI follow-up, and they are not equally useful:

1. **Reminder sending.** A scheduled message fires at hour 24 regardless of what the lead said. Simple, dumb, works occasionally.
2. **Drip sequences.** Multi-step templated messages on a fixed timeline. Better, but the lead can tell.
3. **Conversational re-engagement.** The agent reopens the thread, references the earlier conversation, and keeps qualifying. This is the one that actually produces bookings.

CloseBot sits mostly in the third bucket. Its agents run inside a Job Flow with objectives — collect these fields, qualify on these criteria, book this calendar — and follow-up is a property of the flow rather than a separate email tool bolted on.

## Why follow-up is where the money is

CloseBot's own documentation is blunt about it: "the money is made in the follow-up." That is a vendor statement, so treat it as such, but the underlying behaviour is easy to observe in any CRM — leads don't reply on the first touch, and the second and third attempts do most of the work.

The published numbers on this are thinner than the marketing suggests. A 2026 SetSmart study of 828,000 DM conversations found that a single well-timed follow-up more than doubled booked calls, and that more than half of all conversations died before the third message. Those are DM benchmarks, not CloseBot-specific results, but they explain why so much of the product roadmap went into follow-up sequencing.

CloseBot also claims messages that retry a failed booking add up to 20% more bookings than falling back to a "sorry, that slot is taken" reply. That's a vendor figure too. It is at least a concrete mechanism rather than a vibe.

## How CloseBot's follow-up engine actually works

Follow-up settings live per job flow, under a side tab. A brand-new flow ships with one cadence labeled **Default** that contains no follow-ups at all — which means a flow you set up and forget will never chase anyone. That's a detail worth knowing before you wonder why your agent goes quiet.

A cadence is a set of timed rules. In CloseBot's own example, if the agent sends a message and the lead doesn't reply, it messages again after 5 minutes, and again after 3 hours. The moment the lead replies, the timeline resets.

Four things make this more useful than a standard drip:

**Cadence switching.** You can build up to four cadences and instruct the Agent Node to move between them mid-conversation. The obvious pattern is No follow-up → Warm → Cold: start chasing after the lead gives you any information, stop chasing the moment they book. The cadence stays where it was switched even after the agent exits the node, so the state persists.

**Business hours.** Follow-ups respect your operating hours, so nobody gets a sales text at 2am.

**Extra prompt per step.** Each follow-up step can carry optional additional instruction, useful for changing the angle on attempt two or three. CloseBot's docs recommend leaving it blank until you've seen how the defaults perform.

**Repeating final follow-up.** You can loop the last step indefinitely — every three weeks, forever, until the cadence changes. Use that deliberately. A permanent follow-up loop into a list of cold leads is how you get spam complaints.

text
Example cadence (from CloseBot's docs)
Sent → no reply → follow up in 5 minutes
     → no reply → follow up in 3 hours
     → reply → rules reset


Note that not every node triggers follow-ups. The Agent Node does; the Statement node doesn't. CloseBot marks which nodes fire follow-up behaviour with an icon next to the node name, so you can see at a glance whether your flow is actually set up to chase.

## The Smart FAQ re-engagement trick

This is the part most competitors don't have. When your agent hits a question it can't answer, Smart FAQ logs it as an unresolved item instead of letting the model invent something. You get notified, you type the answer once, and CloseBot folds it into the knowledge library.

Then it does the interesting bit: it can go back and re-engage every lead who asked that same question, now that there's an answer. CloseBot exposes this as an API call too, which is unusual for a feature this specific.

For an agency running the same agent across twenty client accounts, that's a genuinely different behaviour from "the bot doesn't know." One unanswered question stops being a permanent hole in twenty conversations.

## Follow-up only works if the setup is right

An agent that follows up badly is worse than one that doesn't. A few things worth getting right before you turn cadences loose.

**Decide what "disqualified" means.** If your flow can't filter out the leads you don't want, follow-up just means chasing the wrong people more efficiently.

**Connect the calendar properly.** Booking happens through a booking node tied to your CRM calendar. If the calendar isn't connected, the agent can qualify and nurture but can't finish the job.

**Test before you go live.** There's a testing portal built into the flow builder. Every conversation can be run through it, and you can roll back changes or pause the AI on a specific conversation for a human takeover.

**Watch the qualification, not just the booking count.** CloseBot's own marketing asks the right question here: fifty appointments booked sounds great, but how many of those leads were any good? Booking volume without qualification criteria is a number that flatters everyone and pays nobody.

## Full plan comparison

CloseBot runs two separate pricing tracks — business and agency — plus a free tier. Prices below are from the official plans page; the business tier scales with the number of monthly AI replies you select.

| Plan | What you get | Price | Billing | Purchase |
| --- | --- | --- | --- | --- |
| **Free** | 1 agent, 1 user seat, 100 monthly messages, 1 MB storage, unlimited account connections | $0 | Always free | [Start the free plan](https://app.closebot.com/a?fpr=li87) |
| **Core (Business)** | Message costs included in base price, 15+ templates, human support, add-on users ($5/seat), add-on storage, additional agents | From $64/mo monthly; $53/mo billed as $640/yr annually | Monthly or annual | [Check Core business pricing](https://app.closebot.com/a?fpr=li87) |
| **Core (Agency)** | Unlimited agents, re-bill all costs, white-label client portal, client seats, per-message usage billed to you and rebillable | $397/mo monthly; $331/mo equivalent on annual | Monthly or annual | [See the agency plan](https://app.closebot.com/a?fpr=li87) |
| **Growth** | 50+ templates, HIPAA compliance, quarterly audits, 99.99% priority uptime, priority support | Custom quote | Contracted | [Ask about the Growth plan](https://app.closebot.com/a?fpr=li87) |

A few cost mechanics that matter more than the headline price:

- **Business plans include message costs in the base price.** If you go over your ceiling, overage is billed per message at a 2x rate, drawn from a wallet you top up.
- **Agency accounts are billed per message and can rebill it.** The plans page states a flat $0.012 per message that agencies can pass through to clients at their own markup. The help center documents $0.006 per message. Those two numbers don't match, so confirm the current rate inside your account before you build a pricing sheet around it.
- **A "message" is one segment — unless you switch on the Agent Node's unlimited potential.** With many tools and unlimited instruction size enabled, billing moves to token costs and a single message can consume several segments. Heavy agents cost more than the tier table implies.
- **Storage is metered separately.** 1 MB is included; business add-on storage runs roughly $0.10–$3.00 per MB per month depending on volume, cheaper in bulk. For context, 1 MB of text is around 1,000 pages.
- **Seats are $5 each** beyond the one included.
- **There are no refunds.** Instead you get the free tier and a 7-day trial on any paid plan before the first charge. Use the trial to test the follow-up cadences against real conversations, not the demo.

If you're signing up for the first time: CloseBot publishes one official discount, code **CLOSEBOT100OFF**, for $100 off your first payment. It applies on the plans page or under Settings → Subscription, and CloseBot explicitly says it's the only code the team maintains. Partner codes circulate, but the company won't guarantee them at checkout.

## Where an AI setter's follow-up stops short

Some honest limits, because most reviews skip them.

**CloseBot doesn't connect to Instagram or WhatsApp itself.** It connects to a CRM — GoHighLevel, HubSpot, LeadConnector, or a custom stack — and replies to the text channels already flowing into that CRM's inbox. If you don't run a CRM, you're buying two products to get one job done, and GoHighLevel starts at $97/month on its own. That changes the total cost math considerably.

**No voice.** Everything here is text. If your pipeline runs on outbound dialing, this isn't the tool.

**Billing doesn't stop when leads stop.** You pay per segment whether or not the conversation converts. That's the opposite of outcome-based models like Fin's $9.99-per-qualified-lead pricing, and it cuts the other way: for low-volume, low-ACV work, per-message is cheap; for teams where a bad month means plenty of messages and no bookings, it isn't.

**It gets expensive to be sloppy.** One G2 reviewer put it directly — if your pipeline, messaging, offer, or follow-up logic is sloppy, the AI scales that sloppiness faster. A cadence pointed at unqualified leads doesn't fix your pipeline, it just makes it louder.

**English-first.** CloseBot markets multi-language support, and the company cites 40+ languages driven by the same underlying models as Claude and ChatGPT, but its third-party listings still describe English as the supported language. If you sell in another language, test it before committing.

## Who this fits, and who it doesn't

CloseBot's follow-up engine makes most sense when a CRM is already the centre of your operation and you're either running your own pipeline or building agents for clients.

- **Marketing agencies** running GoHighLevel client accounts get the strongest fit — white-label portals, rebilling, and one agent qualifying hundreds of old leads overnight, which is roughly how one G2 reviewer described their retainer model.
- **Home services, real estate, and healthcare** get native tooling that generic bots don't have, including property data, drive-time checks, and HIPAA compliance on the Growth tier.
- **Solo coaches whose pipeline lives entirely in Instagram DMs** should probably look elsewhere first. CloseBot is the brain; your CRM is the nervous system. If you don't have the nervous system, you're paying for both.
- **Businesses automating their own sales** are served by the Core business tier, since the same agents, channels, and integrations point at your pipeline instead of a client's.

The free plan is genuinely usable for evaluating this — 100 messages a month, no card required, unlimited account connections. That's enough to build one agent, wire up a cadence, and watch whether follow-up actually moves replies on your real leads. If your volume is under 100 messages a month, it's also just free forever.

## Frequently asked questions

**Does CloseBot follow up automatically?**
Only if you configure it. New job flows ship with a Default cadence that contains no follow-ups, so a flow you build and leave alone will never chase anyone. You add timed steps under the Follow-Ups tab for each flow.

**How many times will it follow up?**
As many as your cadence specifies. You can build up to four cadences and have the Agent Node switch between them based on the conversation, and you can optionally loop the final follow-up indefinitely until the cadence changes.

**Can it stop following up after a lead books?**
Yes. That's the standard cadence-switching pattern — move to a no-follow-up cadence once a booking is confirmed. The switch persists even after the agent leaves the Agent Node.

**What happens when a lead asks something the bot can't answer?**
Smart FAQ flags it, tells the lead it doesn't have that information, and adds the question to your Knowledge Library as an unresolved item. Once you answer it, CloseBot can re-engage every lead who asked the same thing.

**Is there a free trial?**
A free-forever plan capped at 100 messages a month, plus a 7-day trial on any paid plan. There are no refunds, so the trial is where you do your testing. 👉 [Start the free plan here](https://app.closebot.com/a?fpr=li87)
