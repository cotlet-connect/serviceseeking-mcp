# ServiceSeeking API integration and MCP server: connect ServiceSeeking to ChatGPT, Claude and any AI agent

**ServiceSeeking API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your ServiceSeeking account.** ServiceSeeking has no public API, no official MCP server and no app in ChatGPT or Claude for businesses. Cotlet.dev connects your own signed-in ServiceSeeking account to any AI agent, so you ask in plain words and the AI quotes leads and replies to customers for you.

## What you can ask

> List today's new ServiceSeeking leads in my area and draft a quote message for each. Send nothing yet.

> Which ServiceSeeking quotes from this week have no reply? Draft a follow-up for each.

> Show the customer chats waiting for my reply and draft an answer to each.

> How many credits did I spend on ServiceSeeking leads this month, and how many quotes did I win?

> Flag new leads outside my trade or my suburbs before I spend credits on them.

## What tradies ask their AI to do in ServiceSeeking

From tradies' posts on Reddit, Facebook groups, review sites and the ServiceSeeking help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Quote new leads fast | "List today's new ServiceSeeking leads in my area and draft a quote message for each. Send nothing yet." | When a lead comes in |
| Follow up open quotes | "Which quotes I sent this week have no reply? Draft a short follow-up for each." | Weekly |
| Answer customer chats | "Show customer chats waiting for my reply and draft an answer to each." | Daily |
| Credits and lead spend | "How many credits did I spend on ServiceSeeking leads this month, and how many quotes did I win?" | Monthly |
| Skip the wrong jobs | "Flag new leads outside my trade or my suburbs so I do not spend credits on them." | Daily |
| Keep the profile fresh | "Add my last 5 finished jobs with photos to my ServiceSeeking profile after my OK." | Monthly |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## ServiceSeeking API

ServiceSeeking has no API, webhooks or data export for a business account (checked 25 Nov 2026). With Cotlet.dev you build your own ServiceSeeking API: show your AI agent a job once in your signed-in ServiceSeeking account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/serviceseeking

Endpoints you can build in a few minutes (none of them exists until you build it):

- `new-leads`: Time, job, suburb and whether you quoted
- `quotes-without-reply`: Lead, amount and days since you sent it
- `unanswered-chats`: Customer, job and last message
- `quotes-won`: Quotes sent and won for the period you name
- `credits-spent`: Credits used and quotes won for the period you name
- `wrong-area-leads`: New leads outside your trade or suburbs
- `send-quote`: A quote with your price and message, after your OK

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/new-leads`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its ServiceSeeking tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"serviceseeking":{"command":"uvx","args":["serviceseeking-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t serviceseeking-mcp .
docker run -i --rm -e COTLET_API_KEY serviceseeking-mcp
```

Tools: `serviceseeking_guide`, `serviceseeking_status`, `serviceseeking_open`, `serviceseeking_read`, `serviceseeking_click`, `serviceseeking_type`, `serviceseeking_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install serviceseeking-mcp        # or run it once: uvx serviceseeking-mcp connect
serviceseeking-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 serviceseeking_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 serviceseeking_agent.py status    # is ServiceSeeking ready in your Cotlet.dev account
python3 serviceseeking_agent.py guide     # ServiceSeeking screens and sign-in link
python3 serviceseeking_agent.py read      # open ServiceSeeking, read the page, close the browser
python3 serviceseeking_agent.py tools     # list Cotlet.dev tools
python3 serviceseeking_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ServiceSeeking users

**Does ServiceSeeking have an API?**

ServiceSeeking has no public API for business members (checked 25 Nov 2026). Leads, quotes and customer chats live in its app and site. Cotlet.dev works in your own signed-in ServiceSeeking account, so your AI agent can read job leads, send quotes and reply to chats.

**How do I connect ServiceSeeking to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to ServiceSeeking once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no ServiceSeeking API key.

**Is there a way to use Claude with ServiceSeeking?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own ServiceSeeking account.

**Is there a ServiceSeeking MCP server?**

ServiceSeeking has no official MCP server. Cotlet.dev is a hosted MCP server that works in ServiceSeeking for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can AI follow up my ServiceSeeking quotes?**

Yes. Your AI lists quotes with no reply, drafts a follow-up for each and sends it in your ServiceSeeking chats after your OK.

**How do I know if ServiceSeeking leads pay off?**

ServiceSeeking has no export of your leads or quotes (checked 25 Nov 2026). Your AI reads your leads, quotes and credits in your account and sums spend and quotes won for the month.

## Find it on

- [ServiceSeeking on Cotlet.dev](https://cotlet.dev/integrations/mcp/serviceseeking?utm_source=github&utm_medium=readme&utm_campaign=serviceseeking&utm_content=find)
- [PyPI package serviceseeking-mcp](https://pypi.org/project/serviceseeking-mcp/)
- [Odel](https://odel.app/modules/cotlet/serviceseeking)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/serviceseeking/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-serviceseeking-to-ai-agents-guide)

## Also works with

- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/serviceseeking?utm_source=github&utm_medium=readme&utm_campaign=serviceseeking

Start free: https://cotlet.dev/signup?app=serviceseeking&utm_source=github&utm_medium=readme&utm_campaign=serviceseeking
