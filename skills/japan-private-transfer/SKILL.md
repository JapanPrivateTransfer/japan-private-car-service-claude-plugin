---
name: japan-private-transfer
description: Plan and book private car transfers in Japan — airport pickups and drop-offs (KIX, Haneda, Narita, Chubu, New Chitose, Fukuoka and more), city-to-city rides, hourly chauffeur charters, and day tours. Use when the user mentions traveling in Japan with a driver, needs an airport transfer, asks how many people and bags fit in which vehicle, wants a price for a Japan route, or asks about booking, paying for, or checking a Japan private car reservation.
---

# Japan Private Car Service workflow

The connector tools call the live Japan Private Car Service (JPT) platform. Prices,
vehicle capacities, and availability come from the system — never estimate,
calculate, or invent them yourself.

## Core sequence

1. **Find places** — call `search_locations` with the user's own words
   (airport name, hotel, station, landmark). For hotels or street addresses,
   use action `autocomplete`, let the user pick a suggestion, then action
   `details` and copy `quote_location` verbatim into pickup/dropoff.
2. **Check the route** — call `search_routes` with origin and destination to
   confirm the pair is served.
3. **Size the vehicle** — call `recommend_vehicle` with passenger and luggage
   counts. Present at most three options ranked by fit; do not push oversized
   vehicles. State each option's passenger and luggage capacity when showing it.
4. **Quote for real** — call `create_quote` with exact date, time, locations,
   passengers, and luggage. For side-by-side comparison pass
   `compare_vehicle_ids` with the 1–3 recommended IDs. Only prices returned by
   `create_quote` / `get_quote` are authoritative; reference prices are
   estimates and must be labeled as such.
5. **Flight-aware airport timing** — for airport trips with a flight number,
   call `get_flight_guidance` first and use its pickup-time advice. If the
   lookup is unavailable, say so and keep the user's stated time.
6. **Optional stops** — for charters and day tours, call `search_attractions`
   to offer eligible stops; prices always recalculate through `create_quote`.
7. **Book** — after the user picks one option, collect name, email, and an
   international phone number (plus flight details for airport trips), then
   call `create_booking_draft`.
8. **Pay** — only after the user explicitly accepts the terms and cancellation
   policy, call `create_payment_request`. It returns an official Stripe
   Checkout link from the JPT Payment Engine. Never ask for card numbers, never
   construct payment links yourself.
9. **Confirm** — call `get_order_status` to read payment and booking state.
   Only that tool confirms payment; a browser redirect never does.

## Rules that protect the customer

- Answer in the user's language. Keep replies short and plain.
- Never invent prices, distances, travel times, discounts, or availability.
- A quote can expire or need review; if the system says so, pass that on
  honestly and offer the next step.
- One quote = one vehicle option. When comparing, make clear the customer
  chooses one — the other options are not a fleet being reserved.
- Changes to an existing quote go through `modify_quote` with explicit
  customer confirmation; never silently switch vehicles.
- Cancellations, refunds, and special assistance are handled by human support:
  booking@japanprivatetransfer.com.

## Coverage snapshot

Major airports (Kansai, Osaka Itami, Haneda, Narita, Chubu Centrair, New
Chitose, Fukuoka, Naha and more) and city-to-city routes across Japan, with
sedan, Toyota Alphard-class MPV, Toyota HiAce-class van, and minibus/coach
options sized by the system's live capacity data.
