# Japan Private Car Service — Claude plugin

Book private car travel across Japan without leaving the conversation. This plugin connects Claude to the live Japan Private Car Service platform: airport pickups and drop-offs, city-to-city transfers, hourly chauffeur charters, and day tours.

Claude finds exact pickup and drop-off points, recommends the right vehicle for your passenger and luggage count, returns authoritative prices from the JPT Quote Engine, prepares a booking draft, and hands payment off to the official Stripe Checkout. Order status is read back from the same system, so what you see is what the operations team sees.

## What's inside

- **Connector** — remote MCP server at `https://japanprivatecarservice.com/api/v1/agent/mcp` (OAuth 2.0 sign-in on first use)
- **Skill** — `japan-private-transfer`, teaching Claude the correct search → recommend → quote → book → pay → confirm sequence and the safety rules around pricing and availability

## Example prompts

- "I need a private transfer from Kansai Airport to Kyoto tomorrow at 10 am, 3 people with 4 suitcases — what are my options?"
- "8 个人从新千岁机场去二世谷，行李 10 件，选什么车？多少钱？"
- "Book the Alphard option and send me the checkout link."
- "Has my payment gone through?"

## Privacy and payments

Trip details are used only to quote and book your ride. Card numbers never touch Claude or this plugin — payment happens on Stripe Checkout pages created by the JPT Payment Engine. See https://japanprivatecarservice.com/en/policies/privacy.

## Support

booking@japanprivatetransfer.com · https://japanprivatecarservice.com/en/policies/faq
